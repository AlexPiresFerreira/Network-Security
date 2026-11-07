---
tags:
  - fortinet
  - fortigate
  - autenticacao
  - autenticacao-local
---

# Autenticação local no FortiGate

## Conceito

Na autenticação local, as contas de usuário são armazenadas no próprio FortiGate, que verifica as credenciais durante o login.

É o método mais simples de configurar e pode ser adequado para ambientes pequenos ou instalações com um único FortiGate.

## Criação e uso de usuários locais

Os usuários locais podem ser criados em:

**User & Authentication > User Definition**

Durante a configuração, podem ser associados ao usuário:

- Nome de usuário e senha.
- E-mail ou número de telefone, quando aplicável.
- Autenticação de dois fatores.
- Grupo de usuários.

Depois de criar o usuário ou grupo, ele pode ser incluído em uma política de firewall que exija autenticação.

## Captive Portal

Em políticas com autenticação ativa, o usuário fornece suas credenciais por meio do **Captive Portal**.

Para iniciar a autenticação, a política precisa permitir pelo menos um dos seguintes protocolos:

- HTTP
- HTTPS
- FTP
- Telnet

O tráfego DNS pode ser permitido antes da autenticação, desde que o serviço DNS esteja incluído na política.

## Boas práticas

- Use senhas fortes e exclusivas.
- Organize usuários em grupos quando isso facilitar a aplicação das políticas.
- Ative 2FA quando disponível e adequado ao ambiente.
- Conceda apenas o acesso necessário ao usuário.
- Revise periodicamente os usuários e grupos configurados.

## Resumo

- As contas ficam armazenadas no próprio FortiGate.
- A autenticação pode ser aplicada por meio de políticas de firewall.
- O Captive Portal solicita as credenciais ao usuário.
- Usuários e grupos locais podem ser associados às políticas.

