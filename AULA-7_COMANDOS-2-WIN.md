# CURSO COMPLETO DE WINDOWS

---

# MÓDULO 3: FERRAMENTAS ADMINISTRATIVAS

## 3.1 Utilitários de Sistema

### **Gerenciamento de Discos:**
```cmd
# DISKPART - Gerenciamento avançado de discos
diskpart
  list disk                              # Lista discos
  select disk 0                          # Seleciona disco
  list partition                         # Lista partições
  list volume                            # Lista volumes
  clean                                  # Limpa disco (CUIDADO!)
  create partition primary               # Cria partição primária
  active                                 # Marca como ativa
  format fs=ntfs quick                   # Formata rapidamente
  assign letter=D                        # Atribui letra da unidade
  exit                                   # Sai do diskpart

# CHKDSK - Verificação de disco
chkdsk C:                               # Verifica disco C:
chkdsk C: /f                            # Corrige erros
chkdsk C: /r                            # Localiza setores defeituosos
chkdsk C: /x                            # Força desmontagem se necessário
```

### **Configuração de Sistema:**
```cmd
# MSCONFIG - Configuração do sistema
msconfig                                # Interface gráfica de configuração

# SCONFIG - Server Core Configuration (Windows Server)
sconfig                                 # Menu de configuração do servidor

# Informações do sistema
systeminfo                              # Informações detalhadas do sistema
systeminfo | findstr "Domain"          # Informações de domínio
msinfo32                                # System Information (GUI)
```

### **Rede e Conectividade:**
```cmd
# Configuração de IP
ipconfig                                # Configuração básica
ipconfig /all                          # Configuração detalhada
ipconfig /release                       # Libera IP DHCP
ipconfig /renew                         # Renova IP DHCP
ipconfig /flushdns                      # Limpa cache DNS
ipconfig /displaydns                    # Mostra cache DNS

# Diagnóstico de rede
ping 8.8.8.8                           # Teste de conectividade
ping -t google.com                      # Ping contínuo
tracert google.com                      # Rastreamento de rota
pathping google.com                     # Análise de perda de pacotes
nslookup google.com                     # Consulta DNS
telnet google.com 80                    # Teste de porta

# Conexões de rede
netstat -an                             # Todas as conexões
netstat -r                              # Tabela de roteamento
netstat -b                              # Mostra executáveis (admin)
arp -a                                  # Tabela ARP
route print                             # Tabela de roteamento
```

## 3.2 Ferramentas de Monitoramento

### **Performance e Recursos:**
```cmd
# Tasklist e Taskkill
tasklist                                # Lista processos
tasklist /svc                           # Processos com serviços
tasklist /m                             # Processos com DLLs carregadas
taskkill /im notepad.exe                # Mata processo por nome
taskkill /pid 1234                      # Mata processo por PID
taskkill /f /im chrome.exe              # Força término

# Wmic - Windows Management Instrumentation
wmic process list brief                 # Lista processos
wmic service list brief                 # Lista serviços
wmic product list brief                 # Programas instalados
wmic logicaldisk get size,freespace,caption  # Espaço em disco
wmic cpu get name,numberofcores         # Informações da CPU
```

### **PowerShell para Monitoramento:**
```powershell
# Performance Counters
Get-Counter "\Processor(_Total)\% Processor Time"     # Uso da CPU
Get-Counter "\Memory\Available MBytes"                # Memória disponível
Get-Counter "\LogicalDisk(C:)\% Free Space"          # Espaço livre em disco

# Monitoramento contínuo
while($true) {
    Get-Counter "\Processor(_Total)\% Processor Time" | 
    Select-Object -ExpandProperty CounterSamples | 
    Select-Object CookedValue, Timestamp
    Start-Sleep 2
}

# Processos consumindo mais recursos
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 5
```

## 3.3 Segurança e Backup

### **Controle de Conta de Usuário (UAC):**
```cmd
# Executar como administrador
runas /user:Administrator "cmd"         # Executa CMD como admin
runas /user:DOMAIN\admin "program.exe"  # Executa com usuário específico
```

### **Backup e Restauração:**
```powershell
# Backup com PowerShell
$Source = "C:\ImportantData"
$Destination = "D:\Backup"
Copy-Item -Path $Source -Destination $Destination -Recurse -Force

# Backup incremental
robocopy C:\Data D:\Backup /MIR /R:3 /W:10 /LOG:backup.log


```

### **Criptografia:**
```cmd
# Cipher - Ferramenta de criptografia
cipher /e /s:C:\PrivateFolder          # Criptografa pasta
cipher /d /s:C:\PrivateFolder          # Descriptografa pasta
cipher /w:C:\                          # Limpa espaço livre (seguro)

# BitLocker (PowerShell)
Get-BitLockerVolume                     # Status do BitLocker
Enable-BitLocker -MountPoint "C:" -EncryptionMethod XTS_AES256
```

## 3.4 Scripts e Automação

### **Scripts Batch (.bat/.cmd):**
```batch
@echo off
:: Comentário
echo Olá, Mundo!

:: Variáveis
set nome=João
echo Olá, %nome%!

:: Condicionais
if exist arquivo.txt (
    echo Arquivo existe
) else (
    echo Arquivo não existe
)

:: Loops
for %%i in (*.txt) do (
    echo Processando %%i
)

:: Entrada do usuário
set /p nome=Digite seu nome: 
echo Olá, %nome%!

:: Parâmetros
echo Primeiro parâmetro: %1
echo Segundo parâmetro: %2
echo Todos os parâmetros: %*

:: Exemplo prático - Backup automatizado
set origem=C:\Documentos
set destino=D:\Backup\%date:~-4,4%-%date:~-7,2%-%date:~-10,2%
mkdir "%destino%"
xcopy "%origem%" "%destino%" /e /y
echo Backup concluído em %destino%
```

### **Scripts PowerShell Básicos:**
```powershell
# Variáveis e tipos
$nome = "João"
$idade = 30
$lista = @("item1", "item2", "item3")
$hash = @{Nome="João"; Idade=30}

# Condicionais
if ($idade -gt 18) {
    Write-Host "Maior de idade"
} else {
    Write-Host "Menor de idade"
}

# Loops
foreach ($item in $lista) {
    Write-Host "Processando: $item"
}

for ($i = 1; $i -le 10; $i++) {
    Write-Host "Número: $i"
}

# Funções
function Get-SystemUptime {
    $uptime = (Get-Date) - (Get-CimInstance Win32_OperatingSystem).LastBootUpTime
    return $uptime.Days
}

$dias = Get-SystemUptime
Write-Host "Sistema ligado há $dias dias"

# Exemplo prático - Limpeza de arquivos temporários
function Clear-TempFiles {
    $tempPaths = @(
        $env:TEMP,
        "C:\Windows\Temp",
        "$env:USERPROFILE\AppData\Local\Temp"
    )
    
    foreach ($path in $tempPaths) {
        if (Test-Path $path) {
            Get-ChildItem $path -Recurse | Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
            Write-Host "Limpeza concluída: $path"
        }
    }
}

Clear-TempFiles
```

---

# MÓDULO 4: SOLUÇÃO DE PROBLEMAS E MANUTENÇÃO

## 4.1 Diagnóstico do Sistema

### **Visualizador de Eventos:**
```powershell
# Acessar logs específicos
Get-WinEvent -LogName System | Where-Object {$_.LevelDisplayName -eq "Error"} | Select-Object -First 10
Get-WinEvent -LogName Application | Where-Object {$_.Id -eq 1000}  # Erro específico

# Buscar por tempo
$StartTime = (Get-Date).AddHours(-24)
Get-WinEvent -LogName System | Where-Object {$_.TimeCreated -gt $StartTime}

# Exportar logs
Get-WinEvent -LogName System | Export-Csv -Path "C:\Logs\sistema.csv" -NoTypeInformation
```

### **Ferramentas de Diagnóstico:**
```cmd
# SFC - System File Checker
sfc /scannow                            # Verifica integridade dos arquivos
sfc /verifyonly                         # Apenas verifica, não corrige
sfc /scanfile=C:\Windows\System32\kernel32.dll  # Verifica arquivo específico

# DISM - Deployment Image Servicing and Management
dism /online /cleanup-image /checkhealth      # Verifica saúde da imagem
dism /online /cleanup-image /scanhealth       # Scan completo
dism /online /cleanup-image /restorehealth    # Restaura saúde da imagem

# Verificação de memória
mdsched                                 # Agendamento da verificação de memória
```

### **Performance e Otimização:**
```cmd
# Desfragmentação
defrag C: /a                            # Analisa fragmentação
defrag C: /o                            # Desfragmenta
defrag C: /x                            # Consolida espaço livre

# Limpeza de disco
cleanmgr                                # Interface gráfica
cleanmgr /sageset:1                     # Configurar perfil de limpeza
cleanmgr /sagerun:1                     # Executar perfil configurado

# Arquivos de sistema
dism /online /cleanup-image /startcomponentcleanup  # Limpeza de componentes
```

## 4.2 Recuperação e Restauração

### **Pontos de Restauração:**
```powershell
# Verificar pontos de restauração
Get-ComputerRestorePoint

# Criar ponto de restauração
Checkpoint-Computer -Description "Antes da instalação do software"

# Habilitar restauração do sistema
Enable-ComputerRestore -Drive "C:\"
```

### **Modo de Recuperação:**
```cmd
# Boot em modo de segurança (executar como admin)
bcdedit /set {default} safeboot minimal          # Modo de segurança
bcdedit /set {default} safeboot network          # Modo de segurança com rede
bcdedit /deletevalue {default} safeboot          # Remover modo de segurança

# Reparação de inicialização
bootrec /scanos                         # Escaneia instalações do Windows
bootrec /rebuildbcd                     # Reconstrói BCD
bootrec /fixmbr                         # Corrige MBR
bootrec /fixboot                        # Corrige setor de boot
```

## 4.3 Gerenciamento de Drivers

### **Drivers e Dispositivos:**
```powershell
# Listar drivers
Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, DriverDate

# Dispositivos com problemas
Get-WmiObject Win32_PnPSignedDriver | Where-Object {$_.IsSigned -eq $false}

# Atualizar drivers (Windows 10/11)
Get-WindowsDriver -Online | Where-Object {$_.BootCritical -eq $false}
```

```cmd
# DRIVERQUERY - Informações de drivers
driverquery                             # Lista todos os drivers
driverquery /v                          # Informações detalhadas
driverquery /si                         # Drivers assinados

# PNPUTIL - Gerenciamento de drivers
pnputil /enum-drivers                   # Lista drivers de terceiros
pnputil /add-driver driver.inf          # Instala driver
pnputil /delete-driver driver.inf       # Remove driver
```

## 4.4 Manutenção Preventiva

### **Scripts de Manutenção Automatizada:**
```powershell
# Script completo de manutenção
function Start-SystemMaintenance {
    Write-Host "Iniciando manutenção do sistema..." -ForegroundColor Green
    
    # 1. Limpeza de arquivos temporários
    Write-Host "Limpando arquivos temporários..."
    Get-ChildItem $env:TEMP -Recurse | Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
    
    # 2. Verificação de integridade do sistema
    Write-Host "Verificando integridade dos arquivos do sistema..."
    sfc /scannow
    
    # 3. Limpeza de logs antigos
    Write-Host "Limpando logs antigos..."
    Get-WinEvent -ListLog * | Where-Object {$_.RecordCount -gt 0} | ForEach-Object {
        try {
            [System.Diagnostics.Eventing.Reader.EventLogSession]::GlobalSession.ClearLog($_.LogName)
        } catch {
            Write-Warning "Não foi possível limpar o log: $($_.LogName)"
        }
    }
    
    # 4. Otimização do registro
    Write-Host "Otimizando registro..."
    Get-ChildItem "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" | 
    Where-Object {$_.GetValue("DisplayName") -like "*temp*"} | 
    Remove-Item -Recurse -ErrorAction SilentlyContinue
    
    # 5. Verificação de disco
    Write-Host "Verificando disco C:..."
    chkdsk C: /f /r
    
    # 6. Atualização do sistema
    Write-Host "Verificando atualizações..."
    if (Get-Module -ListAvailable -Name PSWindowsUpdate) {
        Get-WUInstall -AcceptAll -AutoReboot
    } else {
        Write-Warning "Módulo PSWindowsUpdate não instalado. Instale com: Install-Module PSWindowsUpdate"
    }
    
    Write-Host "Manutenção concluída!" -ForegroundColor Green
}

# Executar manutenção
Start-SystemMaintenance
```

### **Agendamento de Manutenção:**
```powershell
# Criar tarefa agendada para manutenção semanal
$TaskName = "ManutencaoSemanal"
$Action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-File C:\Scripts\manutencao.ps1"
$Trigger = New-ScheduledTaskTrigger -Weekly -DaysOfWeek Sunday -At "02:00"
$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries
$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

Register-ScheduledTask -TaskName $TaskName -Action $Action -Trigger $Trigger -Settings $Settings -Principal $Principal
```

### **Monitoramento de Saúde do Sistema:**
```powershell
# Script de monitoramento
function Get-SystemHealth {
    $health = @{}
    
    # CPU
    $cpu = Get-Counter "\Processor(_Total)\% Processor Time" -SampleInterval 1 -MaxSamples 5
    $avgCPU = ($cpu.CounterSamples | Measure-Object CookedValue -Average).Average
    $health.CPU = [math]::Round($avgCPU, 2)
    
    # Memória
    $totalRAM = (Get-CimInstance Win32_PhysicalMemory | Measure-Object Capacity -Sum).Sum / 1GB
    $freeRAM = (Get-Counter "\Memory\Available MBytes").CounterSamples.CookedValue / 1024
    $health.MemoryUsedPercent = [math]::Round((($totalRAM - $freeRAM) / $totalRAM) * 100, 2)
    
    # Disco
    $disk = Get-WmiObject Win32_LogicalDisk -Filter "DeviceID='C:'"
    $health.DiskUsedPercent = [math]::Round((($disk.Size - $disk.FreeSpace) / $disk.Size) * 100, 2)
    
    # Temperatura (se disponível)
    try {
        $temp = Get-WmiObject MSAcpi_ThermalZoneTemperature -Namespace "root/wmi"
        $health.Temperature = [math]::Round(($temp.CurrentTemperature / 10 - 273.15), 2)
    } catch {
        $health.Temperature = "N/A"
    }
    
    # Status dos serviços críticos
    $criticalServices = @("Spooler", "BITS", "Themes", "AudioSrv")
    $health.ServicesStatus = @{}
    foreach ($service in $criticalServices) {
        $health.ServicesStatus[$service] = (Get-Service $service).Status
    }
    
    return $health
}

# Usar o monitoramento
$systemHealth = Get-SystemHealth
Write-Host "=== SAÚDE DO SISTEMA ===" -ForegroundColor Cyan
Write-Host "CPU: $($systemHealth.CPU)%" -ForegroundColor $(if($systemHealth.CPU -gt 80) {"Red"} else {"Green"})
Write-Host "Memória: $($systemHealth.MemoryUsedPercent)%" -ForegroundColor $(if($systemHealth.MemoryUsedPercent -gt 80) {"Red"} else {"Green"})
Write-Host "Disco C: $($systemHealth.DiskUsedPercent)%" -ForegroundColor $(if($systemHealth.DiskUsedPercent -gt 90) {"Red"} else {"Green"})
Write-Host "Temperatura: $($systemHealth.Temperature)°C"
Write-Host "Serviços críticos:"
$systemHealth.ServicesStatus.GetEnumerator() | ForEach-Object {
    $color = if($_.Value -eq "Running") {"Green"} else {"Red"}
    Write-Host "  $($_.Key): $($_.Value)" -ForegroundColor $color
}
```

---

--

# RECURSOS ADICIONAIS

## **Variáveis de Ambiente Importantes:**
```cmd
%USERPROFILE%    # C:\Users\[usuario]
%APPDATA%        # C:\Users\[usuario]\AppData\Roaming  
%LOCALAPPDATA%   # C:\Users\[usuario]\AppData\Local
%TEMP%           # Pasta temporária do usuário
%WINDIR%         # C:\Windows
%SYSTEMROOT%     # C:\Windows
%PROGRAMFILES%   # C:\Program Files
%PROGRAMFILES(X86)% # C:\Program Files (x86)
%PATH%           # Caminhos de executáveis
%COMPUTERNAME%   # Nome do computador
%USERNAME%       # Nome do usuário atual
```

## **Atalhos de Teclado Úteis:**
- `Win + R` - Executar
- `Win + X` - Menu de administração
- `Win + I` - Configurações
- `Win + L` - Bloquear tela
- `Ctrl + Shift + Esc` - Gerenciador de tarefas
- `Win + Pause` - Propriedades do sistema
- `Win + E` - Explorador de arquivos
- `Alt + F4` - Fechar aplicativo
- `F2` - Renomear
- `Delete` - Lixeira
- `Shift + Delete` - Exclusão permanente


## **Códigos de Erro Comuns:**
- **0x80070005** - Acesso negado
- **0x80070002** - Arquivo não encontrado
- **0x8007000E** - Memória insuficiente
- **0x80070020** - Arquivo em uso
- **0x800F0922** - Erro de Windows Update
- **0xC0000135** - DLL não encontrada
- **Blue Screen (BSOD)** - Erro crítico do sistema

## **Logs Importantes:**
- **System** - Eventos do sistema e drivers
- **Application** - Eventos de aplicativos
- **Security** - Eventos de segurança e auditoria
- **Setup** - Instalação e atualizações
- **Microsoft-Windows-PowerShell/Operational** - Atividades do PowerShell
