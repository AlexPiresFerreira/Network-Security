
#### Ferramenta

- tcpdump
- tshark
- wireshark
- mtr
	- Traceroute em tempo real 
		- mtr [IP ou URL]
- ping
	- ping e pong 
- netcat
- iptraf
- packit
	- packit -d 10.0.0.5 -F SPR 
- netdiscover
	- MAC  & IP

#### Outros

- TTL do linux
	- cat /proc/sys/net/ipv4/ip_default_ttl
- Protocol 
	- cat /etc/protocols

#### Simulador

- CORE
	- https://github.com/coreemu/core
- Docker
	- https://hub.docker.com/r/d3f0/coreemu_vnc/
		- debianet.com.br

---

#### Cabeçalho

##### IP

```
    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |Version|  IHL  |Type of Service|          Total Length         |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         Identification        |Flags|      Fragment Offset    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Time to Live |    Protocol   |         Header Checksum       |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                       Source Address                          |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Destination Address                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

- 32 bit (4baty) de largura, no  minumo 5 linhas de cabeçalho - 20 bytes

- Version: 4 bit, versão do IP, valot 0100
- IHL(Internet Hearder Length): 4 bit, quantas linha á no cabeçalho, cada linha é 4 bytes, de 5 a 15 linha - de 20 á 60 bytes
- Type of service(ToS): 8 bit - 1 byte, QoS
- Total Length: 16 bit, Cabeçalho + payload, poderá ter até 65535 bytes de tamanho - 64KB
- Identification: 16 bit, Fragmentação de pacote, Ethernet  padrão 1500bytes, os campos Identification, source addres e destination address não tem alteração para os demais pacotes
- Flags: 3 bits, none(0), DF(don't fragment - 0 pode fragmentar e 1 não pode fragmentar) e MF(more fragment - 1 mais fragmento e 0 acabou a fragmentação)
- Fragment Offset: 13bits, byte inicial  de cada fragmento do pacote dividido por 8 (Ex. pacote 3000bytes = 1 - 0-1479 (0/8=0), 2 - 1480-2959(1480/8=185), 3 - 2960-2979(2960/8=370) ) tem q tirar o cabeçalho IP 20bytes
-  Time to Live: 8bits, 
- Protocol : 8bits, ICMP 1, TCP 6 e UDP 17
- Header Checksum: 16bits, Garantir que o cabeçalho IP transitará integro 
- Source Address: 32bits, endereço de origem 
- Destination Address: 32bits, endereço de destino
- Options: 0-40bytes(0-10 linhas)
- Padding: só existe se tiver Options

Version + IHL = 0x45 (1º)
Header Checksum (11 e 12) --> zera os campos 11 e 12 e soma todos  restante em 2 em 2 bytes, separa o resultado em 2 em 2 bytes e somar novamente, converter o resultado para binario, subtraí ffff do resultado   


- TCP

```
    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |          Source Port          |       Destination Port        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                        Sequence Number                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Acknowledgment Number                      |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Data |           |U|A|P|R|S|F|                               |
   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |
   |       |           |G|K|H|T|N|N|                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           Checksum            |         Urgent Pointer        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                             data                              |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

- 1ª Linha
	- Source Port = Porta de origem, 16bit
	- Destination Port = Porta de destino, 16bits

- 2ª Linha
	- Sequence Number = Identificar o segmento TCP 32bit

- 3ª Linha
	- Acknowledgment Number  = Host-A --> Host-B: Usado para que o Host-A possa declarar que sabe qual será o próximo número de sequência a ser enviado pelo host 32bit

- 4ª Linha
	- Data Offset = Tamanho do cabeçalho TCP 4bit (Quantas linha há no cabeçalho "IHL*4") 5 linha 20bytes á 15 linha 60bytes

	- Flags TCP - 12bits  
		- Reserved bit1 á bit6
		- URG bit7 
		- ACK bit8: (acknowledgment) Serve para que um dos lados, ao receber um segmento, confirme, para quem o enviou, que sabe qual será o número de sequemcia do próximo segmento.  
		- PSH bit9: (push - empurrar) Sinalizar que há dados no payload do segmento
		- RST bit10: (reset) Não entendi, fecha a conexão  
		- SYN bit11: (synchronize) Usada para iniciar uma conexão
		- FIN bit12: Utilizada para finalizar uma conexão  

	- Window = Serve para determinar a quantidade máxima de bytes que o host poderá receber no próximo segmento TCP 16bit 
		- windown Scale = Qual é o valor para multiplicar o campo windown 2^Valor_campo_wscale(Valor de 0 a 14) Mandado no SYN Sliding Windown 

- 5ª Linha
	- Checksum = Verificação do pacote TCP 16bit

	- Urgent Pointer = É usado junto a flag URG, Enviar informações que sejam urgentes para o trafégo na rede 16bit 

- 6ª linha á 15
	- Options = linha 6 á 15
		- Tipo 0: eol (end of optionj list) 1byte código 00h .É empregado para indicar que foi inserido o útimo elemento do campo option
		- Tipo 1: nop (no option) 1byte código 01h. Alinhamento de informações, fazendo com que o próximo dado comece em um bit que seja o início de um byte. 
		- Tipo 2: mss (maximo segment size) 4bytes código 02h. Maior quantidade de dados que o payload de um segmento poderá conter 
		- Tipo 3: wscale (windown scale) 3bytes código 03h. Expação do campo "windown size"
		- Tipo 4: sackOK (selective ack ok ou sack-permitted) 2bytes código 04h. Server para indicar que selectives ACKs (SACK) serão permitidos
		- Tipo 5: sack (Selective ack) com tamanho variável código 05h. Foi explanado, de forma sumária, quando tratamos da informação sackOK. 
		- Tipo 8: timestamp 10butes Essa informação é utilizada para calcular o RTT
			- TS val, ecr

	- Padding: Completa o campo option se não der 4bytes

- Última linha
	- Data
	- Calculo do tamanho do payload 
	- (tamanho total do pacote IP) - {(tamanho do cabeçalho IP) + (tamanho do cabeçalho TCP)}

- IP - RFC 791

	- até 15 linhas (obrigatório 5 linhas)

![[Pasted image 20251229190119.png]]

#### Flags

![[Pasted image 20251229200212.png]]

3whq

![[Pasted image 20251229200943.png]]

![[Pasted image 20251229201445.png]]

---

## UDP

#### Cabeçalho 

```
                  0      7 8     15 16    23 24    31
                 +--------+--------+--------+--------+
                 |     Source      |   Destination   |
                 |      Port       |      Port       |
                 +--------+--------+--------+--------+
                 |                 |                 |
                 |     Length      |    Checksum     |
                 +--------+--------+--------+--------+
```

32 bit de largura do caberçalho e tamanho total fixo de 8bytes

Sourcer Port: 16bits, 
Destination port: 16bits
 Length : 16bits
Checksum: 16bits

---

## ICMP

Tira foto Pg 210

rfc792

Cabeçalho 
	32bits, 
	 Type: 8bits
	 Code: 8bits
	 Checksum: 16bits

---

## Ethernet

- IEEE 802.3
	- mac DO TESTINO: 6 bytes, 
	- MAc de origem: 6 bytes, 
	- Tipo: 2 bytes
		- 0800h: IPv4, 0806h: ARP, 86ddh: IPv6
	- Payload: 46 a 1500bytes, 
	- Checksum: 4 bytes
	- 
- Jumbo frame
	- 9000 bytes
	

---

## ARP

- IPv4
- RFC826
- ARP request e ARP reply
- RFC5342
- netdiscover
- MAC spoofing
	- ifconfig eth2 hw ether [mac]
- ARP spoofing
- arping 
## NDP
- RFC4861

---

OSI 
- CAMADA 6
	- Quando tem tunel SSH o "S" nos protocolos no no inicio do nome 
-

---

Brifge
- bridge-utils

dns 
- RFC1035
- dig(dnsutils), nslookup e host
- 

SSL inspection


---

## Ferramenta 

- telnet 
- netcat
	- teste de conexão 
		- nc <IP ou servidor> <porta>
	- Para criar uma porta no servidor 
		- nc -l <porta>
		- UDP
			- nc -ul <porta>
- packit
- iperf
- dstat
- iptraf