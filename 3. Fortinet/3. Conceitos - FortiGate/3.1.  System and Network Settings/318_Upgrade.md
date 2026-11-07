---
id: 202611071002
tipo: procedimento
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - fortios
  - upgrade
  - firmware
---

# Upgrade do FortiOS

Um upgrade altera a versão do FortiOS instalada no FortiGate. Como a mudança pode afetar recursos, compatibilidade e disponibilidade do serviço, deve ser planejada com antecedência.

## Verificar a versão instalada

A versão atual pode ser consultada no Dashboard, na seção de firmware da GUI ou pela CLI:

```shell
get system status
```

## Planejamento pré-upgrade

Antes de atualizar:

1. **Consulte as Release Notes** da versão de destino.
2. Verifique o **Upgrade Path** recomendado para o modelo e a versão atual.
3. Leia os **Known Issues** e avalie se afetam o ambiente.
4. Confirme compatibilidade de recursos, modelos e equipamentos integrados.
5. Faça e valide um backup da configuração: [[317_Backup_e_restore]].
6. Planeje uma janela de manutenção e um procedimento de recuperação.

> Não escolha o caminho de atualização apenas pela versão mais recente. O caminho suportado e as instruções podem variar conforme modelo e versão.

## Métodos de atualização

Conforme o registro, o licenciamento e os recursos disponíveis, o FortiGate pode permitir:

- Download e instalação pela GUI, conectando-se diretamente à Fortinet.
- Upload manual de um arquivo de firmware obtido no portal de suporte.
- Agendamento do upgrade em alguns equipamentos, conforme o modelo e os recursos disponíveis.

A disponibilidade de notificações, downloads e opções de agendamento pode variar. Confirme os requisitos atuais do equipamento antes de iniciar.

## Pós-upgrade

Após a atualização:

- Confirme a versão com `get system status`.
- Verifique interfaces, rotas, políticas, VPNs e serviços críticos.
- Confirme o estado das licenças e das atualizações do FortiGuard.
- Consulte novamente as Release Notes se algum comportamento tiver mudado.

## Checklist rápido

- [ ] Modelo e versão atual identificados.
- [ ] Release Notes da versão de destino revisadas.
- [ ] Upgrade Path confirmado.
- [ ] Known Issues avaliados.
- [ ] Backup recente disponível.
- [ ] Janela de manutenção e plano de recuperação definidos.
- [ ] Serviços críticos verificados após o upgrade.

## Relações

- [[317_Backup_e_restore]]
- [[319_FortiGuard_Distribution_Network_FDN]]
- [[311_IP_padro_acesso_e_VMs]]