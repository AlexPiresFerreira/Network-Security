
---
id: 202611070903
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - administracao
  - seguranca
  - cli
---

# Acesso e administração do FortiGate

O FortiGate pode ser administrado pela GUI, pela CLI e, para monitoramento, por protocolos como SNMP. Os métodos disponíveis dependem da configuração das interfaces, das permissões da conta e da versão do FortiOS.

## GUI e CLI

### GUI

A GUI é acessada por navegador, normalmente via HTTPS. Ela oferece acesso a configurações, monitoramento e relatórios.

### CLI

A CLI pode ser acessada por:

- Porta console, sem depender da conectividade de rede.
- SSH pela rede.
- Widget de CLI na GUI.
- Telnet, quando habilitado — evite em redes não confiáveis, pois não oferece a mesma proteção de SSH.

A CLI inclui comandos avançados e ferramentas de diagnóstico. O material da aula destaca que relatórios são visualizados pela GUI. SNMP é apresentado como uma opção de monitoramento somente leitura no contexto estudado; verifique a configuração e os recursos da versão.

## Acesso inicial

O usuário inicial costuma ser `admin`, sem senha. Na primeira autenticação, o FortiGate solicita a criação de uma senha.

- Troque a senha inicial imediatamente.
- Use uma senha forte e exclusiva.
- Evite usar a conta `admin` no trabalho cotidiano, quando for possível criar contas individuais.

O IP padrão e a interface de acesso variam conforme o modelo. Consulte [[311_IP_padro_acesso_e_VMs]] antes de tentar o acesso inicial.

## Contas administrativas

É recomendável criar uma conta individual para cada administrador. Isso melhora a rastreabilidade e permite conceder permissões apropriadas para cada função.

Conforme a configuração, o FortiGate pode usar:

- Usuários administrativos locais.
- Autenticação remota, como LDAP, RADIUS ou TACACS+.
- Autenticação baseada em certificados, quando configurada.

## Perfis de administrador

Os perfis controlam quais áreas um administrador pode consultar ou alterar.

- **`super_admin`**: perfil de acesso global amplo; associado por padrão à conta `admin` e não editável.
- **`prof_admin`**: administração ampla dentro de uma VDOM, sem permissão para alterar configurações globais.
- **Perfis personalizados**: podem conceder `Read/Write`, `Read-Only` ou `None` por área.

Aplique o princípio do menor privilégio: conceda somente as permissões necessárias para a função.

## Trusted Hosts

Trusted Hosts restringem os endereços IP ou as redes de origem que podem usar uma conta administrativa.

- Exemplo: `192.168.99.0/24` limita o acesso àquela rede.
- `0.0.0.0/0` permite qualquer origem IPv4 e é menos restritivo.

Cada conta pode ter sua própria lista. Antes de restringir o acesso, confirme que o endereço de administração está incluído para não perder o acesso ao equipamento.

## Protocolos administrativos por interface

Os serviços de administração são habilitados por interface. Assim, uma interface pode permitir HTTPS e SSH, enquanto outra não permite acesso administrativo.

- Habilite somente os protocolos necessários.
- Prefira HTTPS e SSH.
- Desabilite HTTP e Telnet quando não forem necessários.
- Revise separadamente as opções de gerenciamento IPv4 e IPv6.
- Não confunda protocolos de gerenciamento com protocolos de tráfego encaminhado pelo firewall.

O material também cita opções relacionadas à Security Fabric e à descoberta/integração de dispositivos, como CAPWAP, FortiTelemetry, FMG-Access, FTM, RADIUS Accounting e LLDP. Habilite esses serviços somente quando forem necessários ao desenho.

## Portas e timeout

- As portas de acesso administrativo podem ser personalizadas.
- O timeout de inatividade citado no material é de **5 minutos** por padrão.
- O timeout pode ser ajustado globalmente ou, em alguns cenários, substituído por perfil administrativo.

Um timeout menor reduz o período em que uma sessão inativa permanece aberta, mas pode tornar tarefas de diagnóstico mais inconvenientes. Equilibre segurança e necessidade operacional.

## Política de senha

Uma política de senha pode exigir critérios como:

- Quantidade mínima de caracteres.
- Quantidade mínima de caracteres novos em relação à senha anterior.
- Letras maiúsculas e minúsculas, números e caracteres especiais.
- Histórico de senhas.
- Expiração de senha.

Os critérios disponíveis dependem da configuração e da versão do FortiOS.

## Boas práticas

- Crie contas individuais para administradores.
- Use autenticação forte e uma política de senha adequada.
- Restrinja Trusted Hosts e o acesso administrativo por interface.
- Prefira HTTPS e SSH; evite Telnet e HTTP em redes não confiáveis.
- Use perfis com menor privilégio.
- Revise periodicamente contas, permissões e protocolos habilitados.
- Mantenha um caminho de acesso de contingência, como console, especialmente durante mudanças de rede.

## Relações

- [[311_IP_padro_acesso_e_VMs]]
- [[315_VDOM]]
- [[317_Backup_e_restore]]
- [[318_Upgrade]]