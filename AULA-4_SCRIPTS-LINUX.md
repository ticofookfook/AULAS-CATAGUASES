### ✅  O que é Bash Script?

#### 📌 Conceito:
Um Bash Script é um conjunto de comandos escritos em um arquivo de texto que podem ser executados para automatizar tarefas no Linux.

#### 📁 Extensão do arquivo:
`.sh` (exemplo: `meuscript.sh`)

#### ▶️ Como executar um script:
```bash
chmod +x script.sh  # dá permissão de execução
./script.sh         # executa o script
# ou
bash script.sh      # executa sem precisar dar permissão
```

#### 👨‍🏫 Exemplo simples:
```bash
#!/bin/bash
echo "Olá, mundo!"
```

---

### ✅ – Variáveis

#### 🧾 Como declarar variáveis:
```bash
nome="Steven"
idade=25
# IMPORTANTE: Não usar espaços ao redor do =
```

#### 💬 Como usar variáveis:
```bash
echo "Meu nome é $nome e tenho $idade anos"
echo "Também posso usar ${nome} para delimitar melhor"
```

#### 🔤 Tipos de variáveis:
```bash
# String
texto="Olá mundo"
# Número
numero=42
# Array simples
frutas=("maçã" "banana" "laranja")
echo ${frutas[0]}  # primeiro elemento
```

---

### ✅  Entrada do usuário

#### 📥 Usando read:
```bash
#!/bin/bash
echo "Digite seu nome:"
read nome
echo "Olá, $nome!"
```

#### 🔧 Variações do read:
```bash
# Read com prompt na mesma linha
read -p "Digite sua idade: " idade

# Read silencioso (para senhas)
read -s -p "Digite sua senha: " senha

# Read com timeout (5 segundos)
read -t 5 -p "Digite algo (5s): " resposta
```

#### 📋 Parâmetros da linha de comando ($1, $2, etc.):
```bash
#!/bin/bash
echo "Nome do script: $0"
echo "Primeiro parâmetro: $1"
echo "Segundo parâmetro: $2"
echo "Todos os parâmetros: $@"
echo "Número de parâmetros: $#"

# Validação de parâmetros
if [ $# -eq 0 ]; then
    echo "Uso: $0 <nome> <idade>"
    exit 1
fi
```

---

### ✅ – Operadores de Comparação

#### 🔢 Operadores numéricos:
```bash
-eq    # igual (equal)
-ne    # não igual (not equal)
-gt    # maior que (greater than)
-ge    # maior ou igual (greater or equal)
-lt    # menor que (less than)
-le    # menor ou igual (less or equal)
```

#### 🔤 Operadores de string:
```bash
=      # igual
!=     # não igual
-z     # string vazia (zero length)
-n     # string não vazia (non-zero length)
```

#### 📁 Operadores de arquivo:
```bash
-f     # é um arquivo regular
-d     # é um diretório
-e     # existe (file exists)
-r     # é legível (readable)
-w     # é gravável (writable)
-x     # é executável
```

---

### ✅– Condicionais (if/else/elif)

#### 🧠 Sintaxe básica:
```bash
if [ condição ]; then
    # comandos
elif [ outra_condição ]; then
    # outros comandos
else
    # comandos alternativos
fi
```

#### ✔️ Exemplos práticos:
```bash
#!/bin/bash
# Exemplo 1: Comparação numérica
read -p "Digite sua idade: " idade
if [ $idade -ge 18 ]; then
    echo "Você é maior de idade"
elif [ $idade -gt 0 ]; then
    echo "Você é menor de idade"
else
    echo "Idade inválida"
fi

# Exemplo 2: Verificar se arquivo existe
if [ -f "arquivo.txt" ]; then
    echo "O arquivo existe!"
else
    echo "Arquivo não encontrado!"
fi

# Exemplo 3: Validação de parâmetro
if [ $# -ne 2 ]; then
    echo "Erro: Digite exatamente 2 parâmetros"
    exit 1
fi
```

#### 🔗 Operadores lógicos:
```bash
# E lógico (AND)
if [ $idade -ge 18 ] && [ $idade -le 65 ]; then
    echo "Idade de trabalho"
fi

# OU lógico (OR)
if [ $opcao -eq 1 ] || [ $opcao -eq 2 ]; then
    echo "Opção válida"
fi

# Negação (NOT)
if [ ! -f "arquivo.txt" ]; then
    echo "Arquivo não existe"
fi
```

---

### ✅ – Estrutura case

```bash
#!/bin/bash
echo "=== MENU PRINCIPAL ==="
echo "[1] Mostrar data"
echo "[2] Listar arquivos"
echo "[3] Verificar usuário"
echo "[4] Sair"
read -p "Escolha uma opção: " opcao

case $opcao in
    1)
        echo "Data atual: $(date)"
        ;;
    2)
        echo "Arquivos no diretório:"
        ls -la
        ;;
    3)
        read -p "Digite o nome do usuário: " user
        if id "$user" &>/dev/null; then
            echo "Usuário $user existe"
        else
            echo "Usuário não encontrado"
        fi
        ;;
    4)
        echo "Saindo..."
        exit 0
        ;;
    *)
        echo "Opção inválida!"
        ;;
esac
```

---

### ✅ – Loops (for e while)

#### 🔄 Loop for:
```bash
# For com lista de valores
for nome in João Maria Pedro; do
    echo "Olá, $nome!"
done

# For com range numérico
for i in {1..5}; do
    echo "Número: $i"
done

# For com arquivos
for arquivo in *.txt; do
    echo "Processando: $arquivo"
done

# For estilo C
for ((i=1; i<=10; i++)); do
    echo "Contador: $i"
done

# For com array
frutas=("maçã" "banana" "laranja")
for fruta in "${frutas[@]}"; do
    echo "Fruta: $fruta"
done
```

#### 🔁 Loop while:
```bash
# While básico
contador=1
while [ $contador -le 5 ]; do
    echo "numero: $contador"
    contador=$((contador + 1))
done

# While com validação de entrada
while true; do
    read -p "Digite 'sair' para terminar: " resposta
    if [ "$resposta" = "sair" ]; then
        break
    fi
    echo "Você digitou: $resposta"
done

# While lendo arquivo linha por linha
while IFS= read -r linha; do
    echo "Linha: $linha"
done < arquivo.txt
```

#### ⏹️ Controle de loop:
```bash
# break - sai do loop
# continue - pula para próxima iteração

for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue  # pula o 5
    fi
    if [ $i -eq 8 ]; then
        break     # para no 8
    fi
    echo $i
done
```

---

### ✅– Funções

```bash
#!/bin/bash

# Definindo uma função
saudacao() {
    local nome=$1  # parâmetro local
    echo "Olá, $nome! Como vai?"
}
```

---

### ✅ – Validação Avançada e Tratamento de Erros

```bash
#!/bin/bash

# Função para validar número
validar_numero() {
    if [[ $1 =~ ^[0-9]+$ ]]; then
        return 0  # verdadeiro
    else
        return 1  # falso
    fi
}

# Função para validar email (básico)
validar_email() {
    if [[ $1 =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
        return 0
    else
        return 1
    fi
}

# Script principal com validação
echo "=== CADASTRO DE USUÁRIO ==="

# Validar nome (não pode estar vazio)
while true; do
    read -p "Digite seu nome: " nome
    if [ -n "$nome" ]; then
        break
    else
        echo "Nome não pode estar vazio!"
    fi
done

# Validar idade (deve ser número)
while true; do
    read -p "Digite sua idade: " idade
    if validar_numero "$idade"; then
        if [ $idade -gt 0 ] && [ $idade -lt 120 ]; then
            break
        else
            echo "Idade deve estar entre 1 e 119 anos!"
        fi
    else
        echo "Digite apenas números!"
    fi
done

# Validar email
while true; do
    read -p "Digite seu email: " email
    if validar_email "$email"; then
        break
    else
        echo "Email inválido! Use o formato: usuario@dominio.com"
    fi
done

echo "Cadastro realizado com sucesso!"
echo "Nome: $nome"
echo "Idade: $idade anos"
echo "Email: $email"
```

---

## 🧪 Tarefas Práticas Progressivas

### 🧩 Nível 1 – Básico
1. **Script de apresentação**: Criar um script que imprime "Bem-vindo ao curso de Bash!" e mostra a data atual
2. **Variáveis pessoais**: Script que define nome, idade e cidade, e exibe uma frase completa
3. **Calculadora simples**: Script que recebe dois números como parâmetros ($1 e $2) e mostra a soma

### 🧩 Nível 2 – Interação
4. **Saudação personalizada**: Script que pergunta nome e idade, e responde adequadamente
5. **Verificador de arquivos**: Script que pergunta um nome de arquivo e verifica se existe
6. **Contador regressivo**: Script que recebe um número e faz contagem regressiva usando while

### 🧩 Nível 3 – Condicionais e Loops
7. **Classificador de idade**: Script que classifica a pessoa em: criança, adolescente, adulto ou idoso
8. **Menu interativo**: Menu com 5 opções usando case, incluindo uma calculadora básica

### 🧩 Nível 4 – Validação e Arrays
9. **Validador de dados**: Script que valida nome (não vazio), idade (número) e email (formato básico)
10. **Lista de compras**: Script que adiciona itens a um array e lista todos no final


---

## 📚 Conceitos Extras Importantes

### 🔧 Códigos de saída:
```bash
exit 0    # sucesso
exit 1    # erro genérico
exit 2    # uso incorreto do comando
```

### 📊 Redirecionamento:
```bash
comando > arquivo.txt     # redireciona saída para arquivo
comando >> arquivo.txt    # anexa saída ao arquivo
comando 2>&1             # redireciona erro para saída padrão
comando &>/dev/null      # descarta toda saída
```

