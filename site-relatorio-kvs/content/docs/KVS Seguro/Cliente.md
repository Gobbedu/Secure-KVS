# Cliente KVS

{{< hint info >}}
Responsável por enviar comandos CRUD ao servidor e mostrar o resultado de suas operações. **Não** possui acesso ao banco de dados, apenas se comunicando com o servidor.
{{< /hint>}}

## Execução

Com o servidor rodando e após executar:

```shell
python client.py
```

Podemos acessar a interface `cmd.Cmd` e interagir com o Key Value Store (KVS), onde a mensagem de boas vindas pode ser vista abaixo:

{{< details title="Introdução cliente" open=false >}}

```txt
Bem-vindo ao shell do Key-Value Store. Digite 'help' para listar os comandos disponíveis.
(KVS) help

Comandos disponíveis:
  create    : Cria um novo par chave-valor. Sintaxe: create <chave> <valor>
  delete    : Exclui um par chave-valor. Sintaxe: delete <chave>
  exit      : Sai do shell. Sintaxe: exit
  help      : Mostra comandos disponíveis e como usá-los.
  keys      : Obtém todas as chaves do Key-Value Store. Sintaxe: keys
  read      : Lê o valor de uma chave. Sintaxe: read <chave>
  show      : Mostra toda a Key-Value Store. Sintaxe: show
  update    : Atualiza o valor de uma chave existente. Sintaxe: update <chave> <valor>
  values    : Obtém todos os valores do Key-Value Store. Sintaxe: values

(KVS) show
{1: 1, 2: 2, 3: 4, 'joao': 2}
(KVS) update 'joao' 1
Ok
(KVS) show
{1: 1, 2: 2, 3: 4, 'joao': 1}
(KVS) create 5 6
Ok
(KVS) show
{1: 1, 2: 2, 3: 4, 'joao': 1, 5: 6}
(KVS) exit
```

{{< /details >}}

## Kernel

Existem três partes para o kernel do cliente, onde cada uma serve um propósito diferente ao longo da execução e testes do lado do cliente.

## Kernel Client

O primeiro kernel é a base para um cliente genérico `Client`, que apenas cria uma conexão segura com os valores presentes em `config.py`, deixando a implementação da lógica de interação com o servidor para depois.

{{< details title="classe Client" open=true >}}

```py
class Client:
    def __init__(self, authenticate=True):
        self.hostname = HOSTNAME
        self.authenticate = authenticate
        self.certificate = CERT_FILE if authenticate else FAKE_CERT

        context = SSLContext(PROTOCOL_TLS_CLIENT)
        context.load_verify_locations(self.certificate)
        self.context = context

    def create_simple_socket(self):
        return create_connection(SERVER_ADDRESS)

    def create_socket(self):
        socket = self.create_simple_socket()
        return self.context.wrap_socket(socket, server_hostname=self.hostname)

    def run(self):
        # herdar e implementar interação com o server aqui
        pass
```

{{< /details >}}

## Kernel ClientKVS

O Segundo kernel herda da classe `Client`, onde a lógica de conexão já foi definida. Nesta classe `ClientKVS` é implementada a lógica específica de como será a interação com o servidor, ou seja como será implementado o **CRUD** do sistema key value store, que implica apenas chamar a função que implementa a shell `cmd.Cmd` e enviar as mensagens para o cliente.

{{< details title="classe ClientKVS" open=true >}}

```py
class ClientKVS(Client):
    def run(self):
        KeyValueStoreShell(self.create_socket()).cmdloop()
```

{{< /details >}}

Para essa classe ClientKVS apenas definimos que o comportamento do cliente será executar a interface `cmd.Cmd` codificada, onde temos as partes relevantes do seu kernel (omitindo o código de interface) ressaltadas abaixo:

{{< details title="classe Shell" open=false >}}

```py
class KeyValueStoreShell(cmd.Cmd):
    intro = "Bem-vindo ao shell do Key-Value Store. Digite 'help' para listar os comandos disponíveis."
    prompt = "(KVS) "

    # Configurações do cliente
    def __init__(self, socket):
        super().__init__()

        # um socket configurado para enviar e receber packets
        self.secure_socket = socket

    # [...]

    def _send_query(self, query):
        # Envia a requisição ao servidor
        self.secure_socket.send(query.encode())

        # Recebe a resposta do servidor
        return self.secure_socket.recv(1024).decode()
```

{{< /details >}}

## Kernel ClienteAtacavel

E finalmente o terceiro kernel herda da classe `ClientKVS` e re-define a lógica de conexão. Essa nova classe `ClientAtacavel` vai emular um _bad actor_ tentando mexer nas mensagens criptografadas do cliente. Esta funcionalidade é demostrada no Log presente no menu lateral e serve para provar que o protocolo TLS tem Integridade.

Para isso é necessário implementar o socket seguro de forma completamente diferente, onde precisamos de dois buffers que guardem as mensagens sendo recebidas pelo socket ssl (**incoming**) e as mensagens a serem enviadas pelo socket ssl (**outgoing**).

A lógica mencionada foi preferida ao invés de utilizar `scapy` ou `pyshark` pelo fato de não precisar lidar com sudo e a captura de pacotes das portas envolvidas (que envolve **sudo**). Sendo então utilizado um código auxiliar que pode ser encontrado [neste repositório](https://gitlab.com/fer22f/leilaloes/-/blob/main/inspectable/inspectable.py), onde a classe **InspectableSocket** foi implementada.

{{< details title="classe ClienteAtacavel" open=true >}}

```py
class ClientAtacavel(ClientKVS):
    def __init__(self, allow_editing=False):
        super().__init__()
        self.allow_editing = allow_editing

    def create_socket(self):
        socket = self.create_simple_socket()
        socket = InspectableSocket(
            socket,
            self.context,
            allow_editing=self.allow_editing,
            server_hostname=self.hostname,
        )
        socket.do_handshake()
        return socket
```

{{< /details >}}
