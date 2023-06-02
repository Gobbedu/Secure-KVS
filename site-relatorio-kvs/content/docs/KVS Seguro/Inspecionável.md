# Socket Inspecionável

{{< hint info >}}
Esta página explica como funciona o código presente em `inspectable.py` encontrado [neste repositório](https://gitlab.com/fer22f/leilaloes/-/blob/main/inspectable/inspectable.py) e utilizado durante a prova de **Sigilo** e **Integridade** do sistema implementado.
{{< /hint >}}

## MemoryBIO

A documentação para os detalhes da biblioteca utilizada podem ser encontrados na página do ssl abaixo:

- https://docs.python.org/3/library/ssl.html#memory-bio-support

Nela é descrito que o objeto ssl (**SSLObject**) foi criado como uma versão de mais baixo nível em relação ao SSLSocket com o intuito de realizar operações de Input e Output (IO) assíncronas. Sendo possível o acesso do estado de uma conexão ssl **sem nenhuma operação de _Network IO_** implementada. Por este motivo esta biblioteca não possui algumas coisas que são garantidas no SSLSocket, como por exemplo o `do_handshake_on_connection`, o `recv()` e o `send()`. os quais devem ser implementados manualmente.

Um detalhe importante é que todo IO em um SSLObject é não bloqueante. Por exemplo, `ssl_object.read()` vai **sempre** ativar um erro de `SSLWantReadError`. Com isto em mente podemos entender o motivo pelo qual o código implementado tanto para o do_handshake abaixo quanto para as outras operações possuem tantos blocos try-catch.

{{< details title="Handshake classe InspectableSocket" open=true >}}

```py
def do_handshake(self):
    # The do_handshake method should be repeatedly called until
    # the handshake is complete. When it needs to write or read,
    # it raises their respective Errors. As per documentation,
    # before writing, we should check if there is something to read,
    # and before reading, we should check if there is something to write.
    while True:
        try:
            self.ssl_object.do_handshake()
            break
        except SSLWantWriteError:
            if self.handle_incoming() == -1:
                raise SSLEOFError()
            self.handle_outgoing()
        except SSLWantReadError:
            self.handle_outgoing()
            if self.handle_incoming() == -1:
                raise SSLEOFError()
    # Important to make sure that the handshake is completed
    self.handle_outgoing()
    print(f"Cipher: {self.ssl_object.cipher()}")
```

{{< /details >}}

Mas não queremos fazer uma aplicação assíncrona, queremos visualizar os dados cifrados e editar um byte da mensagem para provar o **Sigilo** e a **Integridade** do protocolo. Para tal foi utilizada outra caracteristica importante do SSLObject - Existem dois buffers vinculados com o tráfego de dados entre Python e a instância do protocolo SSL:

- incoming : De Python para SSL
- outgoing : De SSL para Python

Com estes dois buffers em mãos podemos acrescentar ao protocolo padrão a lógica necessária para visualizar e alterar os bytes que estão sendo **enviados** ao outro lado da conexão - seja ela o cliente ou o servidor.

{{< details title="Network IO classe InspectableSocket" open=true >}}

```py
def handle_incoming(self, n_bytes=1024):
    try:
        data = self.socket.recv(n_bytes)
    except BlockingIOError:
        # Avoids a busy loop that consumes 100% of CPU resources
        time.sleep(0.01)
        return 0

    if len(data) == 0:
        # When recv returns an empty string, the socket has been closed
        return -1
    return self.incoming.write(data)

def handle_outgoing(self):
    data = self.outgoing.read()
    if len(data) == 0:
        return
    print("========== send:  ssl ==========")
    hexdump(data)
    # We use sendall instead of send to make sure all data is sent
    self.socket.sendall(self.edit_data(data))

def recv(self, n_bytes):
    while True:
        try:
            data = self.ssl_object.read(n_bytes)
            return data
        except SSLWantReadError:
            # This may happen if the SSL message has been chunked
            if self.handle_incoming() == -1:
                return b""

def send(self, data):
    print("======== send: original ========")
    hexdump(data)
    # There is no loop to send here; since TLS writes into a buffer
    # that resizes automatically, it is never expected to fail.
    self.ssl_object.write(data)
    self.handle_outgoing()
```

{{< /details >}}

Finalmente, agora podemos visualizar uma mensagem antes e depois dela ser enviada para o SSL, como demonstrado abaixo:

```txt
######## Cliente ########
(KVS) create 'hello' 'world!'
======== send: original ========
00000000: 63 72 65 61 74 65 20 27  68 65 6c 6c 6f 27 20 27  create 'hello' '
00000010: 77 6f 72 6c 64 21 27                              world!'
========== send:  ssl ==========
00000000: 17 03 03 00 28 df fa 6d  53 9c cf 8a b1 b9 1d 13  ....(..mS.......
00000010: 2a 0c 62 da ca 90 6c 0c  3f e9 fd d1 b3 88 48 8a  *.b...l.?.....H.
00000020: 5e 42 60 af 4f fa 45 e9  fb f0 ff 6a 27           ^B`.O.E....j'
Ok
(KVS) update 'hello' 'World!!!'
======== send: original ========
00000000: 75 70 64 61 74 65 20 27  68 65 6c 6c 6f 27 20 27  update 'hello' '
00000010: 57 6f 72 6c 64 21 21 21  27                       World!!!'
========== send:  ssl ==========
00000000: 17 03 03 00 2a ef 1b 90  d1 3f e2 57 9b b2 f2 69  ....*....?.W...i
00000010: 7a 48 38 5a b8 fb fd bd  bd f7 ec 83 2a 31 0d a1  zH8Z........*1..
00000020: db 83 f0 77 b9 a1 e5 10  3b 7e de 51 43 44 8d     ...w....;~.QCD.
Ok
(KVS) show
======== send: original ========
00000000: 73 68 6f 77                                       show
========== send:  ssl ==========
00000000: 17 03 03 00 15 39 3e 1b  b1 8f 00 5e 44 25 ce a0  .....9>....^D%..
00000010: 02 f0 62 dc b4 74 96 b3  ab e6                    ..b..t....
{'hello': 'World!!!'}
(KVS)

######## Servidor ########
======== send: original ========
00000000: 4f 6b                                             Ok
========== send:  ssl ==========
00000000: 17 03 03 00 13 50 0d 78  b4 9c 11 87 8e 39 57 97  .....P.x.....9W.
00000010: 6c c8 a6 c5 63 7c 5b 3e                           l...c|[>
======== send: original ========
00000000: 4f 6b                                             Ok
========== send:  ssl ==========
00000000: 17 03 03 00 13 cc 27 95  e9 ec aa 73 fb 9b 0e 1f  ......'....s....
00000010: f1 df 7e 47 fa ca 43 ad                           ..~G..C.
======== send: original ========
00000000: 7b 27 68 65 6c 6c 6f 27  3a 20 27 57 6f 72 6c 64  {'hello': 'World
00000010: 21 21 21 27 7d                                    !!!'}
========== send:  ssl ==========
00000000: 17 03 03 00 26 76 28 56  ab 55 95 1b 9d 14 54 8a  ....&v(V.U....T.
00000010: 69 9d 81 ab 2f 0f 0b a4  ca 09 2f 1c d6 ae 3e 7f  i.../...../...>
00000020: f9 d7 12 5e 05 33 a9 81  12 c1 0c                 ...^.3.....

```
