---
id: 202611070902
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - vdom
  - segmentacao
  - administracao
---

# VDOM — Virtual Domain

Uma VDOM (*Virtual Domain*) divide um FortiGate físico em múltiplos firewalls lógicos. Cada VDOM mantém contextos próprios de configuração, incluindo políticas de firewall e tabela de roteamento.

## Para que serve

VDOMs podem ser usadas para:

- Separar departamentos, clientes, filiais ou ambientes.
- Isolar políticas e configurações.
- Delegar a administração de uma parte do equipamento.
- Consolidar vários contextos de firewall em um único appliance.

## Como funciona o isolamento

Por padrão, o tráfego processado por uma VDOM não passa automaticamente para outra. Cada VDOM tem seu próprio contexto de roteamento e suas próprias políticas.

O isolamento não significa que os recursos físicos do appliance sejam independentes: VDOMs compartilham o hardware e os recursos do FortiGate. Considere capacidade, licenciamento e desenho operacional ao planejar a quantidade de VDOMs.

## Habilitar multi-VDOM

O material da aula apresenta este comando:

```shell
config system global
    set vdom-mode multi-vdom
end
```

> A alteração do modo de VDOM pode afetar a configuração e o gerenciamento do equipamento. Faça backup, revise a documentação da versão e planeje a mudança antes de aplicá-la.

O número máximo de VDOMs depende do modelo, do licenciamento e da versão. O valor citado no material é **até 10 VDOMs por padrão**, com possibilidade de capacidade adicional em alguns modelos e licenças. Confirme os limites aplicáveis ao equipamento.

## Tráfego entre VDOMs

Para permitir comunicação entre VDOMs, o desenho normalmente requer:

1. **VDOM link**: cria a ligação lógica entre duas VDOMs.
2. **Endereçamento e rotas**: cada lado precisa conhecer a rede alcançável pelo outro.
3. **Políticas de firewall**: o tráfego precisa ser permitido nas VDOMs envolvidas.

O VDOM link conecta os contextos, mas não substitui o roteamento nem as políticas.

## Interfaces e VDOMs

Interfaces físicas, subinterfaces VLAN e VDOMs precisam ser associadas de acordo com a topologia. Por exemplo, uma interface física pode estar em `root` e uma subinterface VLAN associada a outra VDOM, como `PARCEIROS`, dependendo do projeto e dos recursos da versão.

Exemplo conceitual:

- `port1` → VDOM `root`
- `port1.100` → VDOM `PARCEIROS`

Consulte [[313_VLANs]] para a relação entre VLANs e interfaces lógicas.

## Administração de VDOMs

O perfil `prof_admin` permite administrar uma VDOM sem conceder acesso para alterar configurações globais. Isso é útil quando a administração é delegada por contexto.

Consulte [[316_Acesso_Administrativos]] para perfis, privilégios e contas administrativas.

## Checklist de troubleshooting entre VDOMs

1. Confirme que o tráfego precisa mesmo cruzar entre VDOMs.
2. Verifique se existe um VDOM link entre os contextos envolvidos.
3. Confira os endereços configurados nas extremidades do link.
4. Verifique se há rotas para as redes de destino em cada VDOM.
5. Revise as políticas de firewall em cada sentido necessário.
6. Confirme a associação de interfaces e VLANs às VDOMs corretas.
7. Verifique se o modelo e o licenciamento suportam a configuração planejada.

## Relações

- [[313_VLANs]]
- [[314_Roteamento]]
- [[316_Acesso_Administrativos]]
- [[317_Backup_e_restore]]