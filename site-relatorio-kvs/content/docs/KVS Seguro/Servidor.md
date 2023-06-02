# Servidor KVS

{{< hint info >}}
Responsável por receber comandos CRUD de um cliente autenticado e executá-los no banco de dados, enviando o resultado das operações de volta para o cliente. **Tem** acesso ao banco de dados, podendo se comunicar com um cliente somente uma vez.
{{< /hint>}}

## Execução

Para inicializar o servidor basta executar

```shell
python server.py
```

A partir de então o servidor fica na escuta por conexões e ao receber uma, executa as operações solicitadas até que o comando `exit` seja enviado, ou que o processo seja terminado com um `Ctrl + C`.

{{< details title="Introdução servidor" open=true >}}

```txt
Aguardando conexões...
Conexão estabelecida com ('127.0.0.1', 54120)
```

{{< /details >}}

## Kernel

Existem três partes para o kernel do servidor, onde cada uma serve um propósito diferente ao longo da execução e testes no TLS.

## Kernel Server

O primeiro kernel é a base para um server genérico, a classe `Server`, que apenas carrega os certificados e fica na espera de uma conexão segura com certificado válido. Deixando a implementação da lógica de interação com o cliente para depois. Novamente esta implementação aceita apenas uma conexão de um cliente.

{{< details title="classe Server" open=true >}}

```py
class Server:
    def __init__(self):
        self.listener_socket = None
        self.client_socket = None
        self.client_address = None

        context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
        context.load_cert_chain(CERT_FILE, PKEY_FILE)
        self.context = context

    def run(self):
        self.listener_socket = self.create_socket()
        print('Aguardando conexões...')

        try:
            self.client_socket, self.client_address = self.listener_socket.accept()
            print('Conexão estabelecida com', self.client_address)

            self.handle_client()
        except ssl.SSLEOFError:
            print("Cliente não finalizou handshake")

        self.listener_socket.close()

    def create_simple_socket(self):
        return create_server(("", PORT))

    def create_socket(self):
        socket = self.create_simple_socket()
        return self.context.wrap_socket(socket, server_side=True)

    def handle_client(self):
        # herdar e implementar interação com o cliente aqui
        pass
```

{{< /details >}}

## Kernel ServerKVS

O segundo kernel `ServerKVS` herda da classe `Server`, onde a lógica de conexão já foi definida. Aqui é implementada a lógica específica de como será a interação com o cliente, ou seja, como será implementada a recepção, execução e retorno de comandos **CRUD** no sistema de Key Value Store, que envolve a interação direta com a base de dados, a qual pode ser vista em mais detalhes no menu lateral Base de Dados.

{{< details title="classe ServerKVS" open=true >}}

```py
class ServerKVS(Server):
    def __init__(self):
        super().__init__()

        self.database = KeyValueStore(DB_FILE)

    def handle_client(self):
        while True:

            # Recebe a requisição do cliente
            request = self.client_socket.recv(1024).decode()

            if request == 'exit': break

            # Processa a requisição
            response = self.process_request(request)

            # Envia a resposta ao cliente
            self.client_socket.send(response.encode())

        # Encerra a conexão com o cliente
        self.listener_socket.shutdown(SHUT_RDWR)
        self.listener_socket.close()

    def process_request(self, request):
        args = request.split()
        comm = args[0]

        if comm not in dir(KeyValueStore):
            return f"'{comm}' não foi interpretado pelo server"

        foo = getattr(self.database, comm)

        try:
            resp = str(foo(*args[1:]))
        except Exception as e:
            resp = f'Não entendi o comando - "{request}"\nErro: {str(e)}'

        return resp
```

{{< /details >}}

## Kernel ServerAtacavel

E finalmente o terceiro kernel herda da classe `ServerKVS` e redefine a lógica de conexão. Essa nova classe `ServerAtacavel` não vai atacar as mensagens propriamente ditas, o responsável por isso é o cliente. Neste caso, esta classe foi implementada somente para mostrar as mensagens abertas e cifradas sendo enviadas ao cliente, nada mais.

Para isso é necessário implementar o socket seguro de forma completamente diferente, onde precisamos de dois buffers que guardem as mensagens sendo recebidas pelo socket ssl (**incoming**) e as mensagens a serem enviadas pelo socket ssl (**outgoing**).

A lógica mencionada foi preferida ao invés de utilizar `scapy` ou `pyshark` pelo fato de não precisar lidar com sudo e a captura de pacotes das portas envolvidas (que envolve **sudo**), sendo então utilizado um código auxiliar que pode ser encontrado [neste repositório](https://gitlab.com/fer22f/leilaloes/-/blob/main/inspectable/inspectable.py), onde a classe **InspectableListenerSocket** foi implementada.

{{< details title="classe ClienteAtacavel" open=true >}}

```py
class ServerAtacavel(ServerKVS):
    def create_socket(self):
        socket = self.create_simple_socket()
        return InspectableListenerSocket(socket, self.context, server_side=True)
```

{{< /details >}}
