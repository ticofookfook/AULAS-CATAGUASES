Sistema de Arquivos Linux

### **Estrutura de Diretórios:**
```
/          (raiz do sistema)
├── /bin   (binários essenciais)
├── /boot  (arquivos de boot)
├── /dev   (dispositivos)
├── /etc   (configurações)
├── /home  (diretórios dos usuários)
├── /lib   (bibliotecas)
├── /media (pontos de montagem removíveis)
├── /mnt   (pontos de montagem temporários)
├── /opt   (software opcional)
├── /proc  (sistema de arquivos virtual)
├── /root  (home do usuário root)
├── /run   (dados de tempo de execução)
├── /sbin  (binários do sistema)
├── /tmp   (arquivos temporários)
├── /usr   (recursos do usuário)
└── /var   (dados variáveis)
```

### **Caminhos:**
- **Absoluto**: Inicia com `/` (ex: `/home/usuario/documentos`)
- **Relativo**: Relativo ao diretório atual (ex: `documentos/arquivo.txt`)

### **Caracteres Especiais:**
- `.` (diretório atual)
- `..` (diretório pai)
- `~` (home do usuário)
- `*` (wildcard - qualquer conjunto de caracteres)
- `?` (wildcard - um caractere qualquer)



## 1.2 Comandos de Navegação

### **pwd** - Print Working Directory
```bash
pwd                    # Mostra o diretório atual
```

### **ls** - List
```bash
ls                     # Lista arquivos e diretórios
ls -l                  # Lista detalhada (long format)
ls -la                 # Inclui arquivos ocultos
ls -lh                 # Tamanhos legíveis (human readable)
ls -lt                 # Ordenado por tempo
ls -lr                 # Ordem reversa
ls /etc                # Lista conteúdo de /etc
ls *.txt               # Lista arquivos .txt
```

### **cd** - Change Directory
```bash
cd /home/usuario       # Navega para diretório específico
cd ~                   # Vai para home
cd ..                  # Sobe um nível
cd -                   # Volta ao diretório anterior
cd                     # Vai para home (sem parâmetros)
```

### **tree** - Estrutura em Árvore
```bash
tree                   # Mostra estrutura em árvore
tree -L 2              # Limita a 2 níveis
tree -a                # Inclui arquivos ocultos
```

## 1.3 Manipulação de Arquivos e Diretórios

### **mkdir** - Make Directory
```bash
mkdir diretorio        # Cria diretório
mkdir -p path/to/dir   # Cria diretórios pais se necessário
mkdir dir1 dir2 dir3   # Cria múltiplos diretórios
mkdir -m 755 diretorio # Cria com permissões específicas
```

### **rmdir** - Remove Directory
```bash
rmdir diretorio        # Remove diretório vazio
rmdir -p path/to/dir   # Remove diretório e pais vazios
```

### **touch** - Criar Arquivos
```bash
touch arquivo.txt      # Cria arquivo vazio
touch file1 file2      # Cria múltiplos arquivos
touch -t 202312251200 arquivo  # Define timestamp específico
```

### **cp** - Copy
```bash
cp origem destino      # Copia arquivo
cp -r dir1 dir2        # Copia diretório recursivamente
cp -i arquivo dest     # Confirmação interativa
cp -v arquivo dest     # Modo verboso
cp *.txt backup/       # Copia todos .txt para backup
```

### **mv** - Move/Rename
```bash
mv origem destino      # Move/renomeia arquivo
mv arquivo novo_nome   # Renomeia arquivo
mv *.log logs/         # Move todos .log para logs/
mv -i arquivo dest     # Confirmação interativa
```

### **rm** - Remove
```bash
rm arquivo             # Remove arquivo
rm -i arquivo          # Confirmação interativa
rm -r diretorio        # Remove diretório recursivamente
rm -f arquivo          # Force (sem confirmação)
rm -rf diretorio       # Remove recursiva e forçada
```

## 1.4 Visualização de Conteúdo

### **cat** - Concatenate
```bash
cat arquivo.txt        # Mostra conteúdo completo
cat file1 file2        # Concatena múltiplos arquivos
cat -n arquivo         # Mostra com números de linha
```

### **less/more** - Paginação
```bash
less arquivo.txt       # Visualização paginada
more arquivo.txt       # Visualização simples
```

**Navegação no less:**
- `Space`: próxima página
- `b`: página anterior
- `q`: sair

### **head/tail** - Início/Fim
```bash
head arquivo.txt       # Primeiras 10 linhas
head -n 5 arquivo      # Primeiras 5 linhas
tail arquivo.txt       # Últimas 10 linhas
tail -n 20 arquivo     # Últimas 20 linhas
tail -f arquivo.log    # Segue o arquivo (útil para logs)
```

### **file** - Tipo de Arquivo
```bash
file arquivo.txt       # Identifica tipo do arquivo
file -i arquivo        # Mostra MIME type
```

---

# MÓDULO 2: GERENCIAMENTO DE ARQUIVOS E USUÁRIOS ()

## 2.1 Permissões e Propriedades

### **Sistema de Permissões:**
Cada arquivo/diretório tem:
- **Proprietário** (user)
- **Grupo** (group)  
- **Outros** (others)

### **Tipos de Permissão:**
- **r** (read) = 4
- **w** (write) = 2
- **x** (execute) = 1

### **chmod** - Change Mode
```bash
chmod 755 arquivo      # rwxr-xr-x
chmod 644 arquivo      # rw-r--r--
chmod +x script.sh     # Adiciona execução
chmod -w arquivo       # Remove escrita
chmod u+x arquivo      # Adiciona execução para usuário
chmod g-w arquivo      # Remove escrita do grupo
chmod o=r arquivo      # Define apenas leitura para outros
```

### **Permissões Especiais:**
```bash
chmod +t diretorio     # Sticky bit (apenas dono remove)
chmod u+s arquivo      # SUID (executa como dono)
chmod g+s diretorio    # SGID (herda grupo)
```

### **chown** - Change Owner
```bash
chown usuario arquivo       # Muda proprietário
chown usuario:grupo arquivo # Muda proprietário e grupo
chown -R usuario dir/       # Recursivo
chown :grupo arquivo        # Muda apenas grupo
```

### **chgrp** - Change Group
```bash
chgrp grupo arquivo    # Muda grupo
chgrp -R grupo dir/    # Recursivo
```

### **umask** - Default Permissions
```bash
umask                  # Mostra umask atual
umask 022             # Define umask
umask -S              # Mostra em formato simbólico
```

## 2.2 Gerenciamento de Usuários e Grupos

### **Informações do Usuário:**
```bash
whoami                # Nome do usuário atual
id                    # ID do usuário e grupos
id usuario            # ID de usuário específico
groups                # Grupos do usuário atual
groups usuario        # Grupos de usuário específico
```

### **Usuários Logados:**
```bash
who                   # Usuários logados
w                     # Usuários e atividades
last                  # Histórico de logins
last usuario          # Logins de usuário específico
```

### **su** - Switch User
```bash
su                    # Muda para root
su -                  # Muda para root com ambiente
su usuario            # Muda para usuário específico
su - usuario          # Com ambiente do usuário
```

### **sudo** - Super User Do
```bash
sudo comando          # Executa comando como root
sudo -u usuario cmd   # Executa como usuário específico
sudo -l               # Lista permissões sudo
sudo su -             # Muda para root via sudo
```

### **Gerenciamento de Usuários (root):**
```bash
useradd usuario       # Cria usuário
useradd -m usuario    # Cria com diretório home
useradd -s /bin/bash -m usuario  # Define shell
userdel usuario       # Remove usuário
userdel -r usuario    # Remove com diretório home
usermod -a -G grupo usuario  # Adiciona a grupo
passwd usuario        # Muda senha
```

### **Gerenciamento de Grupos:**
```bash
groupadd grupo        # Cria grupo
groupdel grupo        # Remove grupo
groupmod -n novo antigo  # Renomeia grupo
```

### **Arquivos Importantes:**
- `/etc/passwd` - Informações de usuários
- `/etc/shadow` - Senhas criptografadas
- `/etc/group` - Informações de grupos
- `/etc/sudoers` - Configurações sudo

## 2.3 Comandos de Busca e Localização

### **find** - Busca Avançada
```bash
# Busca por nome
find /home -name "*.txt"
find . -name arquivo.txt
find / -iname "*.PDF"        # Case insensitive

# Busca por tipo
find /etc -type f            # Arquivos
find /etc -type d            # Diretórios
find /dev -type l            # Links simbólicos

# Busca por tamanho
find /var -size +100M        # Maiores que 100MB
find /tmp -size -1M          # Menores que 1MB
find . -size 500k            # Exatamente 500KB

# Busca por tempo
find /home -mtime -7         # Modificados há menos de 7 dias
find /tmp -atime +30         # Acessados há mais de 30 dias
find /var -ctime -1          # Changed há menos de 1 dia

# Busca por permissões
find /etc -perm 644          # Permissão exata
find /bin -perm -755         # Ao menos essas permissões
find / -perm /u+s            # SUID

# Busca por usuário/grupo
find /home -user joao        # Arquivos do usuário joão
find /var -group www-data    # Arquivos do grupo www-data

# Executar ações
find /tmp -name "*.log" -delete
find /home -name "*.bak" -exec rm {} \;
find . -type f -exec chmod 644 {} \;
```

### **locate** - Busca Rápida
```bash
locate arquivo.txt     # Busca por nome
locate -i arquivo      # Case insensitive
locate "*.pdf"         # Com wildcards
updatedb              # Atualiza base de dados (root)
```

### **which** - Localiza Comandos
```bash
which ls              # Mostra caminho do comando ls
which -a python       # Todos os caminhos do python
```

### **whereis** - Localiza Binários
```bash
whereis ls            # Binário, fonte e manual
whereis -b ls         # Apenas binário
whereis -m ls         # Apenas manual
```

### **grep** - Busca em Conteúdo
```bash
grep "texto" arquivo.txt      # Busca texto em arquivo
grep -r "erro" /var/log/      # Busca recursiva
grep -i "ERROR" arquivo       # Case insensitive
grep -n "palavra" arquivo     # Mostra número da linha
grep -v "excluir" arquivo     # Inverte busca
grep -c "contar" arquivo      # Conta ocorrências
grep -l "texto" *.txt         # Lista arquivos que contêm
grep -E "regex" arquivo       # Expressões regulares
```

## 2.4 Compressão e Descompressão

### **tar** - Tape Archive
```bash
# Criar arquivo tar
tar -cf arquivo.tar diretorio/
tar -czf arquivo.tar.gz dir/     # Com compressão gzip
tar -cjf arquivo.tar.bz2 dir/    # Com compressão bzip2

# Extrair
tar -xf arquivo.tar              # Extrai tar
tar -xzf arquivo.tar.gz          # Extrai tar.gz
tar -xjf arquivo.tar.bz2         # Extrai tar.bz2

# Listar conteúdo
tar -tf arquivo.tar              # Lista sem extrair
tar -tzf arquivo.tar.gz          # Lista tar.gz

# Verbose
tar -czfv backup.tar.gz /home/   # Modo verboso
```

### **gzip/gunzip** - Compressão gzip
```bash
gzip arquivo.txt          # Comprime para arquivo.txt.gz
gunzip arquivo.txt.gz     # Descomprime
gzip -d arquivo.txt.gz    # Descomprime (alternativa)
gzip -r diretorio/        # Comprime recursivamente
```

### **zip/unzip** - Compressão ZIP
```bash
zip -r arquivo.zip diretorio/    # Cria ZIP recursivo
unzip arquivo.zip                # Extrai ZIP
unzip -l arquivo.zip             # Lista conteúdo
zip -e arquivo.zip file.txt      # ZIP com senha
```

---
