
## LDAP

- Active Directory
- Samba

- LDAP 389, LDAPS 636

- Ex.
	- abc.com
		- dc=abc,dc=com

OU = Unidade Organizacional
CN = Grupo de Usuário
uid = Usuário e diversas outras

- Teste CLI
  
  diagnose test authserver ldap <dominio> <user> <senha>

```mermaid
flowchart TB
    A["dc=abc,dc=com"] --> B["ou=pessoas"]
    B --> C["cn=Matheus Azzi<br/>uid: mazzi<br/>sAMAccountName: mazzi"]
    B --> D["cn=user2<br/>uid: userid2<br/>sAMAccountName: userid2"]
    B --> E["cn=user<br/>uid: useridn<br/>sAMAccountName: useridn"]
```

## RADIUS

```mermaid
flowchart LR
    U["Usuário"] <--> F["Cliente RADIUS<br/>(FortiGate)"]
    F -->|"ACCESS-REQUEST"| S["Servidor RADIUS"]
    S -->|"ACCESS-ACCEPT"| F
    S -->|"ACCESS-REJECT"| F
    S -->|"ACCESS-CHALLENGE"| F
```

- ACCESS_ACCEPT
- ACCESS-REJECT
- ACCESS-CHALLENGE

- Método de autenticação
	- PAP
	- MSCHAP2
	- CHAP

diagnose test authserver radius <servidor> <metodo> <user> <senha>