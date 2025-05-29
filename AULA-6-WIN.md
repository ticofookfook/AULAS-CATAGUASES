# MÓDULO 1: SISTEMA DE ARQUIVOS E NAVEGAÇÃO

## 1.1 Sistema de Arquivos Windows

### **Estrutura de Diretórios:**
```
C:\          (raiz do sistema)
├── Windows\ (arquivos do sistema)
│   ├── System32\ (executáveis e DLLs do sistema)
│   ├── SysWOW64\ (executáveis 32-bit em sistemas 64-bit)
│   ├── Temp\ (arquivos temporários do sistema)
│   └── Logs\ (logs do sistema)
├── Program Files\ (programas 64-bit)
├── Program Files (x86)\ (programas 32-bit)
├── ProgramData\ (dados de aplicativos - oculto)
├── Users\ (perfis de usuários)
│   ├── Public\ (pasta pública)
│   └── [NomeUsuario]\ (pasta do usuário)
│       ├── Desktop\ (área de trabalho)
│       ├── Documents\ (documentos)
│       ├── Downloads\ (downloads)
│       ├── Pictures\ (imagens)
│       ├── Music\ (música)
│       ├── Videos\ (vídeos)
│       └── AppData\ (dados de aplicativos - oculto)
└── Temp\ (arquivos temporários)
```

### **Tipos de Caminhos:**
- **Absoluto**: `C:\Users\Usuario\Documents\arquivo.txt`
- **Relativo**: `Documents\arquivo.txt` (relativo ao diretório atual)


### **Caracteres Especiais:**
- `.` (diretório atual)
- `..` (diretório pai)
- `%USERPROFILE%` (pasta do usuário)
- `%HOMEPATH%` (caminho home do usuário)
- `*` (wildcard - qualquer conjunto de caracteres)
- `?` (wildcard - um caractere qualquer)

### **Extensões de Arquivo Importantes:**
- `.exe` - Executáveis
- `.msi` - Instaladores Windows
- `.dll` - Bibliotecas dinâmicas
- `.bat/.cmd` - Scripts de lote
- `.ps1` - Scripts PowerShell
- `.reg` - Arquivos de registro

## 1.2 Prompt de Comando (CMD)

### **Navegação Básica:**
```cmd
# Mostrar diretório atual
cd

# Listar arquivos e pastas
dir
dir /a          # Inclui arquivos ocultos
dir /s          # Recursivo (subdiretórios)
dir /o:n        # Ordenado por nome
dir /o:d        # Ordenado por data
dir /o:s        # Ordenado por tamanho
dir *.txt       # Apenas arquivos .txt
dir /p          # Paginado

# Navegar entre diretórios
cd C:\Windows                    # Ir para diretório específico
cd ..                           # Subir um nível
cd \                            # Ir para raiz da unidade
cd /d D:\Projetos              # Mudar de unidade e diretório
pushd C:\Temp                  # Salva diretório atual e navega
popd                           # Volta ao diretório salvo

# Mostrar estrutura em árvore
tree                           # Árvore do diretório atual
tree /f                        # Inclui arquivos
tree /a                        # Usa caracteres ASCII
tree C:\Windows /f             # Árvore de diretório específico
```

### **Informações do Sistema:**
```cmd
# Informações básicas
ver                            # Versão do Windows
date                           # Data atual
time                           # Hora atual
hostname                       # Nome do computador
whoami                         # Usuário atual
echo %USERNAME%                # Nome do usuário
echo %COMPUTERNAME%           # Nome do computador

# Variáveis de ambiente
set                           # Todas as variáveis
echo %PATH%                   # Caminho de executáveis
echo %USERPROFILE%           # Pasta do usuário
echo %TEMP%                  # Pasta temporária
echo %WINDIR%                # Pasta do Windows
```

## 1.3 Manipulação de Arquivos e Pastas (CMD)

### **Criar e Remover Diretórios:**
```cmd
# Criar diretórios
md nome_pasta                  # Criar pasta
mkdir "Nome com Espaços"       # Com espaços (aspas obrigatórias)
md pasta1\pasta2\pasta3        # Criar estrutura aninhada

# Remover diretórios
rd nome_pasta                  # Remove pasta vazia
rmdir nome_pasta               # Mesmo que rd
rd /s pasta                    # Remove pasta e conteúdo
rd /q pasta                    # Remove sem confirmação
rd /s /q pasta                 # Remove tudo sem confirmação
```

### **Criar e Manipular Arquivos:**
```cmd
# Criar arquivos vazios
type nul > arquivo.txt         # Criar arquivo vazio
echo. > arquivo.txt            # Alternativa
copy nul arquivo.txt           # Outra alternativa

# Criar arquivo com conteúdo
echo "Olá Mundo" > arquivo.txt          # Sobrescreve
echo "Nova linha" >> arquivo.txt        # Adiciona ao final

# Copiar arquivos e pastas
copy origem.txt destino.txt             # Copia arquivo
copy *.txt backup\                      # Copia todos .txt
copy /y origem.txt destino.txt          # Sem confirmação
xcopy pasta1 pasta2 /e                  # Copia pasta recursivamente
xcopy pasta1 pasta2 /e /h /k            # Inclui ocultos e atributos
robocopy origem destino /e              # Ferramenta robusta de cópia
```

### **Mover e Renomear:**
```cmd
# Mover arquivos
move arquivo.txt nova_pasta\            # Move arquivo
move *.log logs\                        # Move todos .log

# Renomear
ren arquivo_antigo.txt arquivo_novo.txt # Renomear arquivo
rename pasta_antiga pasta_nova          # Renomear pasta
```

### **Excluir Arquivos:**
```cmd
del arquivo.txt                # Exclui arquivo
del *.tmp                      # Exclui todos .tmp
del /q arquivo.txt             # Sem confirmação
del /f arquivo.txt             # Força exclusão
del /s *.log                   # Recursivo em subpastas
erase arquivo.txt              # Mesmo que del
```

## 1.4 Visualização de Conteúdo

### **Exibir Conteúdo de Arquivos:**
```cmd
# Mostrar conteúdo completo
type arquivo.txt               # Exibe todo o conteúdo
type arquivo1.txt arquivo2.txt # Múltiplos arquivos

# Mostrar com paginação
more arquivo.txt               # Paginação simples
more < arquivo.txt             # Alternativa

# Primeiras/últimas linhas (PowerShell necessário)
powershell "Get-Content arquivo.txt | Select-Object -First 10"
powershell "Get-Content arquivo.txt | Select-Object -Last 10"
```

### **Buscar em Arquivos:**
```cmd
# Buscar texto em arquivos
find "texto" arquivo.txt       # Busca texto específico
find /i "texto" arquivo.txt    # Case insensitive
find /n "erro" *.log          # Com número da linha
findstr "padrão" arquivo.txt   # Busca com regex básica
findstr /i /n "erro" *.log    # Case insensitive com linha
```

### **Informações de Arquivos:**
```cmd
# Atributos e propriedades
attrib arquivo.txt             # Mostra atributos
attrib +h arquivo.txt          # Define como oculto
attrib -h arquivo.txt          # Remove oculto
attrib +r arquivo.txt          # Somente leitura
attrib -r arquivo.txt          # Remove somente leitura

# Comparar arquivos
fc arquivo1.txt arquivo2.txt   # Compara arquivos texto
comp arquivo1.bin arquivo2.bin # Compara arquivos binários
```

---

# MÓDULO 2: POWERSHELL E ADMINISTRAÇÃO

## 2.1 Introdução ao PowerShell

### **Conceitos Fundamentais:**
- PowerShell trabalha com **objetos**, não apenas texto
- **Cmdlets** seguem o padrão Verb-Noun (Get-Process, Set-Location)
- **Pipeline** passa objetos entre comandos
- **Aliases** são atalhos para cmdlets comuns

### **Navegação e Arquivos:**
```powershell
# Navegação
Get-Location                   # Diretório atual (pwd)
Set-Location C:\Windows        # Mudar diretório (cd)
Push-Location C:\Temp          # Salva e muda diretório
Pop-Location                   # Volta ao diretório salvo

# Listar arquivos
Get-ChildItem                  # Lista arquivos (ls, dir)
Get-ChildItem -Force           # Inclui ocultos
Get-ChildItem -Recurse         # Recursivo
Get-ChildItem *.txt            # Apenas .txt
Get-ChildItem | Where-Object {$_.Length -gt 1MB}  # Maiores que 1MB

# Aliases comuns
ls          # Get-ChildItem
dir         # Get-ChildItem
cd          # Set-Location
pwd         # Get-Location
cat         # Get-Content
```

### **Manipulação de Arquivos:**
```powershell
# Criar arquivos e pastas
New-Item -ItemType File -Name "arquivo.txt"
New-Item -ItemType Directory -Name "nova_pasta"
New-Item -Path "C:\Temp\arquivo.txt" -ItemType File -Force

# Copiar
Copy-Item arquivo.txt backup\
Copy-Item -Recurse pasta1 pasta2
Copy-Item *.log -Destination logs\

# Mover e renomear
Move-Item arquivo.txt nova_pasta\
Rename-Item arquivo_antigo.txt arquivo_novo.txt

# Excluir
Remove-Item arquivo.txt
Remove-Item -Recurse pasta
Remove-Item -Force arquivo_protegido.txt
```

### **Conteúdo de Arquivos:**
```powershell
# Ler conteúdo
Get-Content arquivo.txt        # Todo o conteúdo
Get-Content arquivo.txt | Select-Object -First 10    # Primeiras 10 linhas
Get-Content arquivo.txt | Select-Object -Last 5      # Últimas 5 linhas
Get-Content -Tail 10 arquivo.log                     # Segue arquivo (tail -f)

# Escrever conteúdo
"Hello World" | Out-File arquivo.txt                  # Sobrescreve
"Nova linha" | Add-Content arquivo.txt                # Adiciona
Set-Content arquivo.txt "Novo conteúdo"               # Substitui todo conteúdo
```

## 2.2 Gerenciamento de Processos

### **Visualizar Processos:**
```powershell
# Listar processos
Get-Process                    # Todos os processos
Get-Process -Name "notepad"    # Processo específico
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

# Informações detalhadas
Get-Process | Select-Object Name, Id, CPU, WorkingSet
Get-Process | Where-Object {$_.WorkingSet -gt 100MB}  # Mais de 100MB RAM

# Processos por usuário
Get-WmiObject Win32_Process | Select-Object Name, Owner, ProcessId
```

### **Gerenciar Processos:**
```powershell
# Iniciar processos
Start-Process notepad                    # Abre Notepad
Start-Process "C:\Programa\app.exe"      # Executa programa
Start-Process cmd -ArgumentList "/c dir" # Com argumentos

# Parar processos
Stop-Process -Name "notepad"             # Para por nome
Stop-Process -Id 1234                   # Para por PID
Stop-Process -Name "chrome" -Force       # Força parada
Get-Process chrome | Stop-Process        # Via pipeline
```

### **Serviços do Windows:**
```powershell
# Listar serviços
Get-Service                              # Todos os serviços
Get-Service -Name "Spooler"              # Serviço específico
Get-Service | Where-Object {$_.Status -eq "Running"}  # Apenas em execução

# Controlar serviços
Start-Service -Name "Spooler"            # Iniciar serviço
Stop-Service -Name "Spooler"             # Parar serviço
Restart-Service -Name "Spooler"          # Reiniciar serviço
Set-Service -Name "Spooler" -StartupType Automatic  # Inicialização automática
```

## 2.3 Informações do Sistema

### **Hardware e Sistema:**
```powershell
# Informações básicas
Get-ComputerInfo                         # Informações completas do sistema
Get-WmiObject Win32_OperatingSystem      # Sistema operacional
Get-WmiObject Win32_ComputerSystem       # Hardware básico

# CPU
Get-WmiObject Win32_Processor | Select-Object Name, NumberOfCores, MaxClockSpeed

# Memória
Get-WmiObject Win32_PhysicalMemory | Measure-Object Capacity -Sum
Get-Counter "\Memory\Available MBytes"   # Memória disponível

# Disco
Get-WmiObject Win32_LogicalDisk | Select-Object DeviceID, Size, FreeSpace
Get-Volume                               # Volumes e partições
```

### **Rede:**
```powershell
# Configuração de rede
Get-NetIPAddress                         # Endereços IP
Get-NetAdapter                           # Adaptadores de rede
Get-NetRoute                             # Tabela de roteamento
Get-DnsClientServerAddress               # Servidores DNS

# Conectividade
Test-Connection google.com               # Ping
Test-NetConnection google.com -Port 80   # Teste de porta
Resolve-DnsName google.com               # Resolução DNS
```

### **Usuários e Grupos:**
```powershell
# Usuários locais
Get-LocalUser                            # Usuários locais
Get-LocalGroup                           # Grupos locais
Get-LocalGroupMember "Administrators"    # Membros do grupo

# Usuário atual
[System.Security.Principal.WindowsIdentity]::GetCurrent().Name
$env:USERNAME                            # Nome do usuário
$env:USERDOMAIN                          # Domínio
```

## 2.4 Administração Avançada

### **Registro do Windows:**
```powershell
# Navegar no registro
Get-ChildItem HKLM:\Software             # Listar chaves
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion

# Modificar registro
New-Item -Path "HKCU:\Software\MinhaApp"                    # Criar chave
Set-ItemProperty -Path "HKCU:\Software\MinhaApp" -Name "Versao" -Value "1.0"
Remove-Item -Path "HKCU:\Software\MinhaApp" -Recurse        # Remover chave
```

### **Logs de Eventos:**
```powershell
# Visualizar logs
Get-EventLog -LogName System -Newest 10                     # Últimos 10 eventos do sistema
Get-EventLog -LogName Application -EntryType Error          # Apenas erros
Get-WinEvent -LogName Security -MaxEvents 50                # Log de segurança

# Filtrar eventos
Get-EventLog -LogName System | Where-Object {$_.TimeGenerated -gt (Get-Date).AddDays(-1)}
```

### **Agendamento de Tarefas:**
```powershell
# Listar tarefas agendadas
Get-ScheduledTask                        # Todas as tarefas
Get-ScheduledTask -TaskName "Backup*"    # Tarefas que começam com "Backup"

# Criar tarefa agendada
$Action = New-ScheduledTaskAction -Execute "notepad.exe"
$Trigger = New-ScheduledTaskTrigger -Daily -At "14:00"
Register-ScheduledTask -TaskName "AbrirNotepad" -Action $Action -Trigger $Trigger

# Controlar tarefas
Start-ScheduledTask -TaskName "Backup"   # Executar tarefa
Stop-ScheduledTask -TaskName "Backup"    # Parar tarefa
Unregister-ScheduledTask -TaskName "Backup" -Confirm:$false  # Remover tarefa
```

### **Firewall do Windows:**
```powershell
# Status do firewall
Get-NetFirewallProfile                   # Perfis do firewall
Get-NetFirewallRule | Where-Object {$_.Enabled -eq "True"}  # Regras ativas

# Criar regras
New-NetFirewallRule -DisplayName "Permitir HTTP" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow
New-NetFirewallRule -DisplayName "Bloquear Telnet" -Direction Outbound -Protocol TCP -RemotePort 23 -Action Block

# Gerenciar regras
Enable-NetFirewallRule -DisplayName "Permitir HTTP"
Disable-NetFirewallRule -DisplayName "Permitir HTTP"
Remove-NetFirewallRule -DisplayName "Permitir HTTP"
```

---
