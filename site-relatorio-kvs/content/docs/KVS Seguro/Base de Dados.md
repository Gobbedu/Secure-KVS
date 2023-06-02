# Base de Dados

## Key Value Store - KVS

Um sistema KVS (Key-Value Store), ou Armazenamento Chave-Valor, é um tipo de banco de dados que armazena e recupera dados usando um modelo simples de pares de chave e valor. Nesse tipo de sistema, cada dado é associado a uma chave única, e essas chaves são usadas para recuperar os valores correspondentes. Um KVS não impõe nenhum esquema ou estrutura de dados específica, permitindo armazenar qualquer tipo de dado, como strings, números, objetos serializados, entre outros.

{{< hint info >}}
Esta definição se encaixa perfeitamente com a utilização de dicionários em python, logo esta estrutura foi utilizada como base para realizar todas as operações CRUD na base de dados.
{{< /hint >}}

## Arquivo

Utilizar um `dict` em python como base de dados não é difícil, basta ter uma variável deste tipo. O problema é manter o estado desta base de dados ao desligar o servidor. Para resolver esse problema, foi utilizada a biblioteca python `pickle`, permitindo salvar um objeto de python em um arquivo .pkl, permitindo escrever e carregar este arquivo quando o servidor é ligado ou desligado.

As funcionalidades do CRUD da base de dados foram implementadas como a classe `KeyValueStore`, e abaixo temos algumas partes importantes da classe destacadas. Sendo uma instância de `KeyValueStore` criada pela classe `Server`.

```py
class KeyValueStore:
    def __init__(self, filename):
        self.filename = filename
        self.data = self._load_data()

    def _load_data(self):
        try:
            with open(self.filename, 'rb') as file:
                return pickle.load(file)
        except FileNotFoundError:
            return {}

    def _save_data(self):
        with open(self.filename, 'wb') as file:
            pickle.dump(self.data, file)

    def _push_data(self, data:dict):
        with open(self.filename, 'wb') as file:
            for key, value in data.items():
                self.create(key, value)
            self._save_data()

# [...]
```

## Uso

Como ao passar comandos e valores para o server é necessário transformar os comandos em uma string em python, foi implementado uma função wrapper que avalia o conteúdo da string de acordo com os padrões presentes em python. De modo que ao inserir valores não interpretáveis em python, uma mensagem de erro semelhante a que receberia em python será apresentada, como demonstrado abaixo:

```txt
(KVS) show
{1: 1, 2: 2, 3: 3, 'joao': 1, 4: 4}
(KVS) update "joao" 2
Ok
(KVS) showw
*** Unknown syntax: showw
(KVS) show
{1: 1, 2: 2, 3: 3, 'joao': 2, 4: 4}
(KVS) create 'Maria" 3
Não entendi o comando - "create 'Maria" 3"
Erro: unterminated string literal (detected at line 1) (<string>, line 1)
(KVS) create "';;''l 3
Não entendi o comando - "create "';;''l 3"
Erro: unterminated string literal (detected at line 1) (<string>, line 1)
(KVS) create ;;l 3
Não entendi o comando - "create ;;l 3"
Erro: invalid syntax (<string>, line 1)
(KVS)

```
