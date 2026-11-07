---
id: 202611070802
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - dhcp
  - rede
---

# Servidor DHCP no FortiGate

O FortiGate pode atuar como servidor DHCP e entregar automaticamente parâmetros de rede aos clientes, como endereço IP, máscara, gateway e DNS.

## Requisitos da interface

Para configurar o servidor DHCP em uma interface, o material da aula destaca estes requisitos:

- A interface deve ter um endereço IP configurado manualmente.
- A interface deve usar a role **LAN**.

## Parâmetros básicos

Uma configuração pode incluir:

- Faixa de endereços a distribuir, por exemplo, `192.168.10.2–192.168.10.254`.
- Máscara de rede.
- Gateway padrão.
- Servidores DNS.
- Tempo de concessão (*lease time*).
- Criação de um objeto de endereço correspondente à sub-rede.

O tempo de concessão é informado em segundos. Por exemplo, `604800` segundos correspondem a **7 dias**.

## Opções avançadas

O servidor DHCP também pode oferecer configurações avançadas, como:

- Modo **Server**, **Relay** ou **IPsec**, conforme o cenário e os recursos disponíveis.
- Servidor NTP.
- Fuso horário.
- Controladores wireless.
- Próximo servidor de bootstrap e servidores TFTP.
- Opções DHCP adicionais.

Os nomes e as opções disponíveis podem variar de acordo com a versão do FortiOS.

## Reservas e controle por MAC

Por padrão, o servidor pode oferecer endereços aos dispositivos que solicitarem uma concessão. É possível configurar controles adicionais, como:

- Reservar um endereço IP para um MAC específico.
- Bloquear um MAC específico.
- Restringir a distribuição a dispositivos autorizados, usando a opção de bloqueio geral e exceções permitidas, quando disponível.

> Uma reserva DHCP associa um IP a um dispositivo identificado pelo MAC; não substitui controles de acesso ou autenticação da rede.

## Checklist de troubleshooting

1. Confirme que a interface tem IP estático.
2. Confirme que a role está como **LAN**.
3. Verifique se o servidor DHCP está habilitado.
4. Confira se a faixa de endereços corresponde à sub-rede da interface e não contém conflitos.
5. Revise gateway, DNS e tempo de concessão.
6. Verifique reservas e regras de bloqueio por MAC.

## Relações

- [[311_Acesso_inicial_e_administracao]]
- [[313_VLANs]]
- [[314_Roteamento]]