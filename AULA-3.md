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
