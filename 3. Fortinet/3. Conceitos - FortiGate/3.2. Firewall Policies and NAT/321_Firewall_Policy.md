---
id: 202611071101
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - firewall-policy
  - seguranca
  - logging
---

# Firewall Policies no FortiGate

Uma **Firewall Policy** define quais tráfegos podem atravessar o FortiGate e quais ações serão aplicadas a eles. A política identifica o tráfego por critérios como interfaces, endereços, serviços e horário, e determina se ele será permitido ou negado.

## Como as políticas são avaliadas

As políticas são avaliadas em ordem **top-down**, de cima para baixo:

1. O FortiGate compara o tráfego com a primeira política.
2. Se houver correspondência (*match*), aplica essa política e interrompe a busca.
3. Se nenhuma política corresponder, o tráfego é negado pela regra implícita **Implicit Deny**.

> A primeira política correspondente é aplicada. Uma regra mais específica abaixo não será considerada se o tráfego já corresponder a uma regra anterior.

## O que uma política pode determinar

Uma política pode definir:

- Se o tráfego será **permitido** ou **negado**.
- Quais perfis de segurança serão aplicados.
- Se será usado SNAT.
- Se haverá autenticação de usuários.
- Como o tráfego será registrado em logs.

Em uma política com ação `ACCEPT`, o tráfego ainda pode ser bloqueado por um perfil de segurança se uma inspeção detectar uma ameaça ou violação.

## Componentes de uma Firewall Policy

Os principais campos de uma política de tráfego encaminhado são:

| Campo | Função |
|---|---|
| **Incoming Interface** | Interface ou zona pela qual o tráfego entra |
| **Outgoing Interface** | Interface ou zona pela qual o tráfego sai |
| **Source** | Origem do tráfego, como endereço, usuário ou serviço de Internet |
| **Destination** | Destino do tráfego |
| **Service** | Protocolo e portas que devem corresponder |
| **Schedule** | Período em que a política está ativa |
| **Action** | Ação: `ACCEPT` ou `DENY` |

Na GUI, o nome da política normalmente é solicitado. O material da aula informa que a exigência de nomes pode ser alterada em **Feature Visibility**. A obrigatoriedade e as opções disponíveis podem variar conforme a versão.

### Schedule

O agendamento define quando a política pode ser usada.

- `always`: política ativa continuamente, 24 horas por dia e 7 dias por semana.
- Agendamentos personalizados: limitam a política a dias e horários específicos.

### Action

- **`ACCEPT`**: permite que o tráfego prossiga para as demais verificações configuradas, como perfis de segurança.
- **`DENY`**: nega o tráfego que corresponder à política.

## Critérios de correspondência

De forma geral, o FortiGate avalia se o tráfego corresponde aos seguintes elementos:

1. Interfaces de entrada e saída.
2. Origem.
3. Destino.
4. Serviço.
5. Agendamento.
6. Ação configurada.

A política precisa corresponder ao fluxo real: interface de entrada, interface de saída, endereços, protocolo, portas e horário.

### Interfaces múltiplas e `any`

A GUI pode ocultar ou desabilitar opções para selecionar múltiplas interfaces ou `any`. O material da aula indica que essas opções podem ser habilitadas em **Feature Visibility**. A CLI pode oferecer opções adicionais, mas a sintaxe e o suporte dependem da versão.

## Objetos de origem e destino

### Origem

Uma política pode usar objetos como:

- Endereço IP individual ou intervalo de IPs.
- Sub-rede no formato IP/máscara.
- FQDN — o FortiGate precisa conseguir resolver o nome.
- GeoIP.
- Endereço dinâmico, por exemplo, associado a um Fabric Connector.
- Intervalo de endereços MAC.
- Usuário individual ou grupo de usuários.
- Internet Service Database (ISDB), conforme a configuração.

Usuários e grupos podem ser locais ou associados a serviços remotos, como Active Directory, LDAP ou RADIUS. O material também cita o uso de FSSO.

### Destino

O destino pode ser especificado por objetos como:

- Endereço IP, sub-rede ou intervalo de IPs.
- FQDN.
- GeoIP.
- Endereço dinâmico.
- ISDB.

A identificação de usuário não é configurada como objeto de destino na política de encaminhamento descrita na aula: ela é associada à origem e à autenticação do usuário.

> GeoIP e ISDB dependem de informações atualizadas da FortiGuard. Verifique licenças e conectividade quando esses objetos não funcionarem como esperado.

## Services

Um objeto de serviço define os protocolos e as portas que podem corresponder à política. O material cita:

- TCP, UDP e SCTP.
- ICMP.
- IP.

Conforme o protocolo, o serviço pode especificar:

- Número de protocolo.
- Porta de origem e/ou de destino.
- Intervalo de portas.
- Tipo e código, no caso de ICMP.

Use serviços específicos sempre que possível, em vez de liberar tráfego desnecessário.

## Tipos de políticas

O FortiGate oferece diferentes tipos de políticas, cada uma destinada a uma categoria de tráfego:

- **Firewall Policy IPv4/IPv6**: controla tráfego encaminhado pelo FortiGate.
- **Virtual Wire Pair Policy**: controla tráfego entre interfaces de um virtual wire pair.
- **Proxy Policy**: controla tráfego processado pelo proxy, conforme os recursos configurados.
- **Multicast Policy IPv4/IPv6**: controla tráfego multicast.
- **Local-in Policy**: controla tráfego destinado ao próprio FortiGate, como acesso administrativo e determinados serviços locais.
- **DoS Policy IPv4/IPv6**: monitora e limita padrões de tráfego associados a ataques de negação de serviço.
- **Traffic Shaping Policy**: controla ou prioriza largura de banda e tráfego.

> Uma Local-in Policy é diferente de uma política de tráfego encaminhado: ela trata de tráfego cujo destino é o próprio FortiGate.

## Policy List, IDs e ordem

Cada política recebe um **ID único** quando é criada. O ID permanece associado à política mesmo que sua posição na lista seja alterada.

O material descreve estas visualizações:

- **Interface Pair View**: agrupa políticas de acordo com o par de interfaces de entrada e saída.
- **Sequence View**: mostra as políticas na ordem de processamento.

Para alterar a ordem:

- Na GUI, pode-se usar arrastar e soltar (*drag-and-drop*).
- Pela CLI, pode-se usar o comando `move`, conforme a sintaxe disponível na versão.

A combinação de políticas pode ser considerada quando elas têm critérios e comportamento semelhantes. Antes de combinar regras, avalie diferenças em perfis de segurança, autenticação e logging.

## Security Profiles

Perfis de segurança são aplicados às políticas com ação `ACCEPT`. Eles ajudam a detectar e bloquear ameaças, controlar aplicações e URLs e, em alguns cenários, reduzir o risco de vazamento de dados.

Perfis citados na aula:

- Antivirus.
- Web Filter.
- DNS Filter.
- Application Control.
- Intrusion Prevention System (IPS).
- SSL Inspection.
- Video Filter.
- File Filter.
- Email Filter.
- VoIP.
- Web Application Firewall.
- Inline CASB.
- Data Loss Prevention (DLP).

Alguns perfis podem não estar visíveis na GUI por padrão e podem depender de habilitação em **Feature Visibility**, licença ou versão.

## Modos de inspeção

### Flow-based

No modo **flow-based**, o tráfego é inspecionado durante o processamento do fluxo.

- Em geral, prioriza desempenho e throughput.
- Pode usar menos recursos do que o modo proxy em cenários comparáveis.
- Algumas funções avançadas podem não estar disponíveis.

### Proxy-based

No modo **proxy-based**, o FortiGate pode armazenar temporariamente o tráfego em memória e analisá-lo de forma mais completa antes de tomar uma decisão.

- Permite determinados recursos avançados, dependendo do perfil e da versão.
- Pode consumir mais CPU e memória.
- Deve ser considerado no dimensionamento do equipamento.

A aula cita recursos como quota, Safe Search, Web Profile Override, Video Filter, Inline CASB, ICAP, WAF e DLP entre as funções associadas ao modo proxy. A disponibilidade exata varia por recurso, modelo e versão.

> Em alguns equipamentos de entrada, o modo proxy pode não aparecer na GUI por padrão. Confirme o suporte e a forma de habilitação na documentação da versão antes de alterar a configuração.

## Logging em políticas

O logging registra informações sobre o tráfego e as ações do FortiGate.

### Tipos de registro citados

- **Security Events / UTM**: registra eventos associados aos perfis de segurança.
- **All Sessions**: registra as sessões de tráfego, conforme a configuração da política.
- Em políticas `DENY`, a opção citada é **Log Violation Traffic**.

O comportamento padrão descrito no material é registrar eventos de segurança. A opção `Generate Logs When Session Starts` pode gerar um log no início e outro no encerramento da sessão. Isso pode aumentar o uso de processamento e armazenamento; habilite-a quando houver necessidade operacional.

### Sessões negadas

A aula cita estas opções de CLI para controlar o registro de tráfego negado:

- `ses-denied-traffic`: relacionado ao registro das sessões negadas na tabela de sessões.
- `block-session-timer`: define por quanto tempo a sessão negada permanece na tabela; o material cita **30 segundos** como padrão.

A sintaxe e o contexto desses parâmetros podem variar conforme a versão. Consulte a referência da CLI antes de aplicar mudanças.

### Destinos e consulta de logs

Os logs podem ser enviados ou armazenados em:

- Disco ou memória local do FortiGate.
- FortiGate Cloud.
- FortiAnalyzer, local ou em nuvem.
- Syslog.

A GUI permite consultar logs de tráfego e eventos de segurança, aplicar filtros e escolher origem e período dos registros. O material cita também a opção **Show Matching Logs** na lista de políticas para localizar logs relacionados a uma regra específica.

## GeoIP, ISDB e Geographic-Based ISDB

### GeoIP

GeoIP representa uma região geográfica associada a um endereço IP. Pode ser usado como critério de origem ou destino, conforme a configuração.

### ISDB

A **Internet Service Database (ISDB)** contém informações mantidas pela Fortinet sobre serviços de Internet, como faixas de IP, protocolos e portas associados a serviços conhecidos.

### Geographic-Based ISDB

Combina a identificação de um serviço de Internet com uma região geográfica. A aula cita a possibilidade de selecionar país, região e cidade, de acordo com o recurso e a versão.

> GeoIP e ISDB dependem de bases de dados atualizadas. Confirme licenciamento, conectividade com a FortiGuard e atualização das bases quando os resultados forem inesperados.

## Boas práticas

- Faça backup da configuração antes de criar, alterar ou remover políticas: [[317_Backup_e_restore]].
- Teste mudanças em uma janela de manutenção.
- Quando possível, aplique a mudança inicialmente a um grupo pequeno de usuários ou endereços.
- Use políticas específicas para origem, destino e serviços necessários.
- Evite usar uma sub-rede mais ampla do que o necessário.
- Revise perfis de segurança e configuração de logs por política.
- Confira a ordem das regras: a primeira correspondência é aplicada.
- Avalie o impacto de alterações em objetos compartilhados por várias políticas.
- Considere que mudanças são aplicadas imediatamente e podem levar à reavaliação de sessões ativas.
- Acompanhe CPU, memória, sessões e logs após mudanças relevantes.

## Resumo para revisão

- Políticas são avaliadas **top-down**.
- A primeira regra correspondente é aplicada.
- Sem correspondência, o tráfego é negado pelo **Implicit Deny**.
- Uma política combina interfaces, origem, destino, serviço, agendamento e ação.
- `ACCEPT` permite que o tráfego prossiga para as inspeções configuradas; `DENY` nega o tráfego.
- Perfis de segurança são associados a políticas de permissão.
- A ordem, o escopo dos objetos e o logging afetam diretamente o comportamento e o diagnóstico.

## Relações

- [[322_NAT]]
- [[331_Roteamento]]
- [[316_Acesso_Administrativos]]
- [[317_Backup_e_restore]]
- [[319_FortiGuard_Distribution_Network_FDN]]