---
tags:
  - fortinet
  - fortigate
  - autenticacao
---

# Autenticação no FortiGate

## Conceito

A autenticação de firewall identifica usuários ou grupos para que o FortiGate possa aplicar políticas e perfis de segurança de acordo com a identidade autenticada.

Ela acrescenta uma camada de controle além de critérios como endereço IP ou tipo de dispositivo.

## Métodos de autenticação

### Autenticação local

- As contas e senhas são armazenadas no próprio FortiGate.
- Indicada principalmente para ambientes pequenos ou equipamentos isolados.
- Ver também: [[342_Autenticacao_Local]]

### Autenticação remota

As credenciais são verificadas em um servidor externo. A apostila cita:

- **POP3**
- **LDAP / LDAPS**
- **RADIUS**
- **TACACS+**

O FortiGate pode consultar um servidor remoto para autenticar usuários ou associar servidores/grupos remotos a grupos de usuários do firewall.

- Ver também: [[343_Autenticao_Remorta]]

### Autenticação de dois fatores (2FA)

Combina dois fatores independentes:

- **Algo que você sabe:** senha ou PIN.
- **Algo que você tem:** token ou certificado.

O FortiToken pode gerar senhas de uso único (**OTP**).

- Ver também: [[344_2FA_e_FortiToken]]

## Autenticação ativa

Na autenticação ativa, o usuário precisa informar suas credenciais, normalmente por meio de um **Captive Portal**.

Métodos citados:

- POP3
- LDAP
- RADIUS
- Local
- TACACS+

### Protocolos que podem iniciar o Captive Portal

Para que a tela de autenticação seja apresentada, a política precisa permitir pelo menos um dos protocolos abaixo:

- HTTP
- HTTPS
- FTP
- Telnet

Enquanto o usuário não estiver autenticado, os demais protocolos não são liberados por esse mecanismo.

### DNS antes da autenticação

O tráfego DNS pode ser permitido antes da autenticação para que a resolução de nomes necessária aos protocolos de autenticação funcione. O serviço DNS deve estar incluído explicitamente na política.

### Políticas mistas e autenticação sob demanda

Habilitar autenticação em uma política nem sempre força imediatamente a abertura do Captive Portal. As opções citadas no material incluem:

- Habilitar autenticação nas políticas que podem corresponder ao tráfego.
- Configurar autenticação sob demanda pela CLI.
- Habilitar Captive Portal diretamente na interface de origem.

A autenticação sob demanda pode ser configurada assim:

```bash
config user setting
    set auth-on-demand always
end
```

Se forem usadas autenticações passivas e ativas em conjunto, coloque as políticas de autenticação passiva antes das políticas de autenticação ativa. Assim, o FortiGate pode usar a autenticação ativa como alternativa quando a passiva não identificar o usuário.

## Autenticação passiva

Na autenticação passiva, o usuário não precisa preencher uma tela de login do FortiGate. A identidade é obtida automaticamente, conforme o método utilizado.

Métodos citados:

- **FSSO** — Fortinet Single Sign-On
- **RSSO** — RADIUS Single Sign-On
- **NTLM** — New Technology LAN Manager

## Timeout de autenticação

O timeout define quando o usuário precisará autenticar novamente. O valor padrão indicado na apostila é **5 minutos**.

Tipos de timeout:

- **Idle timeout:** expira após o período configurado sem atividade. É o padrão.
- **Hard timeout:** expira após o tempo configurado, mesmo que exista atividade.
- **New session timeout:** expira se nenhuma nova sessão for iniciada durante o intervalo configurado.

Configuração e consulta pela CLI:

```text
config user setting

set auth-timeout-type
    idle-timeout
    hard-timeout
    new-session

set auth-timeout
```

O valor de `auth-timeout` aceita um número inteiro de **1 a 1440 minutos**; o padrão indicado no CLI é **5 minutos**.

## Monitoramento de usuários autenticados

Na GUI, os usuários podem ser acompanhados em:

**Dashboard > Assets & Identities > Firewall Users**

Essa tela permite consultar informações como usuário, endereço IP, grupo, duração da sessão, volume de tráfego e método de autenticação. Também é possível desconectar usuários.

## Relação com políticas de firewall

Usuários e grupos podem ser usados como origem em políticas. Após uma autenticação bem-sucedida, o FortiGate aplica as políticas correspondentes à identidade do usuário.

As contas podem ser:

- Locais no FortiGate.
- Verificadas em servidores remotos, como LDAP ou RADIUS.
- Associadas a grupos locais ou remotos.
- Baseadas em certificado, conforme o método configurado.

## Ideia central

A autenticação permite que o FortiGate aplique controles de acesso baseados na identidade do usuário. Ela pode ser ativa, quando o usuário fornece credenciais, ou passiva, quando a identidade é obtida automaticamente.