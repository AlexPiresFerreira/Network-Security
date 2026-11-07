
## 1. Conceito de Firewall Policy

- Política define:
    - **Que tráfego dá match** (interfaces, origem, destino, serviço, horário).
    - **O que fazer** com esse tráfego (ACCEPT/DENY, perfis de segurança, NAT, autenticação).
- Avaliação:
    - Ordem **top‑down**: começa da primeira policy e para na **primeira que casar**.
    - Se nenhuma casar → **regra implícita** (Implicit Deny) derruba tudo.
- Após o match:
    - Decide se **permite ou nega**.
    - Se permite, aplica perfis UTM (AV, Web Filter, etc.), NAT e eventualmente autenticação.

---

## 2. Componentes usados em políticas

- **Interfaces/Zonas** (incoming / outgoing).
- **Address objects** (IPs, subnets, FQDN, GeoIP, dynamic address).
- **Users / User groups** (locais, AD/LDAP/Radius, FSSO).
- **Internet Services / ISDB** (serviços conhecidos – Microsoft, Google etc.).
- **Services** (TCP/UDP/SCTP, ICMP, IP; portas, ranges, tipo/código).
- **Schedule** (Always ou horários específicos).
- **Security Profiles** (AV, Web/DNS Filter, App Control, IPS, SSL Inspection, etc.).

---

## 3. Tipos de políticas

- **Firewall Policy (IPv4/IPv6)** – política “normal” de L3/L4.
- **Virtual Wire Pair Policy** – controla tráfego entre interfaces em virtual wire.
- **Proxy Policy** – usada para tráfego HTTP/HTTPS em proxy com inspeção avançada.
- **Multicast Policy** – controla tráfego multicast.
- **Local‑in Policy** – controla tráfego destinado ao próprio FortiGate (admin, VPN, etc.).
- **DoS Policy** – protege contra ataques de negação de serviço.
- **Traffic Shaping Policy** – controla banda, prioridade de tráfego.

---

## 4. Criação e correspondência de políticas

- Em GUI:
    - Nome (único, opcional desativar exigência).
    - **Obrigatório definir**: incoming, outgoing, source, destination, service, schedule, action.
- Correspondência:
    - 1º: par de **interfaces**.
    - 2º: **origem** (endereços, usuários, GeoIP, ISDB, dynamic address, MAC range).
    - 3º: **destino** (endereços, ISDB, GeoIP; não usa usuário).
    - 4º: **serviço** (portas/protocolos).
    - 5º: **schedule** (Always ou janela).
- Múltiplas interfaces / any:
    - Por padrão desativado na GUI, pode ser habilitado; na CLI sempre disponível.

---

## 5. Policy List, IDs e organização

- Cada policy tem **ID único**, criado na criação e que não muda ao mover.
- Visões:
    - **Interface Pair View** – agrupadas por par de interfaces.
    - **Sequence View** – ordem real de processamento, com possibility de **sequence groups**.
- Mover policies:
    - GUI: drag‑and‑drop.
    - CLI: comando `move`.
- Combinação de policies:
    - Se origem/destino iguais, pode combinar serviços em uma policy (quando fizer sentido, ex. logging similar).

---

## 6. Security Profiles

- Perfis de segurança aplicados em policies com **Action = ACCEPT**:
    - **Antivirus**, **Web Filter**, **DNS Filter**, **Application Control**, **IPS**, **SSL Inspection**.
- Outros perfis (ativados via Feature Visibility):
    - **Video Filter**, **File Filter**, **Email Filter**, **VoIP**, **Web Application Firewall**, **Inline CASB**, **DLP**.
- Ideia central:
    - Bloquear ameaças, controlar aplicações/URLs, impedir vazamento de dados.

---

## 7. Modos de inspeção (Flow‑based x Proxy‑based)

- **Flow‑based** (padrão):
    - Inspeção em tempo real, usa menos recursos, maior throughput.
    - Nem todas as features avançadas disponíveis.
- **Proxy‑based**:
    - Bufferiza o tráfego, analisa o fluxo completo.
    - Permite recursos avançados:
    - quotas de Web Filter, safe search, web profile override, alguns perfis (Video Filter, Inline CASB, ICAP, WAF, DLP).
    - Mais pesado em CPU/memória.
- Em appliances menores (2 GB RAM ou menos), Proxy‑based pode não aparecer na GUI; habilita‑se via CLI.

---

## 8. Logging em políticas

- Por padrão:
    - Tráfego logado apenas para eventos de **segurança (UTM)**.
- Pode mudar para:
    - **All Sessions** – logar toda sessão.
- Em policies **DENY**:
    - Só existe opção de **Log Violation Traffic**.
- Modo de geração de logs:
    - Default: loga quando **a sessão termina**.
    - Opção: **Generate Logs When Session Starts** (loga no início e no fim):
    - Aumenta uso de CPU/armazenamento → usar só quando necessário.
    - Em appliances sem storage interno, opção aparece apenas via CLI.

### Logs de bloqueio

- Quando um perfil de segurança bloqueia algo, o log de ameaça é gerado na hora.
- Para reduzir logs e carga:
    - Habilitar `ses-denied-traffic` para registrar sessão negada na session table.
    - Parâmetro `block-session-timer` define quanto tempo a sessão negada fica na tabela (padrão 30s).

### Onde visualizar

- Destinos de log:
    - Disco/memória local, FortiGate Cloud, FortiAnalyzer (local/cloud), Syslog.
- GUI:
    - **Log & Report > Forward Traffic**, filtros por:
    - origem/destino, política, período, origem de log, etc.
- Atalho:
    - Na Policy List, botão direito **“Show Matching Logs”** abre logs daquela policy.

---

## 9. GEOIP, ISDB e Geo‑based ISDB

- **GeoIP**:
    - Objetos dinâmicos baseados em país/região/cidade; precisam do FortiGuard.
    - Ex.: permitir só origem Brasil em uma policy.
- **ISDB (Internet Service Database)**:
    - Banco de serviços (Meta, Google, Microsoft, TeamViewer, etc.) com IP ranges, portas e protocolos.
- **Geographic‑Based ISDB**:
    - Combina GeoIP + ISDB: restringe serviços de internet por região geográfica.
- Uso:
    - Podem ser usados em **origem e destino** das policies.
    - Referenciados por **nome** (config/CLI), não por ID.

---

## 10. NAT – conceitos gerais

- NAT = tradução de endereço IP; com tradução de porta → **PAT**.
- Benefícios:
    - Esconde IP real (segurança).
    - Conserva IPv4 público (CGNAT).
    - Permite flexibilidade de endereçamento interno.
- Tipos:
    - **SNAT** – traduz IP/porta de origem (configurado na Firewall Policy).
    - **DNAT** – traduz IP/porta de destino (via **VIP**).
    - **NAT64 / NAT46** – IPv6↔IPv4.
    - **NAT66** – NAT só IPv6.

---

## 11. SNAT – modos e IP Pools

### 11.1 SNAT usando IP da interface de saída

- Comportamento semelhante a “overload”:
    - Vários hosts internos compartilham o **mesmo IP público** (da interface).
    - Diferença entre sessões feita por **portas de origem**.
- Casos:
    - Portas diferentes → mantêm porta original no SNAT.
    - Mesma porta → FortiGate sorteia nova porta (evitar conflito).

### 11.2 IP Pools

- Definem um IP único ou range para NAT de origem.
- Em geral, IP/Range deve estar na mesma subnet da interface de saída (senão precisa de rota).
- Tipos:
    - **Overload** (padrão) – igual à interface, mas com IP(s) específicos.
    - **One‑to‑One**:
    - Um IP do pool por host interno (enquanto houver sessões).
    - Acabando IPs → hosts novos têm tráfego dropado.
    - **Fixed Port Range**:
    - Cada IP público tem suas portas divididas igualmente entre hosts internos.
    - Interessante para **CGNAT** (deterministic NAT por host).
    - **Port Block Allocation (PBA)**:
    - Portas do IP público são divididas em blocos de tamanho definido.
    - FortiGate entrega blocos conforme necessidade e **loga** qual bloco cada host usa.
    - Também muito útil para ISPs com CGNAT, pois dá rastreabilidade.

### 11.3 Session Clash (Overload)

- Com overload, NAT usa port translation.
- **Session clash** ocorre quando há **port exhaustion**:
    - Todas as portas altas para um mesmo destino estão ocupadas.
    - Começa a reutilizar mesma porta externa para mais de uma sessão.
- É raro (64k+ sessões para o mesmo IP), mas pode indicar malware/tráfego anômalo.
- Diagnóstico via **debug flow**.

---

## 12. DNAT – VIPs

- No FortiGate, DNAT é implementado com **Virtual IP (VIP)**.
- Tipo padrão: **Static NAT** (one‑to‑one), valendo tanto para:
    - Tráfego de entrada (DNAT → IP interno).
    - Tráfego de saída (SNAT usando IP externo do VIP).
- Pode referenciar:
    - IPs fixos ou **FQDN** (altere tipo para FQDN).
- **Port Forwarding**:
    - Mapeia IP/porta externa para IP/porta interna específicos.
    - Permite reutilizar o mesmo IP público em vários serviços.

### 12.1 Fluxos ilustrados

- Conexão de entrada:
    - Destino externo (VIP IP:porta) → traduz para IP interno:porta.
- Conexão de saída:
    - Host interno com IP privado → pode usar o IP do VIP como IP de saída.

---

## 13. Policies de DENY para VIPs (match‑vip)

- Problema:
    - Policies de deny com destinos genéricos (`all`, subnets etc.) **não casam em VIPs** por padrão.
    - Mesmo acima da policy do VIP, tráfego ao VIP continua pegando a policy de allow.
- Soluções:
    1. Habilitar `match-vip` na policy de deny (CLI).
    2. Usar o **próprio VIP** como destino na policy de deny.
- Mudança de comportamento:
    - A partir do **FortiOS 7.2.4**, novas policies de **Deny** vêm com `match-vip enable` por padrão (antes era `disable`).

---

## 14. ARP Reply em VIPs

- Por padrão, **ARP Reply está habilitado** nos VIPs:
    - FortiGate responde ARP pelo IP externo do VIP.
- Na maioria dos cenários não é estritamente necessário, mas:
    - Ajuda a resolver problemas de **roteamento incorreto** em ambientes de ISP.
    - É considerada **boa prática** manter ARP Reply ligado.

---

## 15. Boas práticas (Policies & NAT)

- **Policies**:
    
    - Sempre fazer **backup** antes de alterações.
    - Testar mudanças em **janela de manutenção**.
    - Se possível, testar com poucos usuários/IPs primeiro.
    - Cuidado ao editar/desativar/deletar policies/objetos:
    - Mudanças são aplicadas imediatamente.
    - Sessões ativas são reavaliadas → pode gerar pico de CPU/memória.
    - Criar policies **o mais específicas possível**:
    - Origem/destino/serviço bem definidos.
    - Usar subnets corretas (sem “atalhos” como /16 muito amplo).
    - Configurar adequadamente:
    - Perfis UTM
    - Logging (All / UTM / None) para cada policy.
- **NAT (SNAT/DNAT)**:
    
    - Conferir duas vezes **intervalos de IP Pool** (start/end).
    - Garantir que IPs do pool **não conflitam** com IPs do FortiGate nem de outros dispositivos.
    - Se usuários internos e externos acessam o mesmo servidor:
    - Configurar **DNS interno** para resolver para IP interno (evita hairpin NAT desnecessário).
    - Evitar VIPs desnecessários:
    - Só expor para internet o que for realmente preciso.
    - Fazer mudanças de NAT sempre em **janela de manutenção** e com testes adequados:
    - Configuração errada pode derrubar a rede.

**Ideia central:** explicar o que é uma firewall policy no FortiGate, tipos de políticas, como o tráfego dá match, como configurar e boas práticas, incluindo modos de inspeção, logging, GeoIP/ISDB e NAT (SNAT/DNAT).

### 1. Conceito e tipos de Firewall Policies

- Firewall policy responde:
    - **Que tráfego casa com a regra** (interfaces, origem, destino, serviço, horário).
    - **O que fazer** com esse tráfego (ACCEPT/DENY, perfis de segurança, NAT, autenticação, logging).
- Avaliação das policies é **top‑down**, com **Implicit Deny** no final.
- Tipos principais:
    - **IPv4/IPv6 policy** (tráfego normal).
    - **Virtual Wire Pair policy** (entre interfaces emparelhadas).
    - **Proxy policy** (HTTP/HTTPS com inspeção profunda).
    - **Multicast policy**.
    - **Local‑in policy** (tráfego destinado ao próprio FortiGate: admin, VPN, BGP, etc.).
    - **DoS policy** (proteção contra ataques de negação de serviço).
    - **Traffic shaping policy** (QoS / controle de banda).

### 2. Componentes de uma policy

- Campos obrigatórios:
    - **Incoming interface** e **Outgoing interface**.
    - **Source** (IP/subnet, range, FQDN, GeoIP, endereço dinâmico, MAC range, usuário/grupo, ISDB).
    - **Destination** (IP/subnet, FQDN, GeoIP, ISDB – mas **não** usuário).
    - **Service** (TCP/UDP/SCTP/ICMP, portas origem/destino, tipo/código).
    - **Schedule** (Always ou janelas de tempo).
    - **Action** (ACCEPT ou DENY).
- Opções de UI:
    - Possível habilitar **“Allow unnamed policies”** e **“Multi‑interfaces policies”** em _Feature Visibility_.
    - Visões da lista de policies:
    - **Interface pair view** (agrupadas por par de interfaces).
    - **By sequence** (ordem real de processamento).

### 3. Security Profiles (UTM/NGFW)

- Só são usados em policies com **ACCEPT**.
- Principais:
    - **Antivirus**, **Web Filter**, **DNS Filter**, **Application Control**, **IPS**, **SSL Inspection**.
- Perfis adicionais (ativados em Feature Visibility):
    - **Video Filter, File Filter, Email Filter, VoIP, Web Application Firewall, Inline CASB, DLP**, etc.
- Dois **modos de inspeção**:
    - **Flow‑based**: inspeção em tempo real, melhor performance, menos recursos.
    - **Proxy‑based**: bufferiza tráfego, permite features avançadas (quota, safe‑search, overrides, WAF, DLP), consome mais recursos; em appliances pequenos pode precisar habilitar via CLI.

### 4. Logging

- Em ACCEPT:
    - **UTM** (default): loga só eventos ligados a perfis de segurança.
    - **ALL**: loga todas as sessões.
- Em DENY:
    - Só opção **“Log violation traffic”** (off por default).
- Opção **“Generate logs when session starts”**:
    - Cria log na abertura e no fechamento da sessão (dobro de logs; mais CPU/armazenamento).
- Logs podem ser enviados para: **disco local, memória, FortiCloud, FortiAnalyzer, syslog**.
- Visualização:
    - **Forward Traffic** (tráfego) e **Security Events** (antivírus, webfilter, etc.).
    - Atalho **“Show Matching Logs”** a partir da policy.

### 5. GeoIP, ISDB e Geo‑based ISDB

- **GeoIP**: objeto de endereço que representa um país/região; FortiGate consulta FortiGuard.
- **ISDB (Internet Service Database)**:
    - Objetos dinâmicos com ranges de IP/port/protocol de serviços (Google, Microsoft, Meta, etc.).
- **Geo‑based ISDB**:
    - Combina serviço + região (ex.: Teams BR/RS/Porto Alegre).
- Necessitam de **licença FortiGuard** atualizada; aparecem nas policies como origem/destino ou _Internet Service_.

### 6. NAT – visão geral

- **NAT**: tradução de IP; com portas → **PAT**.
- Tipos:
    - **SNAT (Source NAT)**: traduz IP/porta de origem. Configurado na **policy** (campo NAT).
    - **DNAT (Destination NAT)**: traduz IP/porta de destino. Configurado via **VIP** (Virtual IP).
    - **NAT64/NAT46/NAT66**: tradução envolvendo IPv6.

#### 6.1 SNAT com IP da interface x IP Pools

- Opções na policy:
    - **Use outgoing interface address**: usa IP da própria interface WAN (equivale a overload).
    - **IP Pool**: usa um ou mais IPs configurados (pool).
- Funciona via **port translation**; FortiGate mantém tabela de sessão ligando IP/porta interna ↔ externa.
- Possíveis problemas:
    - **Port exhaustion / session clash** quando muitas sessões para o mesmo destino.
    - Comandos de debug: `diagnose sys session stat` (campo `clash`), `diagnose sys session list`.

#### 6.2 Tipos de IP Pool

- **Overload** (default): vários hosts compartilhando mesmo IP, com portas diferentes.
- **One‑to‑one**: um IP do pool por host interno (drop se não houver IP livre).
- **Fixed Port Range**:
    - Divide faixas de portas de cada IP público entre hosts; usado em cenários tipo **CGNAT**.
- **Port Block Allocation (PBA)**:
    - Aloca blocos de portas por host, com rastreabilidade em logs (muito usado por ISPs/CGNAT).

#### 6.3 DNAT / VIP

- VIP padrão = **Static NAT** (one‑to‑one):
    - Entrada: IP externo → IP interno.
    - Saída: o mesmo IP externo do VIP é usado como **SNAT** daquele servidor (mesmo que policy esteja em “use interface address”).
- **Port Forwarding**:
    - Traduz porta externa → porta interna (443→443, 10443→443 etc.).
- Policies:
    - Na policy de entrada, o VIP é escolhido no campo **Destination**.

#### 6.4 Match VIP em policies de deny

- Por padrão (até 7.2.3), destinos tipo `all`/address **não casam** com VIPs.
- Para bloquear por país/IP específico um VIP, é preciso:
    - Habilitar `set match-vip enable` na policy DENY **ou**
    - Usar o próprio VIP no campo **Destination**.
- Desde ~7.2.4, policies DENY novas já vêm com **match‑vip enable** por padrão.

#### 6.5 Boas práticas de NAT

- Conferir ranges de IPPool para evitar **sobreposição** com IPs de interface ou outros dispositivos.
- Internos acessando servidor interno publicado:
    - Preferir DNS interno apontando para o IP **interno**, não o IP público.
- Evitar VIPs desnecessários; só expor o que realmente precisa.
- Fazer mudanças de NAT em **janela de manutenção** e sempre testar.

### 7. Policy List, melhores práticas e performance

- Ordem top‑down é crítica; sempre posicionar **regra mais específica acima da genérica**.
- Manter policies o mais **específicas** possível (origem/destino/serviço).
- Cuidado ao editar objetos usados em várias policies: afeta tudo de uma vez.
- Sessões ativas podem ser **reavaliadas** após mudança, gerando pico de CPU/memória (até conserve mode).
- Manipulação de ordem:
    - GUI: **drag and drop**.
    - CLI: `config firewall policy` → `move <id> before|after <id>`.

## Flow-based Inspection
No modo **flow-based**, a inspeção acontece diretamente no fluxo do tráfego.

### Características
- Foco em desempenho
- Menor latência
- Processamento mais leve que o modo proxy em muitos casos

### Quando usar
- Ambientes que priorizam performance
- Cenários com alto volume de tráfego
- Quando não se precisa de análise tão profunda

### Vantagens
- Mais rápido
- Menor impacto de processamento
- Boa eficiência operacional

---

## Proxy-based Inspection
No modo **proxy-based**, o FortiGate atua como intermediário entre cliente e servidor.

### Características
- O tráfego é terminado e reprocessado pelo firewall
- Permite inspeção mais profunda
- Pode aplicar controles mais detalhados

### Vantagens
- Maior profundidade de análise
- Mais recursos para políticas e segurança
- Melhor controle em determinados cenários web

### Desvantagens
- Maior uso de recursos
- Pode introduzir mais latência
- Requer atenção ao dimensionamento
