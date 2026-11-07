---
id: 202611070801
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - administracao
  - acesso-inicial
---

# Acesso inicial e administração do FortiGate

O FortiGate pode ser administrado pela **GUI**, pela **CLI** ou por ferramentas de gerenciamento compatíveis. A configuração de fábrica varia conforme o modelo e o modo de implantação.

## Acesso inicial

Em muitos appliances, o endereço IP padrão é `192.168.1.99/24`.

- Em modelos **mid-range** e **high-end**, esse IP costuma estar na interface `mgmt`.
- Em modelos **entry-level**, pode estar em `port1` ou na interface `internal`.
- **PING, HTTPS e SSH** podem estar habilitados por padrão.
- Alguns modelos de entrada também oferecem um servidor DHCP padrão em `port1` ou `internal`.

> Confirme o comportamento do modelo específico antes de conectar o equipamento a uma rede existente.

### FortiGate-VM

O FortiGate-VM normalmente não vem com um endereço IP estático padrão. A interface `port1` pode iniciar em modo DHCP. Para definir um endereço estático pela CLI:

```shell
config system interface
    edit port1
        set mode static
        set ip 192.168.1.99/24
    next
end
```

## Credenciais e segurança inicial

O usuário inicial costuma ser `admin`, sem senha. No primeiro acesso, o FortiGate solicita a definição de uma senha.

- Defina uma senha forte imediatamente.
- Evite compartilhar uma conta administrativa entre pessoas.
- Crie uma conta individual para cada administrador, para melhorar a rastreabilidade.

## Métodos de administração

### GUI

Acesso por navegador, geralmente via HTTPS. É necessária conectividade IP até uma interface que permita gerenciamento.

### CLI

Pode ser acessada por:

- Console do próprio equipamento, sem depender da rede.
- SSH pela rede — geralmente preferível ao Telnet.
- Widget de CLI disponível na GUI.

A CLI oferece comandos avançados e diagnósticos que podem não estar disponíveis na GUI. Relatórios, por sua vez, são visualizados pela GUI.

> O material da aula descreve SNMP como somente leitura no contexto apresentado; verifique a versão e a configuração antes de aplicar essa informação a um ambiente específico.

## Perfis administrativos

- `super_admin`: acesso global amplo; é o perfil associado à conta `admin` por padrão e não pode ser alterado.
- `prof_admin`: administração ampla dentro de uma VDOM, sem permissão para alterar configurações globais.
- Perfis personalizados: permissões **Read/Write**, **Read-Only** ou **None** por área.

Também é possível usar autenticação local ou remota, como LDAP, RADIUS e TACACS+, além de opções baseadas em certificados, conforme a configuração do ambiente.

## Restringir o acesso administrativo

### Trusted Hosts

Restringem os endereços IP ou as redes de origem que podem usar uma determinada conta administrativa.

- Exemplo: `192.168.99.0/24` permite o acesso a partir dessa rede.
- `0.0.0.0/0` representa qualquer origem e é menos restritivo.

Configure esses limites com cuidado para evitar perder o acesso de administração.

### Boas práticas

- Prefira **HTTPS e SSH** a protocolos sem criptografia.
- Restrinja o acesso administrativo por interface e por origem.
- Use contas individuais, perfis de menor privilégio e senhas fortes.
- Avalie o timeout de inatividade: o padrão indicado no material é de **5 minutos**.
- Revise periodicamente os protocolos de gerenciamento habilitados em cada interface.

## Comandos úteis

```shell
get system status
```

Exibe informações do sistema, incluindo a versão do FortiOS.

## Relações

- [[312_DHCP]]
- [[313_VLANs]]
- [[331_Roteamento]]
- [[315_VDOM]]
- [[316_Acesso_Administrativos]]
- [[317_Backup_e_restore]]
- [[318_Upgrade]]
