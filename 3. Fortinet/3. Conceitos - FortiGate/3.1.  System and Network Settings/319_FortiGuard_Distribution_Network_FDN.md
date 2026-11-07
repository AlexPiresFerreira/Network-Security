---
id: 202611071003
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - fortiguard
  - fdn
  - atualizacoes
---

# FortiGuard Distribution Network (FDN)

A **FortiGuard Distribution Network (FDN)** é a infraestrutura da Fortinet que fornece ao FortiGate atualizações e serviços de inteligência de segurança. O FortiGate precisa de conectividade adequada, incluindo resolução DNS, para acessar os serviços correspondentes.

## O que o FortiGuard fornece

Dependendo do produto, da licença e da configuração, o FortiGuard pode ser usado para:

- Validar informações de registro e licença.
- Baixar atualizações de engines e bases de assinatura, como as relacionadas a antivírus e IPS.
- Consultar informações em tempo real para recursos como Web Filter, DNS Filter e Antispam.
- Fornecer outros dados de segurança, como reputação e informações de ameaças.

Nem todos os recursos funcionam da mesma maneira: alguns dependem de assinaturas atualizadas localmente; outros podem realizar consultas aos serviços FortiGuard.

## Requisitos gerais

- Conectividade com a Internet.
- Resolução DNS dos nomes usados pelos serviços.
- Licença ou contrato válido para os recursos que exigem assinatura.
- Regras de rede permitindo o tráfego necessário ao serviço utilizado.

## Endpoints e portas

| Serviço                                                | Endpoint                  | Protocolo/porta citados          | Finalidade                                                  |
| ------------------------------------------------------ | ------------------------- | -------------------------------- | ----------------------------------------------------------- |
| Atualizações de pacotes e assinaturas                  | `update.fortiguard.net`   | TCP 443                          | Atualizações de Antivirus, IPS e outros pacotes/assinaturas |
| Consultas comuns de Web Filter, DNS Filter e Antispam  | `service.fortiguard.net`  | UDP 53 ou UDP 8888               | Consultas aos serviços FortiGuard                           |
| Consultas seguras de Web Filter, DNS Filter e Antispam | `securewf.fortiguard.net` | HTTPS nas portas 443, 53 ou 8888 | Consultas seguras aos serviços FortiGuard                   |


> Endpoints, portas e transportes podem variar conforme serviço, configuração e versão do FortiOS. Use a documentação atual da Fortinet para criar regras de firewall; não trate a tabela como uma lista universal.
## Outros domínios Fortinet citados

O material também cita estes domínios:

- `globalupdate.fortinet.net`
- `globalguardservice.fortinet.net`
- `globallogctrl.fortinet.net`
- `globalmgrctrl.fortinet.net`
- `globalmsgctrl.fortinet.net`
- `globalaptctrl.fortinet.net`
- `globalproductapi.fortinet.net`

## Anycast e seleção de servidores

O material da aula descreve **anycast** como o modo padrão: o FortiGate acessa um endereço anunciado por uma rede de servidores, que direciona a conexão a um ponto de atendimento apropriado.

A aula também menciona uma opção chamada **multicast**, na qual a seleção do servidor pode considerar fatores como fuso horário e carga. A terminologia e o comportamento podem depender da versão e do contexto do material; confirme as opções disponíveis no FortiOS instalado.

Segundo a aula, no modo anycast as consultas de Web Filter, DNS Filter e Antispam são direcionadas para HTTPS na porta 443.

## FortiGuard local

O material cita a possibilidade de usar um serviço FortiGuard local por meio do FortiManager. Esse tipo de arquitetura pode alterar os destinos e os fluxos de comunicação necessários.

## Troubleshooting

O comando citado para verificar informações sobre o serviço de rating é:

```shell
diagnose debug rating
```

Ele pode ajudar a investigar a conectividade e as respostas relacionadas a serviços como Web Filter. O resultado deve ser analisado junto com a resolução DNS, a rota, as políticas de saída e a licença do recurso.

### Checklist

1. Confirme que o FortiGate tem conectividade com a Internet.
2. Verifique se os nomes FortiGuard necessários são resolvidos por DNS.
3. Confira rota de saída e políticas que permitam o tráfego exigido.
4. Valide o estado das licenças e das assinaturas.
5. Confirme endpoint, porta e transporte na documentação da versão.
6. Use `diagnose debug rating` quando o problema envolver serviços de rating, como Web Filter.

## Relações

- [[314_Roteamento]]
- [[317_Backup_e_restore]]
- [[318_Upgrade]]