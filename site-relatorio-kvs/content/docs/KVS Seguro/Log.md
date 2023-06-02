---
title: "Log"
date: 2023-02-23T10:18:19-03:00
---

# Arquivo Log

{{< hint info >}}
Abaixo temos o Log de execução tanto do **servidor** quanto do **cliente**, para a execução com as flags `--show` e/ou `--edit` para cada um deles.
{{< /hint >}}

## Client \-\-show

Da mesma forma, com o servidor rodando, ao executar `python client.py --show` podemos visualizar não só a interação com a KVS shell, mas também podemos ver o tráfego sendo enviado pelo cliente usando ssl (o trafego recebido pode ser visto no log de envio do server).
{{< hint warning >}}
O primeiro trecho de execução mostra o tráfego referente ao _handshake_ entre o cliente e o servidor, onde podemos ver ao final da execução o tipo de criptografia utilizado para a comunicação.
{{< /hint >}}

{{< details title="Handshake cliente" open=true >}}

```txt
========== send:  ssl ==========
00000000: 16 03 01 02 00 01 00 01  fc 03 03 1a 8c a6 ca 54  ...............T
00000010: ef 3f f2 50 2f 9f d1 b2  b4 77 b6 36 06 0d 91 08  .?.P/....w.6....
00000020: 4c 84 98 b6 a7 48 df 40  27 29 93 20 c8 74 ae b2  L....H.@'). .t..
00000030: 18 ab 76 e5 ed eb 8a 18  33 bf 3b da 95 6d 7d fa  ..v.....3.;..m}.
00000040: 5e 6d 87 73 7d b5 20 8e  be 71 eb cb 00 24 13 02  ^m.s}. ..q...$..
00000050: 13 03 13 01 c0 2c c0 30  c0 2b c0 2f cc a9 cc a8  .....,.0.+./....
00000060: c0 24 c0 28 c0 23 c0 27  00 9f 00 9e 00 6b 00 67  .$.(.#.'.....k.g
00000070: 00 ff 01 00 01 8f 00 00  00 0e 00 0c 00 00 09 6c  ...............l
00000080: 6f 63 61 6c 68 6f 73 74  00 0b 00 04 03 00 01 02  ocalhost........
00000090: 00 0a 00 16 00 14 00 1d  00 17 00 1e 00 19 00 18  ................
000000a0: 01 00 01 01 01 02 01 03  01 04 00 23 00 00 00 16  ...........#....
000000b0: 00 00 00 17 00 00 00 0d  00 2a 00 28 04 03 05 03  .........*.(....
000000c0: 06 03 08 07 08 08 08 09  08 0a 08 0b 08 04 08 05  ................
000000d0: 08 06 04 01 05 01 06 01  03 03 03 01 03 02 04 02  ................
000000e0: 05 02 06 02 00 2b 00 05  04 03 04 03 03 00 2d 00  .....+........-.
000000f0: 02 01 01 00 33 00 26 00  24 00 1d 00 20 da 30 5e  ....3.&.$... .0^
00000100: fd 4a 95 67 68 03 4f 59  56 5e e8 96 f7 33 53 37  .J.gh.OYV^...3S7
00000110: f9 ce b3 c5 ea e2 ee 4d  d4 f6 77 22 12 00 15 00  .......M..w"....
00000120: e4 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000130: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000140: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000150: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000160: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000170: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000180: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000190: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
000001a0: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
000001b0: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
000001c0: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
000001d0: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
000001e0: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
000001f0: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000200: 00 00 00 00 00                                    .....
========== send:  ssl ==========
00000000: 14 03 03 00 01 01 17 03  03 00 45 73 50 fe 01 78  ..........EsP..x
00000010: 84 36 b9 40 72 85 1d f8  2b e4 3d 46 43 70 47 52  .6.@r...+.=FCpGR
00000020: 9e 5f 22 50 e6 a3 1d d8  63 0f 54 ff 15 b9 80 48  ._"P....c.T....H
00000030: 8d 71 20 2a d6 05 61 ff  7f 16 21 20 4c c2 dd 7f  .q *..a..! L..
00000040: 19 55 42 30 50 c1 bd d1  db 32 ca c3 b9 b7 e3 a8  .UB0P....2......
Cipher: ('TLS_AES_256_GCM_SHA384', 'TLSv1.3', 256)
Bem-vindo ao shell do Key-Value Store. Digite 'help' para listar os comandos disponíveis.
(KVS)

```

{{< /details >}}

Em seguida podemos ver o conteúdo das mensagens enviadas ao interagir com a shell do cliente. Da maneira que foi implementado é possível ver a mensagem que está para ser enviada (**send: original**) e a mensagem sendo efetivamente enviada pelo ssl (**send: ssl**). Desse modo, a execução abaixo é uma prova de que o TLS **tem sigílo**, pois a mensagem que está sendo enviada pelo socket ssl claramente foi cifrada, em comparação com a mensagem original que está em plena vista.

{{< details title="Tráfego cliente" open=true >}}

```txt
(KVS) show
======== send: original ========
00000000: 73 68 6f 77                                       show
========== send:  ssl ==========
00000000: 17 03 03 00 15 fb 51 0a  d9 81 0e 72 40 d5 8a ef  ......Q....r@...
00000010: d3 8a 4f 7f de 9a fb d9  2c 14                    ..O....,.
{1: 1, 2: 2, 3: 4, 'joao': 1, 5: 6}
(KVS) delete 5
======== send: original ========
00000000: 64 65 6c 65 74 65 20 35                           delete 5
========== send:  ssl ==========
00000000: 17 03 03 00 19 15 d5 33  1e 76 41 95 35 87 2f 79  .......3.vA.5./y
00000010: 63 6c 98 98 c7 19 de 40  8a 96 8c 7d bf 5c        cl.....@...}.\
Ok
(KVS) show
======== send: original ========
00000000: 73 68 6f 77                                       show
========== send:  ssl ==========
00000000: 17 03 03 00 15 53 5a df  5f 2c 5c d9 48 34 c2 e7  .....SZ._,\.H4..
00000010: db 99 d1 f5 00 d2 f2 d2  58 3e                    ........X>
{1: 1, 2: 2, 3: 4, 'joao': 1}
(KVS) exit

```

{{< /details >}}

## Client \-\-edit

Novamente, com o servidor rodando, executamos `ptyhon client.py --edit`. Desta vez, por se tratar de um log que já foi analizado na sessão anterior, vamos emitir o tráfego do _handshake_. Dito isso, segue abaixo a execução de um comando que teve um byte alterado:

{{< details title="Mensagem cliente editada" open=true >}}

```txt
Bem-vindo ao shell do Key-Value Store. Digite 'help' para listar os comandos disponíveis.
(KVS) show
======== send: original ========
00000000: 73 68 6f 77                                       show
========== send:  ssl ==========
00000000: 17 03 03 00 15 9e 49 05  af d1 16 e2 00 0d 3e 9a  ......I.......>.
00000010: bf 69 6d 45 db 39 3e d4  62 9c                    .imE.9>.b.
Do you want to edit this message? [y/N] n
{1: 1, 2: 2, 3: 4, 'joao': 1}
(KVS) update 'joao' 10
======== send: original ========
00000000: 75 70 64 61 74 65 20 27  6a 6f 61 6f 27 20 31 30  update 'joao' 10
========== send:  ssl ==========
00000000: 17 03 03 00 21 a6 27 ff  01 99 13 4b 61 54 1c 20  ....!.'....KaT.
00000010: bd 0e b7 45 30 46 1e 76  01 89 05 b5 03 8a 3b ba  ...E0F.v......;.
00000020: ad 03 2b 4a 72 1c                                 ..+Jr.
Do you want to edit this message? [y/N] y
Enter the index in hexadecimal: 20
Enter the byte in hexadecimal: ae
========= send: edited =========
00000000: 17 03 03 00 21 a6 27 ff  01 99 13 4b 61 54 1c 20  ....!.'....KaT.
00000010: bd 0e b7 45 30 46 1e 76  01 89 05 b5 03 8a 3b ba  ...E0F.v......;.
00000020: ae 03 2b 4a 72 1c                                 ..+Jr.
Traceback (most recent call last):
  File "/home/gobbo/materias/7Periodo/redes3/secureKVS/client.py", line 65, in <module>
    ClientAtacavel(allow_editing).run()
  File "/home/gobbo/materias/7Periodo/redes3/secureKVS/client.py", line 39, in run
    KeyValueStoreShell(self.create_socket()).cmdloop()
  File "/usr/lib/python3.10/cmd.py", line 138, in cmdloop
    stop = self.onecmd(line)
  File "/usr/lib/python3.10/cmd.py", line 217, in onecmd
    return func(arg)
  File "/home/gobbo/materias/7Periodo/redes3/secureKVS/key_value_store.py", line 141, in do_update
    resp = self._send_query(f'{kvs.update.__name__} {chave} {valor}')
  File "/home/gobbo/materias/7Periodo/redes3/secureKVS/key_value_store.py", line 106, in _send_query
    return self.secure_socket.recv(1024).decode()
  File "/home/gobbo/materias/7Periodo/redes3/secureKVS/utils/inspectable.py", line 117, in recv
    data = self.ssl_object.read(n_bytes)
  File "/usr/lib/python3.10/ssl.py", line 917, in read
    v = self._sslobj.read(len)
ssl.SSLError: [SSL: SSLV3_ALERT_BAD_RECORD_MAC] sslv3 alert bad record mac (_ssl.c:2548)

```

{{< /details >}}

Podemos ver que o erro apontado, `SSLV3_ALERT_BAD_RECORD_MAC`, indica que o protocolo não conseguiu verificar a autenticidade da mensagem - **Message Authentication Codes (MAC)**, devido à alteração que fizemos no corpo dela.

{{< hint warning >}}
Provando então a integridade do TLS para o cliente.
{{< /hint >}}

## Server \-\-show

Para ver o tráfego abaixo é necessário rodar uma das duas opções abaixo:

```py
python server.py --show
python  server.py --edit
```

Não só do \-\-show, este log representa o tráfego no servidor para a flag \-\-edit também. Deste modo, o tráfego do lado dos servidor aparece como mostrado abaixo:

{{< details title="Handshake server" open=false >}}

```txt
Aguardando conexões...
========== send:  ssl ==========
00000000: 16 03 03 00 7a 02 00 00  76 03 03 b2 a8 29 69 20  ....z...v....)i
00000010: 89 b0 92 bc fc e8 ac 8d  1f f4 c2 e5 f7 61 78 1e  .............ax.
00000020: b6 d0 ba c8 ef 37 6d b6  dc 49 d0 20 4c 29 01 63  .....7m..I. L).c
00000030: 6f 1c 66 a0 9e a1 77 1e  c1 a2 f2 63 32 f4 f7 e7  o.f...w....c2...
00000040: 3d d9 35 65 10 3a 7b f1  4f 09 ec b1 13 02 00 00  =.5e.:{.O.......
00000050: 2e 00 2b 00 02 03 04 00  33 00 24 00 1d 00 20 94  ..+.....3.$... .
00000060: 02 0f dc 2b 4b 7b 06 1a  c5 02 b6 73 e6 62 a8 be  ...+K{.....s.b..
00000070: 01 6c 16 d3 f2 24 1c 70  26 ad 87 6b 6f 06 2c 14  .l...$.p&..ko.,.
00000080: 03 03 00 01 01 17 03 03  00 17 24 bd 07 3f 42 3d  ..........$..?B=
00000090: 90 ac c7 82 2d 49 87 27  b3 ab fc a9 a1 2e f4 31  ....-I.'.......1
000000a0: 29 17 03 03 03 bb 54 34  f8 4f e1 f9 58 ae 3f 42  ).....T4.O..X.?B
000000b0: 8b b3 81 6e 2d 77 03 43  82 12 6a d8 66 11 81 43  ...n-w.C..j.f..C
000000c0: 3c 36 6b 63 86 51 b8 7b  d4 c2 8f d0 78 41 83 4e  <6kc.Q.{....xA.N
000000d0: 3f 33 fe 54 2b 83 25 37  49 f4 d4 d5 a7 72 dd b6  ?3.T+.%7I....r..
000000e0: a8 26 75 73 60 33 8b ab  1b 08 66 99 a8 77 c0 12  .&us`3....f..w..
000000f0: 10 89 c8 ab 95 27 8d bd  f6 97 d6 52 ec 21 2d e7  .....'.....R.!-.
00000100: 78 0a da 63 81 f6 36 d6  a2 08 5b 31 9d 71 30 e5  x..c..6...[1.q0.
00000110: cd 98 be 3b 99 44 c1 af  69 51 d5 6e 0f ed 8f 70  ...;.D..iQ.n...p
00000120: 11 69 23 62 3e c0 54 8f  b7 13 86 37 e0 35 8c ef  .i#b>.T....7.5..
00000130: 78 0f 20 e6 a0 da 64 39  d1 ed 51 8c 69 91 df b3  x. ...d9..Q.i...
00000140: 9f 69 ad 23 55 a0 f8 30  e0 fe 16 23 6a c2 0c ad  .i.#U..0...#j...
00000150: 90 3c c9 a0 c6 68 79 8c  d3 af f5 6b 18 19 b2 e3  .<...hy....k....
00000160: 3f 52 53 4b e5 03 5c e6  f1 86 10 64 9e 30 73 cb  ?RSK..\....d.0s.
00000170: ee 0d 31 22 5b 9f 34 b8  27 1e d2 2e 6c 5d 76 f6  ..1"[.4.'...l]v.
00000180: 9b 72 0c 01 fe f8 c9 72  46 ed 7f 0e 99 7f e2 82  .r.....rF.....
00000190: 7d 13 f6 8e 83 f2 7c 36  a2 4b 4c 9b 9a 3a e4 06  }.....|6.KL..:..
000001a0: 90 57 fc 59 21 2b 2e 7d  a7 9c 3e 57 7d 11 a2 88  .W.Y!+.}..>W}...
000001b0: cb 12 11 3d 6a bb 3f 4f  9c 24 f1 a9 4c 1d 23 55  ...=j.?O.$..L.#U
000001c0: 3b 01 10 01 58 63 05 30  ae dc 50 5f 1a 8c dd 4b  ;...Xc.0..P_...K
000001d0: b1 c7 2b b8 83 b1 a3 91  05 ba bd 90 6d e7 3e 0a  ..+.........m.>.
000001e0: 2b 37 36 49 e4 5d 9f 66  20 1e 11 24 61 0a c3 1c  +76I.].f ..$a...
000001f0: de 5e ff f0 10 ed dd 52  33 1d 40 22 a1 c0 28 5b  .^.....R3.@"..([
00000200: d0 52 e1 1a db 7f 53 b8  e4 e4 90 f3 26 b4 98 3e  .R...S.....&..>
00000210: 39 35 5f 85 bd 5d 49 90  29 cf 95 46 41 ef 44 fe  95_..]I.)..FA.D.
00000220: f2 66 eb ac dd b8 e6 8b  bd 77 01 4f 73 69 1a 4f  .f.......w.Osi.O
00000230: 7c 3f 2f 44 66 fe 85 df  4e 25 3e 31 b9 ca 15 1d  |?/Df...N%>1....
00000240: 0f 54 5b 00 61 45 d6 c5  6f 42 36 8f 05 53 4d f2  .T[.aE..oB6..SM.
00000250: 03 10 19 08 29 91 5e 9c  02 8d db 00 b0 92 47 d0  ....).^.......G.
00000260: 2a 68 87 5d 4f 63 2d f6  be 46 15 e7 1e f5 9e 6b  *h.]Oc-..F.....k
00000270: 02 4c 6b 72 b2 8b af 53  67 65 60 cf f0 9e d4 b2  .Lkr...Sge`.....
00000280: aa 6b db 12 72 8e 5a d9  ce 80 b0 58 6d 0f 3b 5f  .k..r.Z....Xm.;_
00000290: 51 f6 5e b7 4f 13 d3 79  1b ef 42 22 96 f7 50 a6  Q.^.O..y..B"..P.
000002a0: 11 d1 18 f7 f7 90 18 07  ea e9 f1 08 fe 16 bb f0  ................
000002b0: 3b e1 73 21 50 a2 fe f6  6e 86 0d 89 4a e2 03 8f  ;.s!P...n...J...
000002c0: 02 3f c6 96 93 ea 20 4b  42 d9 58 71 4c 11 5f e5  .?.... KB.XqL._.
000002d0: 1c 5c 6d 63 3f de af 5d  f2 f2 8e 76 8b 41 9d 76  .\mc?..]...v.A.v
000002e0: 63 6c ef 92 81 32 1b e1  09 cd 47 50 8f e8 9c 8f  cl...2....GP....
000002f0: 37 ad fd 8c e9 04 af eb  ac 61 f6 55 da 39 60 7a  7........a.U.9`z
00000300: 89 01 b9 2d 7b 73 c2 38  6a 61 de bc 26 a0 67 b9  ...-{s.8ja..&.g.
00000310: 67 97 e5 73 6e 0a b4 22  db fd c9 09 3b 9a a6 72  g..sn.."....;..r
00000320: 14 b7 a4 d5 09 cb 29 4a  8a dc 14 ab 90 d9 9c e9  ......)J........
00000330: cc 26 95 41 60 8e 35 3b  9a c0 3b f4 e9 1a 1f 3a  .&.A`.5;..;....:
00000340: 3f a8 51 d3 c8 c2 93 03  a6 b3 24 91 ea 1e dc bc  ?.Q.......$.....
00000350: 2c 75 09 79 7e 8c b5 a6  0c 74 82 e3 5f 72 ae d8  ,u.y~....t.._r..
00000360: be 0c 52 0a 40 bf c3 7e  d5 77 65 f0 04 80 a6 d3  ..R.@..~.we.....
00000370: db d4 ed 52 da 94 d3 4f  46 f9 e2 08 47 fa ce 59  ...R...OF...G..Y
00000380: a9 3e 2a 7f 05 e5 77 bd  dc f9 1a 7a 99 14 a7 df  .>*..w....z....
00000390: e9 99 5a a8 ae 08 32 f7  11 88 81 0e 32 79 1e 9c  ..Z...2.....2y..
000003a0: 1b 2a 7f f7 20 49 b9 2e  cc fb 89 40 69 f9 00 01  .*. I.....@i...
000003b0: 35 74 84 e1 aa a7 55 fe  4c 84 a3 17 99 9b ed 28  5t....U.L......(
000003c0: ec 65 25 b6 b4 67 28 ea  64 b3 af bb 87 13 ca 40  .e%..g(.d......@
000003d0: 3c eb a6 eb 31 95 84 92  6a 67 01 6f ab 6c e4 71  <...1...jg.o.l.q
000003e0: af cf 4c 7a c3 86 16 30  6e 52 c5 7c dd f6 62 17  ..Lz...0nR.|..b.
000003f0: c1 92 40 cc 19 b0 2a 3b  c1 4a 29 9d 05 2e ba 0b  ..@...*;.J).....
00000400: e7 1d 91 97 ed 16 00 2d  9b 98 4b dd ab dc a5 4e  .......-..K....N
00000410: ec 3b 9c bb ae ee 3d 97  5a 7d 90 08 8c 81 9a 68  .;....=.Z}.....h
00000420: 66 98 ef 33 48 b5 e0 45  1a 1e 3e 10 3f 82 12 46  f..3H..E..>.?..F
00000430: f1 db ba 73 ff 85 0b 48  20 12 8b 1b 69 34 81 a0  ...s...H ...i4..
00000440: 06 83 ff a5 84 fa e5 ea  84 c7 b7 b0 ad db 91 72  ...............r
00000450: c0 f5 67 12 b4 8d e0 0b  23 70 70 c7 d0 fb 85 b3  ..g.....#pp.....
00000460: 31 17 03 03 01 19 ca 41  a3 ce 5f 0a a5 fc 59 4c  1......A.._...YL
00000470: dc 7a cd fb d5 93 59 7c  95 39 98 8d 14 7f 38 cc  .z....Y|.9...8.
00000480: d8 91 f2 31 9d 02 4a 89  4f b1 bf 07 bc 1d 4e 6a  ...1..J.O.....Nj
00000490: 13 5e a7 5d 5a 1e 98 63  a3 05 3d c7 19 90 c2 9b  .^.]Z..c..=.....
000004a0: ed 84 4f 08 24 c3 c6 c4  1a cc f0 86 2c bb c3 c8  ..O.$.......,...
000004b0: 75 90 78 e8 34 9f bb 91  f5 75 e1 9f ab 7e a1 b4  u.x.4....u...~..
000004c0: f2 83 e4 0d 80 53 33 fd  e5 66 18 48 c3 f3 8a 5e  .....S3..f.H...^
000004d0: 6e 0d 28 83 d5 52 83 b9  15 5a 67 1a 98 66 10 df  n.(..R...Zg..f..
000004e0: bb 32 35 26 87 1a bc e4  30 3b ca 1f 32 f9 91 1a  .25&....0;..2...
000004f0: 9c 16 24 57 07 02 fa 58  e2 46 18 7f 37 3a ce 10  ..$W...X.F.7:..
00000500: a7 36 70 a8 2b 2f 20 7b  c5 0e b8 49 cb 62 8a df  .6p.+/ {...I.b..
00000510: c2 7c 92 54 bd 07 56 30  dd d1 5d 1f 24 5a c1 95  .|.T..V0..].$Z..
00000520: f3 88 1b 98 73 b7 6b eb  08 61 c4 b6 44 e9 80 32  ....s.k..a..D..2
00000530: 2e a2 aa 13 3c d2 9c f0  35 a5 e3 0f e4 06 fd 61  ....<...5......a
00000540: a7 08 a1 af 37 c6 ad 46  79 f2 a2 3f d0 be f6 9c  ....7..Fy..?....
00000550: 90 68 a8 e7 d2 4c d2 00  a0 a1 55 d7 cd fe 57 e5  .h...L....U...W.
00000560: ea 63 7b 41 4d 56 0d 79  02 0a f2 7c 8c bf eb ca  .c{AMV.y...|....
00000570: 45 fa 66 0b 50 39 18 63  49 42 08 19 db 10 ba 17  E.f.P9.cIB......
00000580: 03 03 00 45 a3 f4 17 30  ba d1 94 66 93 6a 58 4e  ...E...0...f.jXN
00000590: b9 fd 14 6c c4 5e 28 85  77 1f 31 68 b8 6f 51 87  ...l.^(.w.1h.oQ.
000005a0: 09 a3 e7 50 f2 20 7a 0a  2a 8c cf ad 55 43 cd 47  ...P. z.*...UC.G
000005b0: 4d 38 ed fb 4e b0 72 bd  5a f3 ba c7 bb e5 5f e4  M8..N.r.Z....._.
000005c0: 48 bd dd 4d 6c 34 97 1f  4f                       H..Ml4..O
========== send:  ssl ==========
00000000: 17 03 03 00 fa 63 4e e5  7c 88 c5 16 1e 84 3c 2c  .....cN.|.....<,
00000010: 7f 8b a6 95 20 fb 96 42  8a 9b 73 e1 50 0d 3e 4f  ... ..B..s.P.>O
00000020: 25 57 40 23 e1 12 be 24  a4 20 fe a3 5a 12 d2 8f  %W@#...$. ..Z...
00000030: e9 d0 51 a3 90 45 d2 04  e3 4b 18 36 22 38 4d d8  ..Q..E...K.6"8M.
00000040: 87 f5 cd 40 bf d0 50 83  52 87 ba 2c 48 99 e6 65  ...@..P.R..,H..e
00000050: ab 28 98 9e 83 28 d6 cc  1b 03 a0 9e 3d 44 56 17  .(...(......=DV.
00000060: 41 81 69 92 a5 8a 14 c1  64 54 ff b3 ac c2 a8 7b  A.i.....dT.....{
00000070: 57 28 08 da f2 fb d3 53  47 c6 d1 1a c7 79 02 d0  W(.....SG....y..
00000080: 07 a7 78 07 1d 8c 81 88  c5 ad 5a ce b7 f4 4a 36  ..x.......Z...J6
00000090: d5 7c 79 40 da f0 0e e0  90 f7 8e bb 4b 1b 4d e9  .|y@........K.M.
000000a0: 92 12 47 be 3e 35 63 ae  aa 1c 2f 5b b9 d5 ad 43  ..G.>5c.../[...C
000000b0: 7c c2 9a 13 ea 2f ad 86  9b d4 a6 77 cd c0 50 2e  |..../.....w..P.
000000c0: b0 6c 31 fc 62 24 7f c5  ba b9 99 46 40 d7 f6 eb  .l1.b$....F@...
000000d0: 9e 4f 98 77 f0 ff 71 16  23 ac 0e 2c ce bb a1 23  .O.w..q.#..,...#
000000e0: 80 8a 2c 78 5d cd 65 bc  f1 f9 a7 cd 2e 09 d3 eb  ..,x].e.........
000000f0: c1 5d c6 bf 76 b4 ca 09  a1 cf 7e 35 a1 7b 51 17  .]..v.....~5.{Q.
00000100: 03 03 00 fa c0 11 e2 7c  0f 61 c0 00 f3 73 12 dd  .......|.a...s..
00000110: 3d 33 e4 c9 b8 d7 53 c1  4b b5 a3 ff e6 d2 54 72  =3....S.K.....Tr
00000120: 77 5c 7f 5a 5d 15 0d 68  4a ab a4 08 cd 0d 1d 0a  w\Z]..hJ.......
00000130: c6 6b 33 51 be 52 06 53  f8 95 69 10 c0 c8 54 12  .k3Q.R.S..i...T.
00000140: c9 32 79 61 6f 46 06 93  d2 85 23 b7 c1 26 e9 cf  .2yaoF....#..&..
00000150: c0 34 83 ab a9 f6 b6 8e  cb ad b6 7c 73 68 e9 1e  .4.........|sh..
00000160: 29 c8 98 06 e2 87 9e b8  b4 42 a6 85 1c 5b ec bc  )........B...[..
00000170: 90 36 3f 1b 6a 91 e0 50  46 72 2d 64 4c 35 fa 48  .6?.j..PFr-dL5.H
00000180: 98 5e ff 33 9f 1d e6 14  c4 46 d0 65 9d 3c e9 ab  .^.3.....F.e.<..
00000190: 33 a9 50 8f 0d b7 60 31  6f f2 d5 13 51 03 d4 69  3.P...`1o...Q..i
000001a0: 3a 6c 5b 23 0d c8 66 a7  e4 f0 b4 ff b7 42 98 d1  :l[#..f......B..
000001b0: 2f 92 1a 6c ea 9a cf c3  62 05 1e dc 44 db c9 10  /..l....b...D...
000001c0: 44 38 ea 36 a4 ad a2 bd  cc 84 bd 8c ba 36 c7 ef  D8.6.........6..
000001d0: 68 44 45 16 20 de 55 c4  11 c8 31 58 f0 27 06 2c  hDE. .U...1X.'.,
000001e0: 8f b1 32 4e 5c 75 e8 8f  20 d2 40 9f 05 77 93 83  ..2N\u.. .@..w..
000001f0: c9 50 2d 10 74 d9 1f 67  b6 87 7c 15 80 d3        .P-.t..g..|...
Cipher: ('TLS_AES_256_GCM_SHA384', 'TLSv1.3', 256)
Conexão estabelecida com ('127.0.0.1', 33720)

```

{{< /details >}}

- Abaixo temos a resposta do servidor para a sequência de comandos

1. show
2. update 4 4
3. create 5 5
4. delete 4
5. exit

{{< details title="Mensagem servidor" open=true >}}

```txt
======== send: original ========
00000000: 7b 31 3a 20 31 2c 20 32  3a 20 32 2c 20 33 3a 20  {1: 1, 2: 2, 3:
00000010: 33 2c 20 27 6a 6f 61 6f  27 3a 20 32 2c 20 34 3a  3, 'joao': 2, 4:
00000020: 20 35 7d                                           5}
========== send:  ssl ==========
00000000: 17 03 03 00 34 fc fe ea  e6 ae c6 f7 e2 c0 58 cd  ....4.........X.
00000010: 51 ef 4d 4d d9 f0 34 87  f0 00 49 6c ea ce 38 5e  Q.MM..4...Il..8^
00000020: f3 3c 01 d9 88 f5 c1 1e  a9 7e 50 0d 2e 7a 33 2c  .<.......~P..z3,
00000030: 9a cc 87 ca dd e9 f3 2f  2d                       ......./-
======== send: original ========
00000000: 4f 6b                                             Ok
========== send:  ssl ==========
00000000: 17 03 03 00 13 c3 8c 5a  61 fa 3f 5c 96 d0 90 57  .......Za.?\...W
00000010: f0 e8 91 5b f6 8d f5 be                           ...[....
======== send: original ========
00000000: 4f 6b                                             Ok
========== send:  ssl ==========
00000000: 17 03 03 00 13 97 f6 ce  70 e3 8a 1e 6a fb 51 c4  ........p...j.Q.
00000010: 1d f0 2e d8 0e 45 b6 b4                           .....E..
======== send: original ========
00000000: 4f 6b                                             Ok
========== send:  ssl ==========
00000000: 17 03 03 00 13 d7 00 ac  f7 95 31 ee 72 a7 fe 57  ..........1.r..W
00000010: 8f 96 82 de c7 19 04 4d                           .......M

```

{{< /details >}}
