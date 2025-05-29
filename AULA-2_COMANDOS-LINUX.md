# MÓDULO 1: PROCESSOS E SISTEMA ()

## 1.1 Gerenciamento de Processos

### **ps** - Process Status
```bash
ps                    # Processos do usuário atual
ps aux                # Todos os processos detalhados
ps -ef                # Formato estendido
ps -u usuario         # Processos de usuário específico
ps -C comando         # Processos por nome do comando
ps --forest           # Árvore de processos
```

### **top/htop** - Monitor de Processos
```bash
top                   # Monitor interativo
htop                  # Versão melhorada (se instalado)
```

**Comandos no top:**
- `q`: sair
- `k`: matar processo
- `r`: alterar prioridade
- `M`: ordenar por memória
- `P`: ordenar por CPU
- `1`: mostrar cores separados

### **jobs** - Jobs do Shell
```bash
jobs                  # Lista jobs em background
bg                    # Coloca job em background
fg                    # Traz job para foreground
fg %1                 # Traz job 1 para foreground
```

### **nohup** - No Hang Up
```bash
nohup comando &       # Executa em background persistente
nohup script.sh > output.log 2>&1 &
```

### **screen/tmux** - Terminal Multiplexer
```bash
# Screen
screen                # Inicia sessão screen
screen -S nome        # Sessão com nome
screen -ls            # Lista sessões
screen -r sessão      # Reconecta à sessão

# Dentro do screen:
# Ctrl+A, D (detach)
# Ctrl+A, K (kill)

# tmux
tmux                  # Inicia sessão
tmux new -s nome      # Sessão com nome
tmux ls               # Lista sessões
tmux attach -t nome   # Reconecta
```

### **kill/killall** - Terminar Processos
```bash
kill PID              # Mata processo por PID
kill -9 PID           # Força término (SIGKILL)
kill -15 PID          # Término gracioso (SIGTERM)
killall nome          # Mata por nome do processo
pkill nome            # Mata por padrão de nome
```

### **Sinais Importantes:**
- `SIGTERM (15)`: Término normal
- `SIGKILL (9)`: Término forçado
- `SIGHUP (1)`: Reload configuração
- `SIGSTOP (19)`: Pausa processo
- `SIGCONT (18)`: Continua processo

## 1.2 Monitoramento do Sistema

### **Informações do Sistema:**
```bash
uname -a              # Informações completas do sistema
hostname              # Nome do host
uptime                # Tempo de atividade e carga
date                  # Data e hora atual
cal                   # Calendário
```

### **CPU e Memória:**
```bash
# CPU
lscpu                 # Informações detalhadas da CPU
cat /proc/cpuinfo     # Informações do processador
nproc                 # Número de processadores

# Memória
free -h               # Uso de memória (human readable)
free -m               # Em megabytes
cat /proc/meminfo     # Informações detalhadas
```

### **Disco:**
```bash
df -h                 # Uso de disco (human readable)
df -i                 # Uso de inodes
du -h diretorio       # Uso por diretório
du -sh *              # Resumo por item no diretório
lsblk                 # Lista dispositivos de bloco
fdisk -l              # Lista discos (root)
```

### **Rede:**
```bash
ifconfig              # Configuração de interfaces
ip addr show          # Endereços IP (alternativa moderna)
ip route show         # Tabela de roteamento
netstat -tuln         # Portas em uso
ss -tuln              # Alternativa moderna ao netstat
ping host             # Testa conectividade
wget URL              # Download de arquivos
curl URL              # Transfer de dados
```

### **I/O:**
```bash
iostat                # Estatísticas de I/O
iotop                 # Monitor de I/O por processo
lsof                  # Lista arquivos abertos
lsof -i :80           # Arquivos abertos na porta 80
```

## 1.3 Serviços e Daemons

### **systemd** (Sistemas modernos)
```bash
# Status de serviços
systemctl status serviço
systemctl list-units --type=service
systemctl list-unit-files

# Controle de serviços
systemctl start serviço       # Inicia
systemctl stop serviço        # Para
systemctl restart serviço     # Reinicia
systemctl reload serviço      # Recarrega configuração

# Habilitação automática
systemctl enable serviço      # Habilita no boot
systemctl disable serviço     # Desabilita no boot

# Logs de serviços
journalctl -u serviço         # Logs do serviço
journalctl -f                 # Segue logs em tempo real
journalctl --since "1 hour ago"
```

### **SysV Init** (Sistemas antigos)
```bash
service serviço start         # Inicia serviço
service serviço stop          # Para serviço
service serviço status        # Status do serviço
/etc/init.d/serviço restart   # Reinicia serviço

# Níveis de execução
runlevel                      # Nível atual
telinit 3                     # Muda para runlevel 3
```

### **Tipos de Runlevels:**
- 0: Halt (desligar)
- 1: Single user (manutenção)
- 2: Multi-user sem rede
- 3: Multi-user com rede
- 4: Não definido
- 5: Multi-user com interface gráfica
- 6: Reboot

## 1.4 Variáveis de Ambiente

### **Visualizar Variáveis:**
```bash
env                   # Todas as variáveis
echo $HOME            # Variável específica
echo $PATH            # Caminho dos executáveis
echo $USER            # Usuário atual
printenv              # Alternativa ao env
set                   # Variáveis do shell
```

### **Definir Variáveis:**
```bash
# Temporária (sessão atual)
VARIAVEL=valor
export VARIAVEL=valor

# Permanente (usuário)
echo 'export VARIAVEL=valor' >> ~/.bashrc
echo 'export VARIAVEL=valor' >> ~/.profile

# Permanente (sistema)
echo 'export VARIAVEL=valor' >> /etc/environment
```

### **Variáveis Importantes:**
- `$HOME`: Diretório home do usuário
- `$PATH`: Caminhos de execução
- `$USER`: Nome do usuário
- `$SHELL`: Shell atual
- `$PWD`: Diretório atual
- `$OLDPWD`: Diretório anterior
- `$?`: Código de retorno do último comando

### **Configuração do Shell:**
```bash
# Bash
~/.bashrc             # Configurações interativas
~/.bash_profile       # Login shells
~/.bash_aliases       # Aliases personalizados

# Arquivos globais
/etc/bash.bashrc      # Configurações globais
/etc/profile          # Login global
```

---

# MÓDULO 2: ADMINISTRAÇÃO AVANÇADA ()

## 2.1 Gerenciamento de Pacotes

### **APT** (Debian/Ubuntu)
```bash
# Atualização
apt update                    # Atualiza lista de pacotes
apt upgrade                   # Atualiza pacotes instalados
apt full-upgrade              # Atualização completa
apt dist-upgrade              # Atualização de distribuição

# Instalação/Remoção
apt install pacote            # Instala pacote
apt remove pacote             # Remove pacote
apt purge pacote              # Remove completamente
apt autoremove                # Remove dependências órfãs

# Busca
apt search termo              # Busca pacotes
apt show pacote               # Informações do pacote
apt list --installed          # Pacotes instalados

# Cache
apt clean                     # Limpa cache
apt autoclean                 # Limpa cache antigo
```

### **YUM/DNF** (Red Hat/Fedora)
```bash
# YUM (RHEL/CentOS)
yum update                    # Atualiza sistema
yum install pacote            # Instala pacote
yum remove pacote             # Remove pacote
yum search termo              # Busca pacotes
yum info pacote               # Informações
yum list installed            # Pacotes instalados

# DNF (Fedora moderno)
dnf update                    # Atualiza sistema
dnf install pacote            # Instala pacote
dnf remove pacote             # Remove pacote
dnf search termo              # Busca pacotes
```

### **Pacman** (Arch Linux)
```bash
pacman -Syu                   # Atualiza sistema
pacman -S pacote              # Instala pacote
pacman -R pacote              # Remove pacote
pacman -Rs pacote             # Remove com dependências
pacman -Ss termo              # Busca pacotes
pacman -Q                     # Pacotes instalados
```

### **SNAP** (Universal)
```bash
snap list                     # Lista snaps instalados
snap find termo               # Busca snaps
snap install pacote           # Instala snap
snap remove pacote            # Remove snap
snap refresh                  # Atualiza snaps
```

## 2.2 Configuração de Rede

### **Interfaces de Rede:**
```bash
# Configuração básica
ifconfig eth0 up              # Ativa interface
ifconfig eth0 down            # Desativa interface
ifconfig eth0 192.168.1.100   # Define IP

# iproute2 (moderno)
ip link show                  # Mostra interfaces
ip addr add 192.168.1.100/24 dev eth0
ip route add default via 192.168.1.1
```

### **Configuração Estática (Ubuntu):**
```bash
# /etc/netplan/01-network.yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4

# Aplicar configuração
netplan apply
```

### **DNS:**
```bash
# /etc/resolv.conf
nameserver 8.8.8.8
nameserver 8.8.4.4

# Teste DNS
nslookup dominio.com
dig dominio.com
host dominio.com
```

### **Firewall:**
```bash
# UFW (Ubuntu)
ufw status                    # Status do firewall
ufw enable                    # Habilita firewall
ufw disable                   # Desabilita firewall
ufw allow 22                  # Permite porta 22
ufw deny 80                   # Bloqueia porta 80
ufw allow from 192.168.1.0/24

# iptables (avançado)
iptables -L                   # Lista regras
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

### **SSH:**
```bash
# Cliente SSH
ssh usuario@servidor          # Conecta via SSH
ssh -p 2222 usuario@servidor  # Porta específica
ssh -i chave.pem usuario@servidor  # Com chave privada

# Geração de chaves
ssh-keygen -t rsa -b 4096     # Gera par de chaves
ssh-copy-id usuario@servidor  # Copia chave pública

# Configuração SSH (/etc/ssh/sshd_config)
Port 22
PermitRootLogin no
PasswordAuthentication yes
PubkeyAuthentication yes
```

## 2.3 Cron e Agendamento

### **crontab** - Agendamento de Tarefas
```bash
crontab -l                    # Lista tarefas do usuário
crontab -e                    # Edita tarefas do usuário
crontab -r                    # Remove todas as tarefas
crontab -u usuario -l         # Lista tarefas de usuário (root)
```

### **Formato do Crontab:**
```
# Minuto(0-59) Hora(0-23) Dia(1-31) Mês(1-12) DiaSemana(0-7)
# * significa "qualquer valor"

# Exemplos de agendamento:
0 0 * * *    /script/backup.sh       # Todo dia à meia-noite
30 8 * * 1-5 /script/trabalho.sh     # 8:30 de segunda a sexta
0 */6 * * *  /script/verificar.sh    # A cada 6 horas
15 14 1 * *  /script/mensal.sh       # Dia 1 de cada mês às 14:15
0 2 * * 0    /script/domingo.sh      # Todo domingo às 2:00
```

### **Diretórios de Cron:**
```bash
/etc/cron.hourly/             # Scripts executados a cada hora
/etc/cron.daily/              # Scripts executados diariamente
/etc/cron.weekly/             # Scripts executados semanalmente
/etc/cron.monthly/            # Scripts executados mensalmente
```

### **anacron** - Para sistemas não 24h
```bash
anacron -T                    # Testa configuração
anacron -f                    # Força execução
```

### **at** - Agendamento Único
```bash
at 15:30                      # Agenda para 15:30 hoje
at 9:00 tomorrow              # Amanhã às 9:00
at now + 1 hour               # Daqui a 1 hora

# Dentro do at:
at> /script/comando.sh
at> <Ctrl+D>

atq                           # Lista tarefas agendadas
atrm 1                        # Remove tarefa 1
```

## 2.4 Logs e Troubleshooting

### **Arquivos de Log Principais:**
```bash
/var/log/syslog               # Log geral do sistema
/var/log/auth.log             # Autenticação e autorização
/var/log/kern.log             # Kernel
/var/log/dmesg                # Boot e hardware
/var/log/apache2/             # Servidor web Apache
/var/log/nginx/               # Servidor web Nginx
/var/log/mysql/               # Banco de dados MySQL
```

### **journalctl** - SystemD Logs
```bash
journalctl                    # Todos os logs
journalctl -f                 # Acompanha em tempo real
journalctl -u apache2         # Logs de serviço específico
journalctl --since "1 hour ago"
journalctl --until "2023-12-31 23:59:59"
journalctl -p err             # Apenas erros
journalctl -b                 # Logs do boot atual
journalctl -b -1              # Boot anterior
journalctl --disk-usage       # Uso de disco dos logs
```

### **Log Rotation:**
```bash
# logrotate - rotaciona logs automaticamente
logrotate -f /etc/logrotate.conf    # Força rotação
logrotate -d /etc/logrotate.conf    # Modo debug

# Configuração exemplo (/etc/logrotate.d/meuapp)
/var/log/meuapp/*.log {
    daily
    missingok
    rotate 52
    compress
    delaycompress
    notifempty
    create 644 www-data www-data
    postrotate
        systemctl reload nginx
    endscript
}
```

### **Análise de Logs:**
```bash
# Busca em logs
grep "ERROR" /var/log/syslog
grep -i "failed" /var/log/auth.log
tail -f /var/log/syslog | grep ERROR

# Estatísticas de logs
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr
grep "404" /var/log/nginx/access.log | wc -l

# Logs de sistema
dmesg                         # Mensagens do kernel
dmesg | grep -i error        # Erros do kernel
dmesg -T                     # Com timestamp
```

### **Troubleshooting de Sistema:**

#### **Performance:**
```bash
# CPU
top                          # Monitor de processos
htop                         # Versão melhorada
sar -u 5 10                  # CPU a cada 5s por 10 vezes

# Memória
free -h                      # Uso de memória
ps aux --sort=-pmem | head   # Processos que mais usam RAM
cat /proc/meminfo            # Informações detalhadas

# Disco
iostat -x 1                  # I/O de disco
iotop                        # I/O por processo
df -h                        # Uso de espaço
lsof +D /var                 # Arquivos abertos em /var
```

#### **Rede:**
```bash
netstat -tuln                # Portas em uso
ss -tuln                     # Alternativa moderna
ping -c 5 8.8.8.8           # Teste de conectividade
traceroute google.com        # Rota até destino
mtr google.com               # Traceroute contínuo
tcpdump -i eth0              # Captura de pacotes
```

#### **Serviços:**
```bash
systemctl status --failed    # Serviços falhados
systemctl list-dependencies  # Dependências
systemctl show serviço       # Configuração detalhada
strace -p PID                # Rastreia chamadas de sistema
```

#### **Hardware:**
```bash
lshw                         # Hardware completo
lspci                        # Dispositivos PCI
lsusb                        # Dispositivos USB
lscpu                        # Informações da CPU
hdparm -I /dev/sda          # Informações do HD
smartctl -a /dev/sda        # S.M.A.R.T. do disco
```

---
## COMANDOS DE REFERÊNCIA RÁPIDA

### **Navegação e Arquivos:**
```bash
pwd, ls, cd, tree
mkdir, rmdir, touch, rm, cp, mv
cat, less, head, tail, file
find, locate, which, whereis, grep
tar, gzip, zip, unzip
```

### **Permissões e Usuários:**
```bash
chmod, chown, chgrp, umask
whoami, id, groups, su, sudo
useradd, userdel, usermod, passwd
groupadd, groupdel, groupmod
```

### **Processos e Sistema:**
```bash
ps, top, htop, jobs, bg, fg
kill, killall, pkill, nohup
screen, tmux
uname, hostname, uptime, date
free, df, du, lscpu
```

### **Rede e Serviços:**
```bash
ifconfig, ip, ping, wget, curl
netstat, ss, lsof
systemctl, service, journalctl
ssh, scp, rsync
```

### **Administração:**
```bash
apt, yum, dnf, pacman
crontab, at, anacron
logrotate, dmesg
mount, umount, fdisk
```

---

## RECURSOS ADICIONAIS

### **Documentação:**
- `man comando` - Manual do comando
- `info comando` - Informações detalhadas
- `comando --help` - Ajuda rápida
- `/usr/share/doc/` - Documentação instalada

### **Atalhos Úteis:**
- `Ctrl+C` - Interrompe comando
- `Ctrl+Z` - Suspende processo
- `Ctrl+D` - EOF/Logout
- `Ctrl+L` - Limpa tela
- `Tab` - Autocompletar
- `!!` - Último comando
- `!n` - Comando n do histórico

### **Comandos Histórico:**
- `history` - Mostra histórico
- `!123` - Executa comando 123
- `!!` - Último comando
- `!string` - Último comando começando com string
- `Ctrl+R` - Busca incremental no histórico

### **Redirecionamento:**
```bash
comando > arquivo      # Redireciona stdout
comando >> arquivo     # Append stdout
comando 2> erro.log    # Redireciona stderr
comando &> saida.log   # Redireciona ambos
comando | outro        # Pipe para outro comando
```

### **Expressões Regulares Básicas:**
- `.` - Qualquer caractere
- `*` - Zero ou mais repetições
- `^` - Início da linha
---

