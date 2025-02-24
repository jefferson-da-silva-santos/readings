# **Guia Completo de Comandos Úteis do CMD (Prompt de Comando) 🖥️**

Seja você um **desenvolvedor**, **administrador de sistemas** ou apenas alguém que quer dominar o **Prompt de Comando (CMD)** do Windows, este guia vai te ajudar a entender e utilizar os **comandos básicos e avançados** para resolver problemas do dia a dia e otimizar suas tarefas. Vamos cobrir **todas as categorias** de comandos, incluindo **redes**, **arquivos**, **gerenciamento de sistemas** e mais.

### **Comandos Básicos do CMD 🔑**

1. **Abrindo o CMD**:
   - Para abrir o **Prompt de Comando**, digite `cmd` no **Menu Iniciar** ou **tecla Windows + R** e digite `cmd`, depois aperte **Enter**.

2. **Verificando a versão do CMD**:
   - **Comando**: `ver`
   - Exibe a versão do sistema operacional Windows em uso.

---

### **Comandos para Navegação e Manipulação de Arquivos 📂**

1. **`dir`**: Listar arquivos e pastas no diretório atual
   - **Exemplo**: `dir`
   - Mostra todos os arquivos e pastas no diretório onde o prompt está localizado.
   
2. **`cd`**: Mudar o diretório
   - **Exemplo**: `cd pasta\subpasta`
   - Muda o diretório de trabalho. Use `cd ..` para voltar um diretório.

3. **`cls`**: Limpar a tela do CMD
   - **Exemplo**: `cls`
   - Limpa todo o conteúdo exibido no terminal.

4. **`md` ou `mkdir`**: Criar um diretório
   - **Exemplo**: `md novo-diretorio`
   - Cria um novo diretório.

5. **`rmdir`**: Remover um diretório
   - **Exemplo**: `rmdir pasta`
   - Remove um diretório vazio.

6. **`del`**: Deletar um arquivo
   - **Exemplo**: `del arquivo.txt`
   - Deleta o arquivo especificado.

7. **`ren`**: Renomear arquivos ou diretórios
   - **Exemplo**: `ren arquivo.txt novo-nome.txt`
   - Renomeia o arquivo ou diretório.

8. **`copy`**: Copiar arquivos
   - **Exemplo**: `copy arquivo1.txt arquivo2.txt`
   - Copia arquivos de uma pasta para outra.

9. **`move`**: Mover arquivos
   - **Exemplo**: `move arquivo.txt C:\novo-local\`
   - Mover um arquivo para uma nova pasta.

10. **`xcopy`**: Copiar arquivos e diretórios (com mais opções)
    - **Exemplo**: `xcopy C:\pasta1 C:\pasta2 /E`
    - Copia arquivos e pastas de maneira recursiva. A opção `/E` copia subpastas, incluindo as vazias.

11. **`robocopy`**: Copiar arquivos robustamente
    - **Exemplo**: `robocopy C:\pasta1 C:\pasta2 /MIR`
    - Ferramenta de cópia de arquivos mais avançada. A opção `/MIR` cria uma cópia exata de uma pasta (mira).

---

### **Comandos de Rede 🌐**

1. **`ipconfig`**: Exibir configurações de rede
   - **Exemplo**: `ipconfig`
   - Mostra informações de configuração da rede local (IP, Máscara de Sub-rede, Gateway).

2. **`ipconfig /all`**: Exibir todas as informações de rede detalhadas
   - **Exemplo**: `ipconfig /all`
   - Mostra detalhes completos da rede, incluindo adaptadores e endereços MAC.

3. **`ping`**: Testar conectividade de rede
   - **Exemplo**: `ping google.com`
   - Envia pacotes ICMP para verificar se um host está acessível.

4. **`tracert`**: Traçar a rota de um pacote até um destino
   - **Exemplo**: `tracert google.com`
   - Mostra a rota percorrida pelos pacotes até o destino e o tempo de resposta de cada salto.

5. **`netstat`**: Exibir estatísticas de rede
   - **Exemplo**: `netstat -ano`
   - Mostra informações sobre as conexões de rede e o status de portas. O parâmetro `-ano` exibe o PID associado às conexões.

6. **`nslookup`**: Verificar DNS
   - **Exemplo**: `nslookup google.com`
   - Verifica o endereço IP de um domínio.

7. **`netsh`**: Configurar rede (comando avançado)
   - **Exemplo**: `netsh interface ip set address "Ethernet" static 192.168.1.100 255.255.255.0 192.168.1.1`
   - Permite configurar a rede manualmente, como definir um IP estático.

8. **`route`**: Exibir ou modificar a tabela de rotas
   - **Exemplo**: `route print`
   - Mostra as rotas de rede atualmente configuradas no seu sistema.

9. **`telnet`**: Testar uma porta específica em um host
   - **Exemplo**: `telnet google.com 80`
   - Testa a conectividade à porta especificada em um servidor remoto (geralmente usado para testar servidores HTTP na porta 80).

---

### **Comandos para Gerenciamento de Sistema ⚙️**

1. **`tasklist`**: Exibir processos em execução
   - **Exemplo**: `tasklist`
   - Exibe todos os processos em execução no sistema.

2. **`taskkill`**: Finalizar um processo
   - **Exemplo**: `taskkill /PID 1234`
   - Finaliza o processo com o PID especificado.

3. **`shutdown`**: Desligar ou reiniciar o computador
   - **Exemplo**: `shutdown /s /t 60`
   - Desliga o computador em 60 segundos.
   - **Exemplo**: `shutdown /r /t 30`
   - Reinicia o computador após 30 segundos.

4. **`systeminfo`**: Obter informações sobre o sistema
   - **Exemplo**: `systeminfo`
   - Mostra detalhes do sistema, como versão, configuração de memória e processador.

5. **`sfc /scannow`**: Verificar e corrigir arquivos de sistema corrompidos
   - **Exemplo**: `sfc /scannow`
   - Escaneia e corrige arquivos de sistema danificados.

6. **`chkdsk`**: Verificar o disco rígido em busca de erros
   - **Exemplo**: `chkdsk C: /f`
   - Verifica o disco `C:` e tenta corrigir qualquer erro encontrado.

7. **`diskpart`**: Gerenciamento de partições de disco
   - **Exemplo**: `diskpart`
   - Inicia o utilitário para gerenciar discos, partições e volumes no Windows.

---

### **Comandos Avançados: Gerenciamento de Arquivos e Automação ⚡**

1. **`for`**: Laços de repetição para iterar sobre arquivos ou dados
   - **Exemplo**: `for %f in (*.txt) do echo %f`
   - Exibe todos os arquivos `.txt` no diretório atual.

2. **`find`**: Buscar por texto dentro de arquivos
   - **Exemplo**: `find "erro" log.txt`
   - Busca pela palavra "erro" no arquivo `log.txt`.

3. **`set`**: Definir ou exibir variáveis de ambiente
   - **Exemplo**: `set MY_VAR=123`
   - Define uma variável de ambiente chamada `MY_VAR`.

4. **`schtasks`**: Agendar tarefas no Windows
   - **Exemplo**: `schtasks /create /tn "Backup" /tr "C:\backup.bat" /sc daily /st 00:00`
   - Cria uma tarefa agendada chamada "Backup", que executa o script `backup.bat` todos os dias à meia-noite.

5. **`at`**: Agendar tarefas (antigo)
   - **Exemplo**: `at 12:00 /interactive shutdown /s`
   - Agendar uma tarefa para desligar o computador às 12:00.

---

### **Comandos de Segurança 🔒**

1. **`net user`**: Gerenciar contas de usuário
   - **Exemplo**: `net user nome_usuario /add`
   - Adiciona um novo usuário.

2. **`net localgroup`**: Gerenciar grupos de usuários
   - **Exemplo**: `net localgroup Administradores nome_usuario /add`
   - Adiciona um usuário ao grupo "Administradores".

3. **`cipher`**: Criptografar ou descriptografar arquivos
   - **Exemplo**: `cipher /e arquivo.txt`
   - Criptografa o arquivo `arquivo.txt`.

4. **`cacls`**: Modificar permissões de arquivos
   - **Exemplo**: `cacls arquivo.txt /E /P usuario:F`
   - Modifica as permissões de `arquivo.txt` para dar acesso total ao `usuario`.

---

### **Comandos Úteis para Diagnóstico e Resolução de Problemas 🔍**

1. **`eventvwr`**: Abrir o Visualizador de Eventos
   - **Exemplo**: `eventvwr`
   - Abre o visualizador de eventos do Windows para revisar logs de erros e eventos do sistema.

2. **`perfmon`**: Monitor de desempenho
   - **Exemplo**: `perfmon`
   - Abre o Monitor de Desempenho para visualizar métricas detalhadas sobre o sistema.

3. **`wmic`**: Windows Management Instrumentation Command-line
   - **Exemplo**: `wmic cpu get caption`
   - Exibe informações detalhadas sobre o processador do sistema.

---

### **Comandos para Monitoramento e Performance 🏃‍♂️**

1. **`tasklist`**: Listar processos em execução no sistema
   - **Exemplo**: `tasklist`
   - Mostra todos os processos em execução no sistema.

2. **`taskkill`**: Finalizar um processo
   - **Exemplo**: `taskkill /PID 1234`
   - Finaliza o processo com o PID especificado.

Claro! Vamos explorar alguns exemplos de **uso avançado** dos comandos do CMD em **diferentes cenários**, unindo-os para **automatizar tarefas**, **diagnóstico**, **manutenção de sistema** e até **gerenciamento de rede**. Esses exemplos vão ilustrar como você pode usar esses comandos de maneira mais complexa e poderosa.

### **1. Exemplo de Backup Automatizado e Agendado com Robocopy e Task Scheduler 🗂️**

Imagine que você quer fazer um backup **automático** de uma pasta importante para outro diretório ou até um **dispositivo externo**. Você pode usar **`robocopy`** (para cópias robustas) junto com o **`schtasks`** para agendar o backup em um horário específico todos os dias.

#### Passo a Passo:

1. **Comando para backup com `robocopy`**:
   - Usaremos `robocopy` para fazer uma cópia eficiente de arquivos, incluindo subpastas e arquivos ocultos, de uma pasta importante para um backup.
   
   **Comando**:
   ```bash
   robocopy "C:\Users\MeuUsuario\Documents" "D:\Backup" /MIR /E /Z /XA:H /R:3 /W:5
   ```

   - **`/MIR`**: Espelha a estrutura de diretórios (copiando e removendo arquivos que não estão mais na origem).
   - **`/E`**: Copia subpastas, incluindo vazias.
   - **`/Z`**: Ativa a cópia em modo reiniciável (útil para falhas de rede).
   - **`/XA:H`**: Exclui arquivos ocultos.
   - **`/R:3`**: Tenta novamente 3 vezes em caso de erro.
   - **`/W:5`**: Aguarda 5 segundos entre as tentativas.

2. **Agendar o backup diário usando `schtasks`**:
   
   Agora, vamos agendar o backup para ocorrer automaticamente todos os dias à meia-noite.

   **Comando**:
   ```bash
   schtasks /create /tn "Backup Diário" /tr "robocopy C:\Users\MeuUsuario\Documents D:\Backup /MIR /E /Z /XA:H /R:3 /W:5" /sc daily /st 00:00
   ```

   - **`/sc daily`**: Define o agendamento para **diariamente**.
   - **`/st 00:00`**: Define a hora de execução (meia-noite).
   - **`/tn "Backup Diário"`**: Define o nome da tarefa agendada.

**Resultado**: O backup será feito automaticamente todos os dias à meia-noite, usando o `robocopy` para copiar seus arquivos de forma robusta e eficiente.

---

### **2. Diagnóstico e Monitoramento de Rede com `ping`, `tracert`, `netstat` e `tasklist` 🌐**

Vamos criar um exemplo onde você precisa diagnosticar a conectividade de rede e verificar se há **problemas de latência** ou **falhas de rede**. Para isso, vamos usar **`ping`**, **`tracert`**, **`netstat`** e **`tasklist`** em conjunto.

#### Passo 1: Diagnóstico de Rede

1. **Verificar conectividade com o Google (ping)**:
   
   Vamos **pingar** o **Google** para verificar a latência e se a conexão está estável.

   ```bash
   ping google.com -t
   ```

   - **`-t`**: Faz o ping contínuo até você pressionar `Ctrl+C` para parar.

2. **Verificar a rota até o Google (tracert)**:
   
   Se você quer entender a **rota** que os pacotes estão percorrendo até o Google, use `tracert` para identificar onde podem estar ocorrendo **gargalos** na rede.

   ```bash
   tracert google.com
   ```

   - Esse comando vai mostrar os **"saltos"** pelos quais os pacotes de dados passam até alcançar o destino.

#### Passo 2: Verificar Conexões Ativas

1. **Verificar conexões de rede com `netstat`**:
   
   Se você está tendo problemas de rede e quer ver quais **portas** estão sendo usadas, use `netstat` para listar todas as conexões abertas e as portas em uso.

   ```bash
   netstat -ano
   ```

   - **`-a`**: Mostra todas as conexões e portas de escuta.
   - **`-n`**: Exibe endereços e portas numéricos (sem resolução DNS).
   - **`-o`**: Mostra o ID do processo (PID) de cada conexão.

2. **Verificar os processos que estão usando a rede com `tasklist`**:

   Agora, com o PID da conexão, você pode verificar qual processo está consumindo a rede.

   ```bash
   tasklist /fi "PID eq 1234"
   ```

   - Substitua `1234` pelo PID que você encontrou usando `netstat`.

#### Passo 3: Gerenciar Processos de Rede e Sistema

1. **Finalizar um processo que está consumindo rede**:

   Se você identificar um processo que está **consumindo muita largura de banda** ou causando problemas na rede, pode encerrá-lo com o comando `taskkill`.

   ```bash
   taskkill /PID 1234 /F
   ```

   - **`/F`**: Força a finalização do processo.

---

### **3. Limpeza e Otimização do Sistema com `chkdsk`, `sfc` e `cleanmgr` 🧹**

Aqui, vamos mostrar como manter o **sistema limpo** e funcionando corretamente, fazendo diagnósticos e **limpeza** de arquivos temporários, além de corrigir erros no disco rígido.

#### Passo 1: Verificar e Corrigir Erros no Disco

1. **Verificar o disco com `chkdsk`**:

   Se você suspeitar que o **sistema de arquivos** está corrompido, use o comando `chkdsk` para verificar e corrigir erros.

   ```bash
   chkdsk C: /f
   ```

   - **`/f`**: Corrige erros no sistema de arquivos.

2. **Verificar arquivos corrompidos do sistema com `sfc`**:

   O comando `sfc /scannow` verifica se há arquivos **do sistema** corrompidos e tenta repará-los automaticamente.

   ```bash
   sfc /scannow
   ```

#### Passo 2: Limpeza de Arquivos Temporários

1. **Limpar arquivos temporários com `cleanmgr`**:

   Você pode usar o **Cleaner** para excluir **arquivos temporários** e liberar espaço no disco.

   ```bash
   cleanmgr
   ```

   - Esse comando abre a interface gráfica para você selecionar quais arquivos deseja excluir (como cache, arquivos de sistema, etc.).

#### Passo 3: Defrag de Disco

1. **Desfragmentar o disco com `defrag`**:

   Se o seu sistema está ficando mais lento, uma desfragmentação do disco pode ajudar a melhorar a **performance de leitura e escrita**.

   ```bash
   defrag C: /O
   ```

   - **`/O`**: Otimiza o disco para melhorar o desempenho.

---

### **4. Monitoramento de Sistema com `tasklist`, `systeminfo` e `perfmon` 🏃‍♂️**

Agora vamos ver como monitorar o **sistema em tempo real**, visualizar o uso de **memória**, **CPU**, **processos** e mais.

#### Passo 1: Obter Informações do Sistema

1. **Exibir informações do sistema com `systeminfo`**:

   Para obter um relatório completo do seu sistema (hardware, SO, memória, etc.), use:

   ```bash
   systeminfo
   ```

   - Ele fornece informações detalhadas sobre o sistema, incluindo versão do Windows, tempo de atividade, informações de memória, etc.

#### Passo 2: Monitoramento de Processos

1. **Listar processos em execução com `tasklist`**:

   Se você quiser ver todos os **processos** em execução e o uso de recursos, utilize:

   ```bash
   tasklist
   ```

   - Exibe todos os processos com **PID** e **uso de memória**.

2. **Monitoramento detalhado de desempenho com `perfmon`**:

   Você pode usar **`perfmon`** para abrir a interface gráfica de **Monitor de Desempenho** e acompanhar em tempo real o uso da CPU, memória, disco e rede.

   ```bash
   perfmon
   ```

---

### **5. Gerenciamento de Rede Avançado com `netsh` e `route` 🛠️**

1. **Configurar IP estático com `netsh`**:

   Para configurar um **IP estático**, você pode usar o comando `netsh` para modificar a configuração da interface de rede.

   ```bash
   netsh interface ip set address "Ethernet" static 192.168.1.100 255.255.255.0 192.168.1.1
   ```

   - **`"Ethernet"`**: Nome da interface de rede.
   - **`192.168.1.100`**: O IP estático.
   - **`255.255.255.0`**: Máscara de sub-rede.
   - **`192.168.1.1`**: Gateway padrão.

2. **Adicionar rota estática com `route`**:

   Para adicionar uma **rota estática** e alterar as rotas de rede:

   ```bash
   route add 192.168.1.0 mask 255.255.255.0 192.168.1.1
