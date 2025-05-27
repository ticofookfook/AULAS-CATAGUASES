GREP - Busca e Filtros

### Conceitos Básicos
```bash
# Sintaxe básica
grep "padrão" arquivo

# Busca case-insensitive
grep -i "root" /var/log/auth.log

# Busca recursiva em diretórios
grep -r "password" /etc/

# Mostrar número da linha
grep -n "error" /var/log/syslog

# Inverter busca (mostrar linhas que NÃO contêm o padrão)
grep -v "INFO" application.log
```

### Regex Essenciais para Pentest
```bash
# Buscar IPs
grep -E "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" access.log

# Buscar emails
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" logs.txt

# Buscar URLs
grep -E "https?://[^\s]+" access.log

# Buscar tentativas de login
grep -i "failed\|error\|invalid" /var/log/auth.log
```

### Contexto e Formatação
```bash
# Mostrar linhas antes e depois do match
grep -A 3 -B 3 "error" log.txt

# Contar ocorrências
grep -c "404" access.log

```

---

## 📊 SORT e UNIQ - Ordenação e Duplicatas

### Sort - Ordenação
```bash
# Ordenação básica
sort arquivo.txt

# Ordenação numérica
sort -n numeros.txt

# Ordenação reversa
sort -r arquivo.txt

```

### Uniq - Remoção de Duplicatas
```bash
# Remover duplicatas (arquivo deve estar ordenado)
sort arquivo.txt | uniq

# Contar ocorrências
sort access.log | uniq -c

# Mostrar apenas duplicatas
sort arquivo.txt | uniq -d

# Mostrar apenas únicos (não duplicados)
sort arquivo.txt | uniq -u
```

### Aplicações em Segurança
```bash
# Top IPs atacantes
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10

# URLs mais acessadas
awk '{print $7}' access.log | sort | uniq -c | sort -nr | head -20

# User agents únicos
awk -F'"' '{print $6}' access.log | sort | uniq
```

---

## ✂️ CUT e TR - Extração e Transformação

### Cut - Extrair Colunas
```bash
#extrair campos separados por delimitador
cut -d: -f1,3 /etc/passwd

# Extrair múltiplos campos
cut -d' ' -f1,4,7 access.log

```

### TR - Transformar Caracteres
```bash
# Converter maiúsculas para minúsculas
tr '[:upper:]' '[:lower:]' < arquivo.txt

# Remover caracteres específicos
tr -d '[:digit:]' < arquivo.txt

# Substituir caracteres
tr ' ' '_' < arquivo.txt

# Remover quebras de linha
tr -d '\n' < arquivo.txt


```

---

## 🔧 AULA 2: SED - Editor de Stream

### Substituições Básicas
```bash
# Substituir primeira ocorrência
sed 's/antigo/novo/' arquivo.txt

# Substituir todas as ocorrências
sed 's/antigo/novo/g' arquivo.txt

# Substituir apenas na linha 5
sed '5s/antigo/novo/' arquivo.txt

# Substituir em range de linhas
sed '2,5s/antigo/novo/g' arquivo.txt
```

### Operações de Linha
```bash
# Deletar linhas
sed '3d' arquivo.txt          # Deletar linha 3
sed '2,4d' arquivo.txt        # Deletar linhas 2-4
sed '/padrão/d' arquivo.txt   # Deletar linhas com padrão

# Inserir linhas
sed '2i\Nova linha' arquivo.txt    # Inserir antes da linha 2
sed '2a\Nova linha' arquivo.txt    # Inserir depois da linha 2

# Imprimir linhas específicas
sed -n '2,5p' arquivo.txt     # Imprimir apenas linhas 2-5
```


---

## 📈 AWK - Processamento 

### Sintaxe Básica
```bash
# Estrutura básica
awk 'padrão { ação }' arquivo

# Imprimir colunas específicas
awk '{print $1, $3}' arquivo.txt

# Usar delimitador específico
awk -F: '{print $1}' /etc/passwd

```


### Análise de Logs com AWK
```bash
# Análise de Apache Access Log
# Formato: IP - - [timestamp] "method url protocol" status size "referer" "user-agent"

# Top IPs por requests
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head

# Análise de códigos de status
awk '{print $9}' access.log | sort | uniq -c | sort -nr


# Tamanho total de dados transferidos
awk '{sum += $10} END {print "Total bytes:", sum}' access.log

```

---

## 🔗 AULA 3: XARGS - Execução em Lote

### Conceitos Básicos
```bash
# Sintaxe básica
comando1 | xargs comando2

# Executar rm com lista de arquivos
ls *.tmp | xargs rm

# Usar placeholder
ls *.txt | xargs -I {} cp {} backup/{}
```

### Aplicações em Segurança
```bash
# Buscar strings em múltiplos arquivos
find /var/log -name "*.log" | xargs grep -l "attack"

# Verificar permissões de arquivos
find /etc -name "*.conf" | xargs ls -l

# Processar lista de IPs
echo 192.168.29.{1..10} | tr ' ' '\n' | xargs -I {} ping -c 1 {}

# Verificar serviços em múltiplas portas
echo "80 443 22 21" | tr ' ' '\n' | xargs -I {} nmap -p {} target.com
```

### Paralelização
```bash
# Executar comandos em paralelo
ls *.log | xargs -P 4 -I {} wc -l {}

```

---

## 🔍 FIND - Busca Avançada

### Busca por Critérios
```bash
# Busca básica por nome
find /var/log -name "*.log"

# Busca case-insensitive
find /etc -iname "*PASS*"

# Busca por tipo
find /home -type f -name "*.txt"    # Arquivos
find /var -type d -name "log*"      # Diretórios

# Busca por tamanho
find /var/log -size +100M           # Maior que 100MB
find /tmp -size -1k                 # Menor que 1KB
```

### Busca por Tempo e Permissões
```bash
# Arquivos modificados recentemente
find /var/log -mtime -1             # Últimas 24h
find /home -atime +30               # Não acessados há 30+ dias

# Busca por permissões
find /etc -perm 777                 # Permissões exatas
find /var -perm -4000               # SUID files
```

### Combinando com Outras Ferramentas
```bash
# Buscar e executar ação
find /var/log -name "*.log" -exec grep -l "error" {} \;

# Pipeline com xargs
find /etc -name "*.conf" | xargs grep -i "password"

# Buscar arquivos suspeitos
find /var/www -name "*.php" -exec grep -l "eval\|base64_decode" {} \;

# Arquivos com SUID/SGID
find / -perm -4000 -o -perm -2000 2>/dev/null | sort
```

---
## 🎯 DESAFIOS PRÁTICOS

### Desafio 1: Análise de Log de Acesso (Fácil)
**Arquivo**: `access.log` (formato Apache)
```
192.168.1.100 - - [25/Dec/2023:10:00:01 +0000] "GET /index.html HTTP/1.1" 200 1234
192.168.1.101 - - [25/Dec/2023:10:00:02 +0000] "POST /login.php HTTP/1.1" 401 0
192.168.1.100 - - [25/Dec/2023:10:00:03 +0000] "GET /admin.php HTTP/1.1" 403 567
```

**Tarefas**:
1. Extrair apenas os IPs únicos
2. Contar quantas vezes cada IP aparece
3. Listar apenas requisições com erro (4xx, 5xx)
4. Mostrar as 5 páginas mais acessadas

**Solução esperada**:
```bash
# 1. IPs únicos
awk '{print $1}' access.log | sort | uniq

# 2. Contagem por IP
awk '{print $1}' access.log | sort | uniq -c | sort -nr

# 3. Requisições com erro
awk '$9 ~ /^[45]/' access.log

# 4. Top 5 páginas
awk '{print $7}' access.log | sort | uniq -c | sort -nr | head -5
```

---

### Desafio 2: Limpeza de Lista de Senhas (Fácil)
**Arquivo**: `passwords.txt` (lista de senhas com duplicatas e espaços)
```
admin123
password  
ADMIN123
password
 secret 
Password
admin123
```

**Tarefas**:
1. Remover duplicatas (case-insensitive)
2. Remover espaços em branco no início e fim
3. Converter tudo para minúsculas
4. Mostrar apenas senhas com mais de 6 caracteres

**Solução esperada**:
```bash
# Limpar e filtrar senhas
cat passwords.txt | sed 's/^[ \t]*//;s/[ \t]*$//' | tr '[:upper:]' '[:lower:]' | sort | uniq | awk 'length($0) > 6'
```

---

### Desafio 3: Análise de Usuários do Sistema (Médio)
**Arquivo**: `/etc/passwd`

**Tarefas**:
1. Listar usuários com UID >= 1000 (usuários normais)
2. Mostrar apenas username e shell padrão
3. Contar quantos usuários usam bash vs outras shells


**Solução esperada**:
```bash
# 1. Usuários normais (UID >= 1000)
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd

# 2. Username e shell
awk -F: '$3 >= 1000 {print $1, $7}' /etc/passwd

# 3. Contagem por shell
awk -F: '{print $7}' /etc/passwd | sort | uniq -c | sort -nr


```

---

### Desafio 4: Busca de Configurações Sensíveis (Médio)
**Tarefa**: Encontrar arquivos de configuração que podem conter informações sensíveis

**Objetivos**:
1. Buscar todos os arquivos `.conf` e `.cfg` em `/etc`
2. Procurar por linhas contendo "password", "secret", "key" (case-insensitive)


**Solução esperada**:
```bash
find /etc -name "*.conf" -o -name "*.cfg" | xargs grep -i -E "(password|secret|key)" | grep -v -E "^\s*[#;]" | grep -v "^#"
```

---

### Dicas
- **Sempre redirecione stderr**: `comando 2>/dev/null`
- **Use aliases para comandos complexos**: `alias logips="awk '{print \$1}' | sort | uniq -c | sort -nr"`
- **Combine ferramentas em scripts**: Crie scripts reutilizáveis para análises comuns
- **Mantenha one-liners úteis**: Documente comandos que funcionam bem


## ✅ Checklist de Avaliação

### Aluno deve demonstrar capacidade de:
- [ ] Usar grep com regex para buscar padrões em logs
- [ ] Extrair e processar dados com awk
- [ ] Manipular texto com sed
- [ ] Combinar ferramentas em pipelines eficientes
- [ ] Usar xargs para processamento em lote
- [ ] Otimizar comandos para performance
