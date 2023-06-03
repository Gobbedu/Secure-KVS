---
title: Introdução
type: docs
---

# Relatório de Redes III

<!-- {{< figure src="ufpr.png" >}} -->

**Universidade Federal do Paraná UFPR 2023-2**

## Descrição do Trabalho

{{< hint info >}}
A dupla vai fazer uma aplicação cliente/servidor simples baseada em KVS (Key-Value Store). O servidor mantém uma base de dados, que o cliente pode consultar, alterar... A comunicação cliente/servidor é segura: garantindo sigilo, autenticidade e integridade. Cada uma destas propriedades deve ser demonstrada:
{{< /hint >}}

- **Sigilo**: mostre os dados criptografados transmitidos (imprimindo junto a versão original)
- **Autenticidade**: um invasor tenta comunicar com o servidor ou com o cliente, mostre que não consegue
- **Integridade**: troque o valor de alguns bits da mensagem e mostre que não descriptografa corretamente

Cada dupla pode escolher a linguagem que quiser para implementar o trabalho. Sugestão: Python, podem usar C, C++, Java, outras linguagens são bem vindas.

{{< hint warning >}}
**Foi utilizado Python**
{{< /hint >}}

Desta vez o relatório vai ser especial: explique como programar um sistema cliente-servidor seguro usando TLS na linguagem da sua escolha. Formato sugerido: vídeo!

- Se não quiser: apresentação;
- se não quiser: página Web com relatório em texto.

Divulgue na Internet, nas redes sociais, no YouTube, faça deste uma contribuição do seu currículo!

## Relatório

Cada dupla deve fazer uma página Web contendo:

1. O relatório: por exemplo link para o vídeo no YouTube Ou a apresentação, ou o relatório em texto mesmo
2. Logs de execução: se no vídeo você estiver confiante que demonstrou seu sistema bem – não precisa de logs na página!
3. Código comentado, pense no usuário que viu seu vídeo e vai acessar o código para aprender

O relatório deve ser manuscrito, e enviado por email **até 3/junho/2023** para eliasufpr at gmail.com com o assunto: "TopRedes 2023: KVS Seguro"

{{< hint warning >}}
**Relatório escolhido:**

- Pagina Web com relatório em texto.
  {{< /hint>}}

Os logs de execução também podem ser acessados no menu lateral.

## Código Fonte

Para este trabalho foram utilizadas bibliotecas em python e código aberto para visualizações auxiliares. Principalmente, a criação de sockets seguros com _Transport Layer Security_ TLS foi utilizada a biblioteca de python **ssl**.

Outra biblioteca utilizada para fazer a interface do cliente foi a **cmd**, podendo criar uma interface semelhante a uma shell, mas com comandos definidos para o trabalho.

Para visualizar e editar um byte na mensagem cifrada do cliente foi utilizado um arquivo auxiliar de um veterano (lendário) do curso, sendo encontrado no link abaixo. https://gitlab.com/fer22f/leilaloes/-/blob/main/inspectable/inspectable.py

</br>

{{< hint warning >}}
**Código**

Todo o trabalho está salvo e pode também ser acessado neste [repositório](https://github.com/Gobbedu/SecureKVS) do Github.

- Partes importantes do código também podem ser acessados pelo menu lateral.
{{< /hint >}}

  </br>

## Mais informações

O código também pode ser visualizado utilizando as barras no menu lateral.
Para o site foi utilizado a framework [Hugo](https://gohugo.io/) em conjunto com [este tema](https://themes.gohugo.io/themes/hugo-book/) da galeria disponível. O botão de trocar tema foi adicionado baseado [neste site](https://yonkov.github.io/post/add-dark-mode-toggle-to-hugo/).

## Autores

- Eduardo Gobbo Willi Vasconcellos Gonçalves, ([Github](https://github.com/Gobbedu))
- Anderson Aparecido do Carmo Frasão, ([Github](https://github.com/carmofrasao))
