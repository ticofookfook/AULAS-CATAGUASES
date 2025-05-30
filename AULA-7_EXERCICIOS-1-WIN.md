# EXERCÍCIOS PRÁTICOS - MÓDULOS 3 E 4

---

## MÓDULO 3: FERRAMENTAS ADMINISTRATIVAS

### **Exercício 1: Auditoria do Sistema**
**Cenário:** Você precisa documentar as configurações de um computador novo.

**Tarefas:**
```cmd
# Execute e anote os resultados:
systeminfo | findstr /C:"Nome do sistema" /C:"Versão" /C:"Memória"
hostname
whoami
ipconfig | findstr "IPv4"
```

**Entregável:** Preencha esta tabela:
- Nome do PC: _______________
- Versão Windows: _______________
- RAM Total: _______________
- Seu IP: _______________

---

### **Exercício 2: Diagnóstico de Rede**
**Cenário:** Internet lenta. Investigue o problema.

**Procedimento:**
1. Teste conectividade básica:
```cmd
ping google.com
ping 8.8.8.8
```

2. Verifique configuração de rede:
```cmd
ipconfig /all
```

3. Teste resolução DNS:
```cmd
nslookup google.com
```

4. Limpe e renove configurações:
```cmd
ipconfig /flushdns
ipconfig /release
ipconfig /renew
```

**Questão:** Se `ping google.com` falhar mas `ping 8.8.8.8` funcionar, qual é o problema?

---

### **Exercício 3: Monitoramento de Processos**
**Cenário:** Computador lento. Identifique o culpado.

**Investigação:**
```cmd
# Veja todos os processos
tasklist

# Processos com mais detalhes
tasklist /v

# Encontre processos específicos
tasklist | findstr "chrome"
tasklist | findstr "firefox"
```

**PowerShell para análise:**
```powershell
# Top 5 processos por CPU
Get-Process | Sort-Object CPU -Descending | Select-Object Name, CPU -First 5

# Top 5 processos por Memória  
Get-Process | Sort-Object WorkingSet -Descending | Select-Object Name, @{n="MemoriaMB";e={[math]::Round($_.WorkingSet/1MB,2)}} -First 5
```


---

### **Exercício 4: Gerenciamento de Discos**
**Cenário:** Precisa verificar espaço em disco e fazer limpeza.

**Verificação:**
```cmd
# Espaço em disco - método simples
dir C:\ /-c

# Informações detalhadas
wmic logicaldisk get size,freespace,caption
```

**PowerShell mais legível:**
```powershell
Get-WmiObject Win32_LogicalDisk | 
Select-Object DeviceID, 
@{n="TamanhoGB";e={[math]::Round($_.Size/1GB,2)}}, 
@{n="LivreGB";e={[math]::Round($_.FreeSpace/1GB,2)}}, 
@{n="Usado%";e={[math]::Round((($_.Size-$_.FreeSpace)/$_.Size)*100,2)}}
```

**Limpeza básica:**
```cmd
# Interface gráfica
cleanmgr

# Limpar arquivos temp manualmente
del /q /s %temp%\*.*
```

---

---

### **Exercício 5: Script de Manutenção Básica**
**Objetivo:** Criar um script que faça limpeza automatizada.

**Crie um arquivo `manutencao.bat`:**
```batch
@echo off
echo ================================================
echo           MANUTENÇÃO AUTOMÁTICA
echo ================================================
echo.

echo [1/4] Limpando arquivos temporários...
del /q /s %temp%\*.* >nul 2>&1
echo Concluído!

echo [2/4] Limpando cache DNS...
ipconfig /flushdns >nul
echo Concluído!

echo [3/4] Verificando conectividade...
ping -n 1 google.com >nul
if %errorlevel%==0 (
    echo Internet: OK
) else (
    echo Internet: PROBLEMA
)

echo [4/4] Gerando relatório...
echo === RELATÓRIO DE MANUTENÇÃO === > manutencao_log.txt
echo Data: %date% %time% >> manutencao_log.txt
echo Usuário: %username% >> manutencao_log.txt
echo Computador: %computername% >> manutencao_log.txt
echo.

echo ================================================
echo Manutenção concluída! Verifique manutencao_log.txt
echo ================================================
pause
```

---


### **Exercício 6: Gerenciamento de Serviços**
**Cenário:** Alguns programas não estão funcionando corretamente. Você precisa verificar e gerenciar os serviços do sistema.

**Tarefas:**
```cmd
# Listar todos os serviços
sc query state= all

# Verificar serviços específicos
sc query "Spooler"
sc query "BITS"
sc query "Themes"

# Ver serviços em execução
net start
```

**PowerShell para análise detalhada:**
```powershell
# Serviços parados que deveriam estar rodando
Get-Service | Where-Object {$_.Status -eq "Stopped" -and $_.StartType -eq "Automatic"}

# Status dos serviços críticos
Get-Service | Where-Object {$_.Name -match "Spooler|BITS|Windows Update|DNS"} | Select-Object Name, Status, StartType
```

**Entregável:** Complete:
- Quantos serviços estão rodando: _______________
- Status do serviço Spooler: _______________

---

### **Exercício 7: Análise de Disco e Performance**
**Cenário:** HD quase cheio e sistema lento. Faça um diagnóstico completo.

**Procedimento:**
1. **Verificar espaço em disco:**
```cmd
dir C:\ /-c
fsutil volume diskfree C:
```

2. **Encontrar arquivos grandes:**
```cmd
forfiles /p C:\ /s /m *.* /c "cmd /c if @fsize GEQ 104857600 echo @path @fsize"
```

**Questão:** Se o disco C: tem menos ou mais de 15% livre ?

---

### **Exercício 8: Troubleshooting de Aplicativos**
**Cenário:** Um aplicativo específico não abre ou trava constantemente.

**Investigação:**
```cmd
# Verificar se o processo está rodando
tasklist | findstr "notepad"
tasklist | findstr "calculator"

# Verificar arquivos em uso
openfiles /query

# Finalizar processo travado
taskkill /f /im "notepad.exe"
taskkill /f /pid 1234

# Verificar logs de erro
eventvwr.msc
```

**PowerShell para diagnóstico:**
```powershell
# Processos que não respondem
Get-Process | Where-Object {$_.Responding -eq $false}

# Histórico de crashes da aplicação
Get-WinEvent -FilterHashtable @{LogName='Application'; Level=2} -MaxEvents 20 | 
Where-Object {$_.ProviderName -like "*Application Error*"} | 
Select-Object TimeCreated, Id, LevelDisplayName, Message
```

**Tarefa prática:**
1. Abra o Bloco de Notas
2. Execute os comandos para encontrá-lo
3. Force o encerramento usando taskkill
4. Documente o PID que foi usado

---
