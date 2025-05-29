**1. Gerenciamento de Usuário**
Criar um script que:
- Receba um nome de usuário como parâmetro
- Verifique se o usuário já existe
- Se não existir, crie o usuário
- Se existir, exiba uma mensagem informando

**2. Criação de Grupo e Adição de Usuário**
Criar um script que:
- Receba nome do usuário e nome do grupo
- Crie o grupo se não existir
- Adicione o usuário ao grupo
- Exiba confirmação das ações realizadas


**3. Backup Simples**
Criar um script que:
- Receba um diretório como parâmetro
- Verifique se o diretório existe
- Crie um backup compactado (.tar.gz) com data/hora no nome
- Salve em /tmp/backups/

**4. Monitor de Espaço em Disco**
Criar um script que:
- Verifique o uso do disco da partição /
- Se o uso for > 80%, exiba alerta
- Se for > 90%, envie um email de alerta (simular com echo)


**5. Limpeza de Logs Antigos**
Criar um script que:
- Encontre arquivos .log em /var/log mais antigos que 30 dias
- Liste os arquivos encontrados
- Pergunte ao usuário se deseja deletar
- Execute a ação conforme resposta

**6. Gerador de Relatório de Sistema**
Criar um script que gere um relatório com:
- Data/hora atual
- Uptime do sistema
- Uso de CPU e memória
- Espaço em disco de todas as partições

**7. Sincronizador de Diretórios**
Criar um script que:
- Receba dois diretórios como parâmetros
- Compare o conteúdo dos diretórios


**8. Monitor de Processos**
Criar um script que:
- Receba nome de um processo como parâmetro
- Monitore se o processo está rodando
- Se parar, tente reiniciar automaticamente
- Registre tentativas em um log
- Limite a 3 tentativas de restart



**9. Menu Interativo de Administração**
Criar um script com menu que permita:
- Criar/remover usuários
- Gerenciar grupos
- Ver status de serviços
- Fazer backup de diretórios
- Ver relatório do sistema
- Sair do programa


########################################


### 21 - FTP
```bash
ftp <target>

```bash
ssh <user>@<target>

```bash
telnet <target> 23
```
### 25/587 - SMTP
```bash
nc <target> 25
```
### 53 - DNS
```bash
dig @<target> axfr <domain>
dnsrecon -d <domain> -t axfr
```
### 80/443 - HTTP/HTTPS
```bash
gobuster dir -u http://<target> -w /usr/share/seclists/Discovery/Web-Content/common.txt
nikto -h http://<target>
```
### 110/995 - POP3
```bash
nc <target> 110
USER <username>
PASS <password>
```

### 135/445 - SMB (Windows)
```bash
smbclient -L //<target>/
smbmap -H <target>
enum4linux <target>
```
### 139/445 - NetBIOS
```bash
nbtscan <target>
smbclient -L //<target>/ -N
```
### 993/143 - IMAP
```bash
nc <target> 143
LOGIN <user> <pass>
```

### 1521 - Oracle
```bash
sqlplus <user>/<password>@<target>:1521/XE
```

### 2049 - NFS
```bash
showmount -e <target>
mount -t nfs <target>:/path /mnt/nfs
```

### 3306 - MySQL
```bash
mysql -h <target> -u root -p
```

### 3389 - RDP
```bash
rdesktop <target>
xfreerdp /v:<target> /u:<user> /p:<password>
```

### 5432 - PostgreSQL
```bash
psql -h <target> -U postgres
```

### 5985/5986 - WinRM
```bash
evil-winrm -i <target> -u <user> -p <password>
```

### 6379 - Redis
```bash
redis-cli -h <target>
info
keys *
```

### 11211 - Memcached
```bash
nc <target> 11211
stats
```

### Enum Web Rápido
```bash
for port in 80 443 8080 8000 8443; do curl -s http://<target>:$port | head -20; done
```
