---
id: 202611071001
tipo: conceito
tema: FortiGate
tags:
  - fortinet
  - fortigate
  - backup
  - restore
  - configuracao
---

# Backup e restore de configuração

O backup preserva a configuração do FortiGate para recuperação após falhas, desastres ou alterações malsucedidas. Armazene os arquivos de forma segura e mantenha cópias atualizadas antes de mudanças importantes.

## Formatos

O material da aula apresenta dois formatos:

- **FortiOS**: formato padrão de configuração.
- **YAML**: formato alternativo; pode ser usado para backup e restauração, conforme os recursos da versão.

## Criar um backup

O backup pode ser feito pela GUI para um computador local ou para uma unidade USB. O material também cita opções de automação, como Automation Stitch, e backup no logout em alguns modelos.

Uma opção chamada **Password mask** substitui valores de senha por `FortinetPasswordMask`. Segundo a aula, essa opção é útil ao enviar um arquivo ao suporte, mas o arquivo mascarado **não deve ser usado como backup para restauração**.

### Criptografia do arquivo

Além da proteção aplicada a dados sensíveis, é possível criptografar o arquivo inteiro com uma senha.

- Guarde a senha em local seguro; ela será necessária para usar o arquivo.
- O material da aula alerta que a restauração de um backup criptografado pode exigir o mesmo modelo e firmware.
- Confirme os requisitos de compatibilidade na documentação da versão e do modelo antes de depender de um backup para recuperação.

## Restaurar um backup

A restauração pode ser iniciada pela GUI ou pela CLI. O processo pode reiniciar o FortiGate e interromper temporariamente o tráfego.

Antes de restaurar:

1. Confirme o modelo, a versão e a compatibilidade do arquivo.
2. Preserve uma cópia da configuração atual, se possível.
3. Planeje a interrupção e garanta acesso ao equipamento após a restauração.
4. Tenha a senha disponível, caso o backup esteja criptografado.

## Conteúdo do arquivo

- Um backup não criptografado pode ser legível em editor de texto.
- O cabeçalho pode incluir informações como modelo, versão e build.
- O material descreve o arquivo como contendo configurações não padrão.
- Um backup criptografado não é legível além das informações que possam permanecer no cabeçalho.

## VDOMs

Em ambientes com VDOMs, um administrador limitado a uma VDOM pode fazer backup ou restaurar apenas a configuração sob seu escopo de administração. O backup global depende das permissões administrativas correspondentes.

Consulte [[315_VDOM]] e [[316_Acesso_Administrativos]].

## Checklist de boas práticas

- Faça backup antes de upgrades e mudanças de configuração.
- Mantenha cópias fora do FortiGate e com acesso restrito.
- Identifique cada arquivo com equipamento, data, versão e finalidade.
- Proteja a senha do backup criptografado separadamente do arquivo.
- Teste periodicamente o processo de restauração em ambiente adequado.
- Não confunda backup com `Password mask`: o arquivo mascarado é destinado ao compartilhamento, não à recuperação.

## Relações

- [[315_VDOM]]
- [[316_Acesso_Administrativos]]
- [[318_Upgrade]]