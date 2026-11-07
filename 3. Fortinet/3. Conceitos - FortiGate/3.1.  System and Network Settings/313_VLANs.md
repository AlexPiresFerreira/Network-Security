---
id: 202611070803
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - vlan
  - camada-2
  - segmentacao
---

# VLANs no FortiGate

Uma VLAN permite segmentar uma rede de camada 2 em domínios de broadcast distintos. No FortiGate, uma interface VLAN funciona como uma interface lógica associada a uma interface física.

## Como funciona

- O identificador da VLAN é o **VLAN ID**.
- Em quadros Ethernet marcados, a tag VLAN identifica o segmento ao qual o tráfego pertence.
- Uma interface física pode transportar várias VLANs, desde que cada subinterface use a configuração de VLAN correspondente.
- Cada subinterface VLAN pode ter configurações próprias, como endereço IP, role e, quando aplicável, servidor DHCP.

## Criar uma interface VLAN

Ao configurar uma VLAN, defina:

1. O tipo **VLAN**.
2. A interface física associada.
3. O nome da interface lógica.
4. O **VLAN ID**.
5. As demais configurações necessárias, como IP, role e DHCP.

A porta física e a configuração do switch conectado precisam permitir o transporte das VLANs esperadas.

## Tráfego marcado e não marcado

- **Tráfego marcado (*tagged*)**: contém um VLAN ID e pode ser associado à subinterface VLAN correspondente.
- **Tráfego não marcado (*untagged*)**: chega sem tag e é tratado de acordo com a configuração da interface física e do equipamento conectado.

> Atenção: o material da aula relaciona tráfego sem tag à VLAN nativa e menciona VID 0. Em redes Ethernet, tráfego não marcado não carrega, por si só, um VLAN ID; a VLAN a que ele pertence depende da configuração da porta e do equipamento. Não confunda quadros sem tag com uma tag de VLAN ID 0.

## VLANs e multi-VDOM

Em um ambiente multi-VDOM, a interface física e uma interface VLAN associada podem estar vinculadas a VDOMs diferentes, conforme o desenho e os recursos da versão. Por exemplo:

- `port1` pertence à VDOM `root`.
- A interface VLAN `port1.100` pertence à VDOM `PARCEIROS`.

Planeje essa associação com cuidado: o tráfego entre VDOMs não é permitido automaticamente. Consulte [[315_VDOM]] para a ligação entre VDOMs.

## Checklist de troubleshooting

1. Confirme o VLAN ID na subinterface e no switch.
2. Verifique se a VLAN está permitida no enlace entre o switch e o FortiGate.
3. Confira se o tráfego chega marcado ou não marcado, conforme o desenho.
4. Confirme a associação da interface à VDOM correta.
5. Revise o IP, a máscara e as políticas necessárias para o tráfego.

## Relações

- [[312_DHCP]]
- [[314_Roteamento]]
- [[315_VDOM]]