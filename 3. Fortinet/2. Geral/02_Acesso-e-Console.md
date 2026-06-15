
# 🔒 Acesso e Console do FortiGate

> **Tags:** #fortinet #fortigate #acesso #console #cli #seguranca #admin

---

## 📌 Visão Geral

Configurar corretamente o acesso ao FortiGate é fundamental para a segurança e a capacidade de gerenciamento do dispositivo. Isso inclui acesso via console, SSH e HTTPS.

🎯 **Objetivo:** Garantir acesso seguro e eficiente ao FortiGate, limitando métodos e origens.

---

## 💻 Configurações de Console

### 1. `config system console`

**Descrição:** Define parâmetros para o acesso via porta serial do console.

bash

```
config system console
   set baudrate 9600 # Velocidade de comunicação (padrão 9600 ou 115200)
   set output more # Habilita o paginador (similar ao 'less' no Linux)
   set login enable # Exige login para acesso ao console 
   set idle-timeout 5 # Tempo de inatividade em minutos antes de desconectar 
   end
```

**Dica Importante:** Em ambientes de produção, sempre habilite `set login enable` para exigir autenticação no console.

---

## 🌐 Portas Administrativas e Acesso Remoto

### 1. `config system global`

**Descrição:** Define portas para acesso administrativo via HTTPS (GUI) e SSH (CLI).

<div class="widget code-container remove-before-copy"><div class="code-header non-draggable"><span class="iaf s13 w700 code-language-placeholder">bash</span><div class="code-copy-button"><span class="iaf s13 w500 code-copy-placeholder">Copiar</span><img class="code-copy-icon" src="data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A" /></div></div><pre id="code-vmh9ilss1" style="color:white;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;white-space:pre;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none;padding:8px;margin:8px;overflow:auto;background:#011627;width:calc(100% - 8px);border-radius:8px;box-shadow:0px 8px 18px 0px rgba(120, 120, 143, 0.10), 2px 2px 10px 0px rgba(255, 255, 255, 0.30) inset"><code class="language-bash" style="white-space:pre;color:#d6deeb;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none"><span>config system global
</span><span>    </span><span class="token" style="color:rgb(255, 203, 139)">set</span><span> admin-sport </span><span class="token" style="color:rgb(247, 140, 108)">443</span><span>         </span><span class="token" style="color:rgb(99, 119, 119);font-style:italic"># Porta HTTPS para acesso à GUI (padrão 443)</span><span>
</span><span>    </span><span class="token" style="color:rgb(255, 203, 139)">set</span><span> admin-ssh-port </span><span class="token" style="color:rgb(247, 140, 108)">22</span><span>       </span><span class="token" style="color:rgb(99, 119, 119);font-style:italic"># Porta SSH para acesso CLI (padrão 22)</span><span>
</span><span>    </span><span class="token" style="color:rgb(255, 203, 139)">set</span><span> admin-telnet-port </span><span class="token" style="color:rgb(247, 140, 108)">23</span><span>    </span><span class="token" style="color:rgb(99, 119, 119);font-style:italic"># Porta Telnet (desabilitar por segurança)</span><span>
</span><span>    </span><span class="token" style="color:rgb(255, 203, 139)">set</span><span> admin-https-pki-required disable </span><span class="token" style="color:rgb(99, 119, 119);font-style:italic"># Exigir certificado cliente para HTTPS (segurança extra)</span><span>
</span><span>    </span><span class="token" style="color:rgb(255, 203, 139)">set</span><span> admin-https-redirect </span><span class="token" style="color:rgb(255, 203, 139)">enable</span><span> </span><span class="token" style="color:rgb(99, 119, 119);font-style:italic"># Redirecionar HTTP para HTTPS</span><span>
</span><span>    </span><span class="token" style="color:rgb(255, 203, 139)">set</span><span> admin-port </span><span class="token" style="color:rgb(247, 140, 108)">80</span><span>           </span><span class="token" style="color:rgb(99, 119, 119);font-style:italic"># Porta HTTP (desabilitar ou redirecionar)</span><span>
</span>end
</code></pre></div>

**Dica Importante:**
- **Sempre desabilite Telnet (`set admin-telnet-port 0`)** em produção, pois é um protocolo inseguro.
- Considere mudar as portas padrão (443, 22) para outras portas não-padrão para reduzir ataques automatizados (security by obscurity), mas lembre-se de documentar.
- Para máxima segurança, use `set admin-https-pki-required enable` e configure certificados de cliente.

---

## 🔒 Trusted Hosts (Limitar Origens de Acesso)


---

## 2️⃣ 01_Visao-Geral-Status.md

**Pasta:** `📁 Fortinet/01_Geral/`

<div class="widget code-container remove-before-copy"><div class="code-header non-draggable"><span class="iaf s13 w700 code-language-placeholder">markdown</span><div class="code-copy-button"><span class="iaf s13 w500 code-copy-placeholder">Copiar</span><img class="code-copy-icon" src="data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A" /></div></div><pre id="code-8zuusuk54" style="color:white;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;white-space:pre;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none;padding:8px;margin:8px;overflow:auto;background:#011627;width:calc(100% - 8px);border-radius:8px;box-shadow:0px 8px 18px 0px rgba(120, 120, 143, 0.10), 2px 2px 10px 0px rgba(255, 255, 255, 0.30) inset"><code class="language-markdown" style="white-space:pre;color:#d6deeb;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none"><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">#</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 📊 Visão Geral e Status do Sistema (FortiGate)</span><span>
</span>
<span></span><span class="token blockquote" style="color:rgb(199, 146, 234)">&gt;</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Tags:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> #fortinet #fortigate #status #performance #cli #diagnostico #conserve-mode
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 📌 Visão Geral</span><span>
</span>
<!-- -->Monitorar o status geral e a performance do FortiGate é crucial para garantir a estabilidade, identificar problemas proativamente e otimizar o uso de recursos.
<!-- -->
<span>🎯 </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Objetivo:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Entender os comandos essenciais para verificar a saúde do FortiGate e interpretar suas saídas.
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 💻 Comandos Essenciais</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">###</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 1. `get system status`</span><span>
</span>
<span></span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Descrição:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Exibe informações gerais do sistema, incluindo modelo, versão do firmware, número de série, uptime, status de licenças e modo de operação.
</span>
<span></span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Exemplo de Saída:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span>
</span></code></pre></div>
Version: FortiGate-VM64 v7.2.5,build1358,230807 (GA)
Build: 1358
Branch point: 1358
Release Version Information: GA
FortiOS Version: v7.2.5
Firmware: FortiGate-VM64 v7.2.5,build1358,230807 (GA)
Serial-Number: FGVM02TM2300XXXX
BIOS version: 04000000
...
System time: Fri Jan  2 23:41:00 2026
```

**Pontos Chave:**

- **Version/Firmware:** Confirma a versão do FortiOS. Essencial antes de qualquer troubleshooting ou upgrade.
- **Serial-Number:** Identificador único para registro e suporte.
- **System time:** Verifica a sincronização de tempo (importante para logs e autenticação).

---

### 2. `get system performance status`

**Descrição:** Fornece um resumo rápido da performance do FortiGate, incluindo uso de CPU, memória, sessões ativas, latência e estatísticas de segurança.

**Exemplo de Saída:**

```
CPU states: 0% user 0% system 0% nice 99% idle 0% iowait 0% irq 0% softirq
CPU0 states: 0% user 0% system 0% nice 99% idle 0% iowait 0% irq 0% softirq
CPU1 states: 0% user 0% system 0% nice 99% idle 0% iowait 0% irq 0% softirq
...
Memory states: 20% used
Average network usage: 0 kbps in 0 kbps out
Average sessions: 10
Average session setup rate: 0 sessions/sec
Average CPU: 0%
Average memory: 20%
...
Number of virus detected: 0
Number of IPS blocked: 0
```

**Pontos Chave:**

- **CPU states:** Indica a porcentagem de tempo que a CPU gasta em diferentes estados. `idle` alto é bom. `user` e `system` altos indicam carga.
- **Memory states:** Uso total da memória.
- **Average sessions:** Número de sessões ativas. Um aumento súbito pode indicar um ataque ou problema de aplicação.
- **Average session setup rate:** Taxa de novas sessões.
- **Virus/IPS:** Estatísticas de detecção de segurança.

---

### 3. `diagnose hardware sysinfo memory`

**Descrição:** Detalha o uso da memória RAM do FortiGate.

**Exemplo de Saída:**

```
total: 2048 MB
used: 400 MB
free: 1648 MB
buffers: 10 MB
cached: 200 MB
```

**Pontos Chave:**

- **total, used, free:** Informações básicas.
- **buffers, cached:** Memória usada para otimizar I/O de disco e acesso a arquivos. Não é memória "perdida".

---

### 4. `diagnose sys top`

**Descrição:** Exibe o uso de CPU e memória por processo em tempo real (similar ao `top` do Linux).

**Comandos no `diagnose sys top`:**

- `q`: Sair.
- `m`: Ordenar por uso de memória.
- `p`: Ordenar por uso de CPU.
- `k <PID>`: Matar um processo (use com extrema cautela!).

**Exemplo de Saída:**

```
Run Time:  0 days, 00:00:05
0U, 0S, 99I; 1T, 1F, 0M, 0P, 0N
   PID  PPID  STATE %CPU %MEM  VMID  VDOM  NAME
   123     1  S     0.0  0.1     0  root  newcli
   456     1  S     0.0  0.2     0  root  miglogd
   789     1  S     0.0  0.3     0  root  httpsd
```

**Pontos Chave:**

- **%CPU, %MEM:** Indica os processos que mais consomem recursos.
- **NAME:** Nome do processo. `newcli` (CLI), `httpsd` (GUI), `miglogd` (logging).
- **VDOM:** VDOM ao qual o processo pertence.

**Dica:** Se um processo estiver consumindo muita CPU/memória, pode ser um bug, um ataque ou uma configuração mal otimizada.

---

### 5. `diagnose hardware sysinfo conserve`

**Descrição:** Verifica o status do "Conserve Mode".

**Conserve Mode:** Um estado de emergência onde o FortiGate tenta preservar a memória RAM para funções críticas, descartando pacotes e desabilitando algumas funcionalidades. É um sinal de que o FortiGate está sob extrema pressão de memória.

**Exemplo de Saída:**

```
memory usage: 20%
red-line: 80%
green-line: 70%
...
Mode: normal
```

**Pontos Chave:**

- **memory usage:** Uso atual da memória.
- **red-line:** Limite superior para entrar em conserve mode (padrão 80%).
- **green-line:** Limite inferior para sair do conserve mode (padrão 70%).
- **Mode:** `normal`, `red-mode` ou `green-mode`.

**Dica Importante:**

- **Monitore o Conserve Mode!** Se o FortiGate entrar em `red-mode`, a performance será severamente afetada.
- **Configure thresholds para alertas:**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
  config system global
      set memory-use-threshold-red 80
      set memory-use-threshold-green 70
      set memory-use-threshold-extreme 90 # Novo threshold para alerta crítico
  end
```

- **Evite sobrecarga em produção:** Se o conserve mode for frequente, considere um upgrade de hardware ou otimização de configurações (ex: reduzir sessões, desabilitar features não usadas).
- **Reiniciar processos:** Em casos extremos, se um processo específico estiver causando o conserve mode, você pode tentar matá-lo com `diagnose sys kill 9 <PID>`, mas isso pode causar instabilidade. Um reboot pode ser necessário.

---

## 🔗 Links Relacionados

- [[02_Console-Acesso]]
- [[07_Diagnosticos-Gerais]]
- [[04_Logs-Monitoramento]]
- [[02_Boas-Praticas]]

---

## 📚 Referências

- FortiGate CLI Reference (docs.fortinet.com)
- FortiGate Troubleshooting Guide (docs.fortinet.com)
- Fortinet Community (community.fortinet.com)

```
---

## 3️⃣ 02_Console-Acesso.md

**Pasta:** `📁 Fortinet/01_Geral/`

<div class="widget code-container remove-before-copy"><div class="code-header non-draggable"><span class="iaf s13 w700 code-language-placeholder">markdown</span><div class="code-copy-button"><span class="iaf s13 w500 code-copy-placeholder">Copiar</span><img class="code-copy-icon" src="data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A" /></div></div><pre id="code-cyr5jv0aq" style="color:white;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;white-space:pre;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none;padding:8px;margin:8px;overflow:auto;background:#011627;width:calc(100% - 8px);border-radius:8px;box-shadow:0px 8px 18px 0px rgba(120, 120, 143, 0.10), 2px 2px 10px 0px rgba(255, 255, 255, 0.30) inset"><code class="language-markdown" style="white-space:pre;color:#d6deeb;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none"><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">#</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 💻 Console e Acesso (FortiGate)</span><span>
</span>
<span></span><span class="token blockquote" style="color:rgb(199, 146, 234)">&gt;</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Tags:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> #fortinet #fortigate #acesso #cli #seguranca #ssh #console #admin
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 📌 Visão Geral</span><span>
</span>
<!-- -->A configuração correta do acesso ao FortiGate é fundamental para a segurança e para o gerenciamento eficiente do dispositivo. Isso inclui o acesso via console, SSH e HTTPS.
<!-- -->
<span>🎯 </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Objetivo:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Configurar e proteger os métodos de acesso administrativo ao FortiGate.
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 💻 Configuração do Console Serial</span><span>
</span>
<!-- -->O console serial é o método de acesso de último recurso, útil para configurações iniciais, recuperação de senha ou quando a rede está inacessível.
<!-- -->
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">###</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> Comandos</span><span>
</span>
</code></pre></div>forticli
config system console
    set baudrate 9600           # Velocidade de comunicação (padrão 9600 ou 115200)
    set output more             # Paginador para exibir a saída de comandos em blocos
    set login enable            # Exige login no console (recomendado)
    set idle-timeout 5          # Tempo de inatividade em minutos antes de desconectar
end
```

**Dica:**

- **`set output more`**: Essencial para comandos com muita saída. Pressione `Space` para avançar uma página, `q` para sair.
- **`set login enable`**: **Sempre ative!** Garante que mesmo via console, seja necessário autenticação.

---

## 🌐 Acesso Administrativo via Rede (HTTPS/SSH)

O acesso via rede é o método mais comum para gerenciamento diário.

### 1. Portas Administrativas

Por padrão, o FortiGate usa a porta 443 para HTTPS e 22 para SSH. É uma boa prática alterar essas portas para dificultar ataques automatizados (security by obscurity).

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
config system global
    set admin-sport 4433        # Altera a porta HTTPS para 4433 (exemplo)
    set admin-ssh-port 2222     # Altera a porta SSH para 2222 (exemplo)
    set admin-telnet-port 23    # Porta Telnet (desabilitar!)
    set admin-http-port 80      # Porta HTTP (desabilitar!)
end
```

**Dica Importante:**

- **Desabilite HTTP e Telnet!** Por serem protocolos não criptografados, eles expõem credenciais. Use apenas HTTPS e SSH.
- **Altere as portas padrão:** Embora não seja uma solução de segurança robusta, ajuda a reduzir o ruído de scanners automatizados.

---

### 2. Trusted Hosts (Hosts Confiáveis)

Restringir o acesso administrativo a IPs ou redes específicas é uma medida de segurança crítica.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
config system admin
    edit "admin" # Ou o nome do seu usuário administrativo
        set trusthost1 192.168.1.0 255.255.255.0 # Permite acesso da rede 192.168.1.0/24
        set trusthost2 10.0.0.1 255.255.255.255 # Permite acesso do IP 10.0.0.1
        # set trusthost3 0.0.0.0 0.0.0.0 # NÃO use isso em produção, permite qualquer IP
    next
end
```

**Dica:**

- **Sempre configure Trusted Hosts!** Limite o acesso apenas aos IPs ou redes de onde você gerencia o FortiGate.
- Se você precisar de acesso de qualquer lugar (ex: para FortiManager), use `0.0.0.0 0.0.0.0` **apenas** se for absolutamente necessário e com outras camadas de segurança (ex: VPN, 2FA).

---

### 3. Verificando Acessos Ativos

Para saber quem está logado no FortiGate.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
get system info admin status
```

**Exemplo de Saída:**

```
Login time: 2026-01-02 23:41:00
From: 192.168.1.10 (ssh)
User: admin
VDOM: root
```

**Pontos Chave:**

- **Login time:** Quando o usuário logou.
- **From:** IP de origem e método de acesso (ssh, https, console).
- **User:** Nome do usuário.
- **VDOM:** VDOM em que o usuário está logado.

---

## 🔗 Links Relacionados

- [[01_Visao-Geral-Status]]
- [[02_Usuarios-Autenticacao]]
- [[02_Boas-Praticas]]
- [[03_Links-Uteis-Fortinet]]

---

## 📚 Referências

- FortiGate Administration Guide (docs.fortinet.com)
- FortiGate Security Best Practices (docs.fortinet.com)

```
---

## 4️⃣ 03_NTP-DNS.md

**Pasta:** `📁 Fortinet/01_Geral/`

<div class="widget code-container remove-before-copy"><div class="code-header non-draggable"><span class="iaf s13 w700 code-language-placeholder">markdown</span><div class="code-copy-button"><span class="iaf s13 w500 code-copy-placeholder">Copiar</span><img class="code-copy-icon" src="data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A" /></div></div><pre id="code-9b3ba42ug" style="color:white;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;white-space:pre;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none;padding:8px;margin:8px;overflow:auto;background:#011627;width:calc(100% - 8px);border-radius:8px;box-shadow:0px 8px 18px 0px rgba(120, 120, 143, 0.10), 2px 2px 10px 0px rgba(255, 255, 255, 0.30) inset"><code class="language-markdown" style="white-space:pre;color:#d6deeb;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none"><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">#</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> ⏰ NTP e DNS (FortiGate)</span><span>
</span>
<span></span><span class="token blockquote" style="color:rgb(199, 146, 234)">&gt;</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Tags:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> #fortinet #fortigate #ntp #dns #sincronizacao #resolucao #cli
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 📌 Visão Geral</span><span>
</span>
<!-- -->A sincronização de tempo (NTP) e a resolução de nomes (DNS) são serviços fundamentais para o correto funcionamento de qualquer dispositivo de rede, incluindo o FortiGate. Eles afetam desde a precisão dos logs até a autenticação e a navegação web.
<!-- -->
<span>🎯 </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Objetivo:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Configurar NTP e DNS no FortiGate e entender sua importância.
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> ⏰ NTP (Network Time Protocol)</span><span>
</span>
<!-- -->A sincronização de tempo é crítica para:
<span></span><span class="token list" style="color:rgb(199, 146, 234)">-</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Logs:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Garantir que os eventos de segurança e tráfego tenham timestamps precisos para troubleshooting e auditoria.
</span><span></span><span class="token list" style="color:rgb(199, 146, 234)">-</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Autenticação:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> FortiToken (2FA) e outros métodos de autenticação baseados em tempo dependem de uma sincronização precisa.
</span><span></span><span class="token list" style="color:rgb(199, 146, 234)">-</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Certificados SSL:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Validação de certificados depende do tempo correto.
</span><span></span><span class="token list" style="color:rgb(199, 146, 234)">-</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">VPN:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Alguns tipos de VPN podem ter problemas com desvio de tempo.
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">###</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> Comandos</span><span>
</span>
</code></pre></div>forticli
config system ntp
    set ntpsync enable          # Habilita a sincronização NTP
    set ntpserver-mode unicast  # Modo de operação (unicast é o mais comum)
    set ntpserver "pool.ntp.org" # Servidor NTP primário (pode ser um IP ou FQDN)
    set ntpserver2 "ntp.br"     # Servidor NTP secundário
    set syncinterval 60         # Intervalo de sincronização em segundos (padrão 60)
    set type client             # FortiGate como cliente NTP
end
```

**Dica Importante:**

- **Use servidores NTP confiáveis:** `pool.ntp.org` é um bom ponto de partida. Para ambientes corporativos, use servidores NTP internos ou do seu provedor.
- **Sincronize NTP para autenticação:** Como você usa FortiToken, a sincronização precisa é **mandatória** para que o 2FA funcione corretamente.
- **Verificar status NTP:**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
  diagnose sys ntp status
```

**Exemplo de Saída:**

```
  NTP server: pool.ntp.org
  NTP server stratum: 2
  NTP server reachability: 377
  NTP server offset: 0.000000 sec
  NTP server jitter: 0.000000 sec
  NTP server poll interval: 64 sec
  NTP server sync status: Synchronized
```

`Synchronized` é o que você quer ver.

---

## 🌐 DNS (Domain Name System)

A resolução de nomes é essencial para:

- **Navegação Web:** Acessar sites por FQDNs.
- **FortiGuard:** Conectar-se aos servidores FortiGuard para atualizações de assinaturas (AV, IPS, WebFilter, etc.).
- **FortiManager/FortiAnalyzer:** Conectar-se a esses dispositivos por FQDN.
- **Serviços Externos:** Resolução de FQDNs para servidores NTP, VPN peers, etc.

### Comandos

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
config system dns
    set primary 8.8.8.8         # Servidor DNS primário (ex: Google DNS)
    set secondary 8.8.4.4       # Servidor DNS secundário
    set dns-cache-limit 50000   # Limite de entradas no cache DNS
    set dns-cache-ttl 1800      # Tempo de vida (TTL) para entradas no cache DNS (em segundos)
    set ip-fragment-protection enable # Proteção contra fragmentação de IP em DNS (segurança)
    set server-host-name "FortiGate" # Nome do host para consultas DNS (opcional)
end
```

**Dica Importante:**

- **Use servidores DNS confiáveis e de baixa latência:** Servidores públicos (Google, Cloudflare) são bons, mas em ambientes corporativos, use servidores DNS internos para resolver nomes locais.
- **Teste a resolução DNS:**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
  execute ping google.com       # Testa a conectividade e resolução para um FQDN externo
  execute ping fortiguard.com   # Testa a conectividade com os servidores FortiGuard
```

- **Limpar cache DNS:** Se houver problemas de resolução após mudanças, limpe o cache.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
  execute clear system dns cache
```

- **Verificar cache DNS:**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
  diagnose test application dnsproxy 5
```

**Exemplo de Saída:**

```
  DNS cache size: 100
  Total entries: 10
  ...
  google.com (A) 172.217.160.142 ttl=1800
```

---

## 🔗 Links Relacionados

- [[01_Visao-Geral-Status]]
- [[02_Console-Acesso]]
- [[02_Usuarios-Autenticacao]]
- [[07_Diagnosticos-Gerais]]

---

## 📚 Referências

- FortiGate CLI Reference (docs.fortinet.com)
- FortiGate Best Practices Guide (docs.fortinet.com)

```
---

## 5️⃣ 04_Aliases-Comandos-Uteis.md

**Pasta:** `📁 Fortinet/01_Geral/`

<div class="widget code-container remove-before-copy"><div class="code-header non-draggable"><span class="iaf s13 w700 code-language-placeholder">markdown</span><div class="code-copy-button"><span class="iaf s13 w500 code-copy-placeholder">Copiar</span><img class="code-copy-icon" src="data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A" /></div></div><pre id="code-4kf2o3m2n" style="color:white;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;white-space:pre;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none;padding:8px;margin:8px;overflow:auto;background:#011627;width:calc(100% - 8px);border-radius:8px;box-shadow:0px 8px 18px 0px rgba(120, 120, 143, 0.10), 2px 2px 10px 0px rgba(255, 255, 255, 0.30) inset"><code class="language-markdown" style="white-space:pre;color:#d6deeb;font-family:Consolas, Monaco, &quot;Andale Mono&quot;, &quot;Ubuntu Mono&quot;, monospace;text-align:left;word-spacing:normal;word-break:normal;word-wrap:normal;line-height:1.5;font-size:1em;-moz-tab-size:4;-o-tab-size:4;tab-size:4;-webkit-hyphens:none;-moz-hyphens:none;-ms-hyphens:none;hyphens:none"><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">#</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 📝 Aliases e Comandos Úteis (FortiGate)</span><span>
</span>
<span></span><span class="token blockquote" style="color:rgb(199, 146, 234)">&gt;</span><span> </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Tags:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> #fortinet #fortigate #cli #aliases #produtividade #comandos-uteis
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 📌 Visão Geral</span><span>
</span>
<!-- -->A CLI do FortiGate é poderosa, mas alguns comandos podem ser longos ou repetitivos. Criar aliases e conhecer atalhos pode aumentar significativamente sua produtividade e agilizar o troubleshooting.
<!-- -->
<span>🎯 </span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Objetivo:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Personalizar a CLI com aliases e listar comandos úteis para o dia a dia.
</span>
<span></span><span class="token hr" style="color:rgb(199, 146, 234)">---</span><span>
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">##</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> 💻 Criando Aliases</span><span>
</span>
<span></span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span class="token content" style="font-weight:bold">Descrição:</span><span class="token" style="font-weight:bold;color:rgb(199, 146, 234)">**</span><span> Aliases são atalhos para comandos mais longos ou sequências de comandos.
</span>
<span></span><span class="token title" style="color:rgb(199, 146, 234);font-weight:bold">###</span><span class="token title" style="color:rgb(214, 222, 235);font-weight:bold"> Comandos</span><span>
</span>
</code></pre></div>forticli
config system alias
    edit "pingall"              # Nome do alias
        set command "execute ping-options source all" # Comando completo
    next
    edit "showint"
        set command "get system interface"
    next
    edit "showperf"
        set command "get system performance status"
    next
    edit "cleardns"
        set command "execute clear system dns cache"
    next
    edit "debugflow"
        set command "diagnose debug flow show console enable && diagnose debug flow show function enable && diagnose debug flow show ip enable && diagnose debug flow show port enable && diagnose debug flow trace start 100 && diagnose debug enable"
    next
    edit "stopdebug"
        set command "diagnose debug flow trace stop && diagnose debug disable"
    next
end
```

**Dica Importante:**

- **Use aliases para comandos frequentes:** Pense nos comandos que você digita várias vezes ao dia e crie aliases para eles.
- **Aliases para troubleshooting:** Crie aliases para iniciar e parar sessões de debug complexas, como o `debug flow`.
- **Compartilhe aliases:** Se você trabalha em equipe, padronizar aliases pode melhorar a colaboração.

---

## 🚀 Comandos Úteis Adicionais

Além dos aliases, alguns comandos são essenciais para o dia a dia:

### 1. `execute ping-options`

**Descrição:** Permite configurar opções avançadas para o comando `execute ping`.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute ping-options ?
# Opções:
#   adaptive: Ping adaptativo.
#   data-size: Tamanho do pacote de dados.
#   interval: Intervalo entre pings.
#   repeat: Número de pings.
#   source: Interface de origem ou IP.
#   timeout: Timeout por ping.
#   tos: Tipo de serviço.
#   ttl: Tempo de vida.
```

**Exemplo:**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute ping-options source port1 # Define a interface de origem para o próximo ping
execute ping 8.8.8.8              # O ping será enviado pela port1
execute ping-options source clear # Limpa a opção de origem
```

---

### 2. `execute traceroute`

**Descrição:** Rastreia a rota que os pacotes IP levam para alcançar um destino.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute traceroute google.com
```

---

### 3. `execute date`

**Descrição:** Exibe ou configura a data e hora do sistema.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute date
execute date 2026-01-02 23:41:00 # Definir data e hora manualmente (use NTP!)
```

---

### 4. `execute factoryreset`

**Descrição:** Restaura o FortiGate para as configurações de fábrica. **USE COM EXTREMA CAUTELA!**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute factoryreset
```

---

### 5. `execute backup config`

**Descrição:** Realiza backup da configuração do FortiGate. **ESSENCIAL!**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute backup config tftp <IP_TFTP_SERVER> <nome_arquivo.conf>
execute backup config ftp <IP_FTP_SERVER> <user> <pass> <nome_arquivo.conf>
execute backup config usb <nome_arquivo.conf> # Se tiver USB conectado
```

---

### 6. `execute restore config`

**Descrição:** Restaura a configuração do FortiGate a partir de um backup. **USE COM EXTREMA CAUTELA!**

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute restore config tftp <IP_TFTP_SERVER> <nome_arquivo.conf>
```

---

### 7. `execute reboot`

**Descrição:** Reinicia o FortiGate.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute reboot
```

---

### 8. `execute shutdown`

**Descrição:** Desliga o FortiGate.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
execute shutdown
```

---

### 9. `get router info routing-table all`

**Descrição:** Exibe a tabela de roteamento completa (FIB/RIB).

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
get router info routing-table all
```

---

### 10. `diagnose sys session list`

**Descrição:** Lista todas as sessões ativas no FortiGate.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
diagnose sys session list
diagnose sys session list | grep "src=192.168.1.10" # Filtrar por IP de origem
```

---

### 11. `diagnose firewall iprope list`

**Descrição:** Exibe a tabela de roteamento interna do FortiGate, mostrando como o tráfego é encaminhado.

forticli

Copiar![](data:image/svg+xml;utf8,%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%3E%0A%20%20%3Cpath%20d%3D%22M10.8%208.63V11.57C10.8%2014.02%209.82%2015%207.37%2015H4.43C1.98%2015%201%2014.02%201%2011.57V8.63C1%206.18%201.98%205.2%204.43%205.2H7.37C9.82%205.2%2010.8%206.18%2010.8%208.63Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%20%20%3Cpath%20d%3D%22M15%204.42999V7.36999C15%209.81999%2014.02%2010.8%2011.57%2010.8H10.8V8.62999C10.8%206.17999%209.81995%205.19999%207.36995%205.19999H5.19995V4.42999C5.19995%201.97999%206.17995%200.999992%208.62995%200.999992H11.57C14.02%200.999992%2015%201.97999%2015%204.42999Z%22%20stroke%3D%22%23717C92%22%20stroke-width%3D%221.05%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%0A%3C%2Fsvg%3E%0A)

```forticli
diagnose firewall iprope list
```

---

## 🔗 Links Relacionados

- [[01_Visao-Geral-Status]]
- [[02_Console-Acesso]]
- [[07_Diagnosticos-Gerais]]
- [[02_Boas-Praticas]]

---

## 📚 Referências

- FortiGate CLI Reference (docs.fortinet.com)
- FortiGate Troubleshooting Guide (docs.fortinet.com) ```