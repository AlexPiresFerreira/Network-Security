
## Autemticação Ativa 

- POP3, LDAP, RADIUS, LOCAL e TACACS
- Captive Portal
- DNS não precisa de autenticação
- Protocolos liberado
	- HTTP
	- HTTPS
	- FTP
	- TELNET
- Autenticação sob demanda forçada(Padrão: Implicity)

```
config user setting
	set auth-on-demand always
```

## Autenticação Passiva

- FSSO(Fortinet Single Sign On)
- RSSO(RADIUS Single Sign On)
- NTLM(New Technology LAN Manager) - Microsoft

## TimeOut

Inativo(idle): 5m
Hard
Nova sessão

```
FGT-MTZ # config user setting

FGT-MTZ (setting) # set auth-timeout-type
idle-timeout    Idle timeout.
hard-timeout    Hard timeout.
new-session     New session timeout.

FGT-MTZ (setting) # set auth-timeout
auth-timeout    Enter an integer value from <1> to <1440> (default = <5>).
```

- Monitora
	- Dashboard > Assets & Identities > Firewall Users

