---
id: 202611070901
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - roteamento
  - ospf
  - bgp
---

# Roteamento no FortiGate

O roteamento determina por qual interface e próximo salto (*next hop*) um pacote será encaminhado. No modo **NAT**, o FortiGate atua como roteador de camada 3 e pode encaminhar tráfego entre redes IPv4 e IPv6.

A decisão de rota afeta o tráfego que atravessa o FortiGate e o tráfego originado pelo próprio equipamento. O roteamento não substitui as políticas de firewall: uma rota define o caminho possível, enquanto a política controla se o tráfego é permitido.

## Como o FortiGate escolhe uma rota

A seleção ocorre em etapas:

1. **Maior correspondência de prefixo**: entre rotas que correspondem ao destino, é preferida a mais específica.
2. **Distância administrativa**: entre rotas para o mesmo prefixo, a menor distância costuma ser preferida.
3. **Critérios do protocolo e da rota**: podem incluir métrica e prioridade, conforme o tipo de rota e a configuração.
4. **ECMP**: se múltiplas rotas elegíveis tiverem custo equivalente, o FortiGate pode instalar mais de um caminho.

> Não confunda distância, métrica e prioridade. A distância compara a preferência entre fontes de rota; a métrica é, em geral, específica do protocolo; a prioridade pode participar do desempate de rotas estáticas e do ECMP.

## RIB e FIB

### RIB — Routing Information Base

A RIB é a base de informações de roteamento. Ela reúne rotas conectadas, estáticas e aprendidas por protocolos dinâmicos, como OSPF e BGP. A tabela de roteamento apresenta as rotas consideradas ativas pelo sistema.

```shell
get router info routing-table all
```

### FIB — Forwarding Information Base

A FIB é a estrutura usada pelo kernel para encaminhar pacotes. Ela contém as rotas instaladas para o encaminhamento, organizadas para permitir consultas rápidas.

```shell
get router info kernel
```

**Resumo:** a RIB representa as informações de roteamento; a FIB representa os caminhos instalados para encaminhar tráfego.

## Rotas estáticas e rota default

Rotas estáticas são configuradas manualmente. Podem ser usadas para alcançar redes específicas ou para definir uma rota padrão.

A rota default IPv4 é `0.0.0.0/0`. Ela é usada quando não existe uma rota mais específica para o destino. Para IPv6, a rota default é `::/0`.

Uma rota estática pode especificar:

- Destino e máscara/prefixo.
- Gateway ou próximo salto.
- Interface de saída.
- Distância administrativa e, quando aplicável, prioridade.
- Gateway dinâmico, em cenários com interface DHCP ou PPPoE.

### Distância e prioridade

- **Menor distância administrativa**: maior preferência entre rotas concorrentes para o mesmo destino.
- **Menor prioridade**: pode ser usada para desempatar rotas estáticas com distância equivalente, conforme a configuração.
- Rotas menos preferidas podem permanecer configuradas sem serem selecionadas para encaminhamento.

| Tipo de rota         | Distance |
| -------------------- | -------- |
| Connected            | 0        |
| Static (SD-WAN zone) | 1        |
| Static (DHCP)        | 5        |
| Static (Manual)      | 10       |
| Static (IKE)         | 15       |
| EGBP                 | 20       |
| OSPF                 | 110      |
| IS-IS                | 115      |
| RIP                  | 120      |
| IBGP                 | 200      |

## ECMP — Equal-Cost Multi-Path

ECMP permite instalar múltiplas rotas de custo equivalente para o mesmo destino. O tráfego pode ser distribuído entre os caminhos disponíveis.

O método de distribuição define como as sessões são associadas aos caminhos. Entre os modos citados no material estão:

- `source-ip-based`: considera o IP de origem.
- `source-dest-ip-based`: considera os IPs de origem e destino.
- `weight-based`: distribui conforme pesos configurados; o suporte depende do tipo de rota e da configuração.
- `usage-based`: usa limiares de utilização para direcionar tráfego a outro caminho quando necessário.

Exemplo de configuração citado nas aulas:

```shell
config system settings
    set v4-ecmp-mode [source-ip-based | source-dest-ip-based | weight-based | usage-based]
end
```

> A sintaxe e os modos disponíveis podem variar conforme a versão do FortiOS. Consulte a referência da versão em uso antes de aplicar comandos.

### ECMP e SD-WAN

SD-WAN e ECMP tradicional são mecanismos diferentes de seleção de caminhos. Quando o SD-WAN está habilitado, parte do comportamento de balanceamento é configurada no contexto de SD-WAN, e não apenas pelas opções globais de ECMP.

- O SD-WAN seleciona caminhos com base em regras, membros e critérios de desempenho configurados.
- O ECMP tradicional distribui tráfego entre rotas de custo equivalente na tabela de roteamento.
- Os parâmetros e algoritmos disponíveis dependem da versão e da configuração.

## Policy routes

Policy routes permitem selecionar um caminho considerando critérios adicionais, como origem, destino, protocolo, portas e interface de entrada.

O resultado da consulta de rota pode depender de quais critérios foram informados. Ao investigar um fluxo, verifique se ele corresponde a uma policy route ou a uma regra de SD-WAN, além da tabela de roteamento convencional.

## RPF — Reverse Path Forwarding

RPF ajuda a proteger contra falsificação de IP (*IP spoofing*), verificando se existe um caminho de retorno plausível para o endereço de origem do pacote.

- **Feasible path**: permite a validação por um caminho de retorno elegível; não exige necessariamente que seja a melhor rota.
- **Strict**: exige que o caminho de retorno corresponda à melhor rota, sendo mais restritivo.
- Uma falha pode gerar mensagens como `reverse path check fail, drop`.

O comportamento exato depende da configuração de RPF e da topologia. Em ambientes assimétricos ou com múltiplos caminhos, valide o efeito antes de habilitar o modo estrito.

Comandos citados no material:

```shell
config system settings
    set strict-src-check enable
end
```

Também é possível ajustar a verificação por interface:

```shell
config system interface
    edit <interface>
        set src-check disable
    next
end
```

> Desabilitar a verificação de origem reduz uma proteção de segurança. Faça isso somente quando houver justificativa de projeto e uma avaliação dos riscos.

## OSPF

OSPF é um protocolo de roteamento dinâmico de estado de enlace. Os roteadores trocam LSAs, formam uma LSDB e executam o algoritmo SPF para calcular os caminhos.

### Conceitos e tipos de LSA

- **LSDB**: base de dados de estado de enlace usada no cálculo SPF.
- **LSA Type 1 — Router LSA**: descreve enlaces e informações do roteador dentro da área.
- **LSA Type 2 — Network LSA**: descreve uma rede multiacesso e é originada pelo DR.
- **LSA Type 3 — Summary LSA**: anuncia prefixos entre áreas, normalmente por um ABR.
- **LSA Type 4 — ASBR Summary LSA**: informa como alcançar um ASBR.
- **LSA Type 5 — External LSA**: anuncia rotas externas ao domínio OSPF.

### Papéis

- **ABR** (*Area Border Router*): conecta áreas OSPF.
- **ASBR** (*Autonomous System Boundary Router*): introduz ou anuncia rotas externas ao OSPF.
- **DR/BDR**: papéis eleitos em certos segmentos multiacesso para reduzir a quantidade de adjacências e trocas de LSAs.

Multicast IPv4 usado pelo OSPF:

- `224.0.0.5`: AllSPFRouters.
- `224.0.0.6`: AllDRouters.

### Verificação e troubleshooting

```shell
get router info ospf status
get router info ospf interface
get router info ospf neighbor
get router info ospf database brief
get router info ospf database self-originate
get router info ospf database router lsa
```

Para acompanhar debug em tempo real:

```shell
diagnose ip router ospf all enable
diagnose ip router ospf level info
diagnose debug enable
```

Para encerrar o debug, use `diagnose debug disable` e, quando necessário, desative as opções específicas de debug. Evite deixar debug ativo por mais tempo do que o necessário.

Para reiniciar o processo OSPF, o material cita:

```shell
execute router clear ospf process
```

> Reiniciar o processo pode interromper temporariamente as adjacências e a convergência. Confirme a sintaxe suportada na versão instalada antes de executar.

## BGP

BGP troca informações de roteamento entre sistemas autônomos e também pode ser usado em redes internas de grande porte.

### Visões de rotas

- **RIB-in**: rotas recebidas do vizinho antes ou durante a aplicação de filtros de entrada, conforme a representação da plataforma.
- **RIB**: rotas aceitas e processadas localmente.
- **RIB-out**: rotas selecionadas para anúncio ao vizinho.

Filtros e políticas de importação/exportação podem alterar quais rotas são aceitas ou anunciadas.

### Route Reflector

Um Route Reflector reduz a necessidade de uma malha completa de sessões iBGP. O vizinho configurado como cliente recebe rotas refletidas segundo as regras do BGP.

Exemplo citado no material:

```shell
config router bgp
    edit <neighbor IPv4 address>
        set route-reflector-client enable
    next
end
```

> A estrutura exata da configuração de vizinhos pode variar entre versões. Valide o contexto do comando na CLI do equipamento.

### Import check

O material cita a seguinte configuração:

```shell
config router bgp
    set network-import-check disable
end
```

Essa opção afeta a verificação associada à origem/anúncio de redes no BGP. Altere-a somente quando compreender o efeito sobre os prefixos anunciados.

### Verificação e troubleshooting

```shell
get router info bgp summary
get router info bgp neighbor
get router info bgp neighbor <neighbor> route
get router info bgp neighbor <neighbor> advertise
```

Para solicitar atualização de rotas sem derrubar a sessão, o material cita:

```shell
execute router clear bgp [soft in | soft out]
```

O uso de atualização *soft* e a forma exata do comando dependem da versão e do objetivo: reprocessar rotas recebidas ou atualizar os anúncios enviados.

Debug em tempo real, conforme o material:

```shell
diagnose ip router bgp all enable
diagnose ip router bgp level info
diagnose debug enable
```

Encerre o debug após coletar as informações necessárias.

## Checklist de troubleshooting de roteamento

1. Confirme o endereço de origem e destino do fluxo.
2. Verifique a rota correspondente ao destino e identifique a interface e o próximo salto.
3. Confira a rota default, se não houver rota mais específica.
4. Revise distância, métrica, prioridade e possíveis caminhos ECMP.
5. Verifique policy routes e regras SD-WAN aplicáveis ao fluxo.
6. Confirme se há falha de RPF ou assimetria de roteamento.
7. Para OSPF, confira interface, vizinhança, LSDB e estado das rotas.
8. Para BGP, confira o estado do peer, filtros, rotas recebidas e anúncios.
9. Revise também as políticas de firewall: ter uma rota não significa que o tráfego está permitido.

## Comandos rápidos

```shell
get router info routing-table all
get router info kernel
diagnose ip rtcache list
```

O cache de rotas pode ajudar em diagnósticos, mas sua relação com o encaminhamento varia conforme a versão e o fluxo. Não trate o cache como substituto da análise da tabela de roteamento e das regras de política.

## Relações

- [[312_DHCP]]
- [[313_VLANs]]
- [[315_VDOM]]
- [[316_Acesso_Administrativos]]
- [[319_FortiGuard_Distribution_Network_FDN]]