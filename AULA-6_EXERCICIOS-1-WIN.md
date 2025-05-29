## **EXERCÍCIO 1: Navegação Básica e Exploração do Sistema**

### **Objetivo:** Familiarizar-se com a estrutura de diretórios do Windows e comandos básicos de navegação.

**Tarefas:**

1. **Exploração inicial:**
   ```cmd
   # Execute os comandos e anote os resultados:
   cd
   dir
   ver
   hostname
   whoami
   ```

2. **Navegação pela estrutura do sistema:**
   ```cmd
   # Navegue pelos diretórios principais e observe o conteúdo:
   cd C:\
   dir
   cd Windows
   dir
   cd System32
   dir /p
   cd ..
   cd ..
   ```

3. **Variáveis de ambiente:**
   ```cmd
   # Descubra os valores dessas variáveis no seu sistema:
   echo %USERNAME%
   echo %COMPUTERNAME%
   echo %USERPROFILE%
   echo %WINDIR%
   echo %TEMP%
   echo %PATH%
   ```

**Questões:**
- Qual é o nome do seu usuário e computador?
- Quantos arquivos existem em C:\Windows\System32?
- Qual é o caminho completo da sua pasta de usuário?
- Quais programas estão listados no PATH?

---

## **EXERCÍCIO 2: Criação e Organização de Estrutura de Pastas**

### **Objetivo:** Praticar criação, organização e navegação em estruturas de diretórios.

**Cenário:** Você precisa criar uma estrutura de pastas para organizar um projeto de desenvolvimento.

**Tarefas:**

1. **Criar estrutura básica:**
   ```cmd
   # Vá para sua pasta de documentos
   cd %USERPROFILE%\Documents
   
   # Crie a seguinte estrutura:
   md Projeto_Windows
   cd Projeto_Windows
   md Codigos
   md Documentacao
   md Testes
   md Backup
   ```

2. **Criar subpastas:**
   ```cmd
   # Dentro de Codigos:
   cd Codigos
   md Python
   md JavaScript
   md PowerShell
   cd ..
   
   # Dentro de Documentacao:
   cd Documentacao
   md Manuais
   md Diagramas
   md "Notas de Reuniao"
   cd ..
   ```

3. **Verificar estrutura criada:**
   ```cmd
   # Use tree para visualizar a estrutura completa
   tree
   tree /f
   ```

4. **Navegação avançada:**
   ```cmd
   # Pratique navegação usando pushd e popd
   pushd Codigos\Python
   # ... faça algum trabalho aqui
   popd
   
   # Navegue para diferentes unidades (se disponível)
   cd /d D:\
   cd /d C:\Projeto_Windows
   ```

**Desafios:**
- Crie uma pasta com nome que contenha espaços
- Navegue para a pasta mais profunda da estrutura em um comando
- Use pushd/popd para alternar rapidamente entre duas pastas

---

## **EXERCÍCIO 3: Manipulação de Arquivos - Básico**

### **Objetivo:** Aprender comandos básicos de criação, cópia, movimentação e exclusão de arquivos.

**Preparação:**
```cmd
cd %USERPROFILE%\Documents\Projeto_Windows
```

**Tarefas:**

1. **Criar arquivos de diferentes formas:**
   ```cmd
   # Arquivos vazios
   type nul > readme.txt
   echo. > config.ini
   copy nul dados.csv
   
   # Arquivos com conteúdo
   echo "Este é o arquivo principal do projeto" > readme.txt
   echo "versao=1.0" > config.ini
   echo "nome,idade,cidade" > dados.csv
   echo "João,25,São Paulo" >> dados.csv
   echo "Maria,30,Rio de Janeiro" >> dados.csv
   ```

2. **Copiar arquivos:**
   ```cmd
   # Cópia simples
   copy readme.txt readme_backup.txt
   
   # Cópia para subpasta
   copy config.ini Codigos\
   copy dados.csv Backup\
   
   # Cópia múltipla
   copy *.txt Documentacao\
   ```

3. **Mover e renomear:**
   ```cmd
   # Criar arquivo temporário
   echo "arquivo temporário" > temp.tmp
   
   # Mover arquivo
   move temp.tmp Testes\
   
   # Renomear arquivo
   ren readme_backup.txt readme_v2.txt
   ```

4. **Verificar operações:**
   ```cmd
   dir
   dir Codigos\
   dir Backup\
   dir Documentacao\
   dir Testes\
   ```

**Questões:**
- Quantos arquivos .txt existem na pasta principal?
- O que acontece se você tentar copiar um arquivo que já existe no destino?
- Como você copiaria todos os arquivos de uma pasta para outra?

---

## **EXERCÍCIO 4: Visualização e Busca de Conteúdo**

### **Objetivo:** Praticar comandos de visualização de arquivos e busca de informações.

**Preparação:** Use a estrutura criada no exercício anterior.

**Tarefas:**

1. **Visualizar conteúdo de arquivos:**
   ```cmd
   # Diferentes formas de visualizar
   type readme.txt
   type config.ini
   more dados.csv
   
   # Visualizar múltiplos arquivos
   type readme.txt config.ini
   ```

2. **Criar arquivo maior para testes:**
   ```cmd
   # Crie um arquivo com várias linhas
   echo "Linha 1 - Inicio do arquivo" > arquivo_grande.txt
   echo "Linha 2 - Conteúdo importante" >> arquivo_grande.txt
   echo "Linha 3 - Dados de teste" >> arquivo_grande.txt
   echo "Linha 4 - Informação relevante" >> arquivo_grande.txt
   echo "Linha 5 - ERRO: Falha na conexão" >> arquivo_grande.txt
   echo "Linha 6 - Continuação normal" >> arquivo_grande.txt
   echo "Linha 7 - Mais dados" >> arquivo_grande.txt
   echo "Linha 8 - ERRO: Timeout" >> arquivo_grande.txt
   echo "Linha 9 - Final do processamento" >> arquivo_grande.txt
   echo "Linha 10 - Fim do arquivo" >> arquivo_grande.txt
   ```

3. **Praticar busca em arquivos:**
   ```cmd
   # Busca simples
   find "ERRO" arquivo_grande.txt
   
   # Busca case insensitive
   find /i "erro" arquivo_grande.txt
   
   # Busca com número de linha
   find /n "dados" arquivo_grande.txt
   
   # Busca em múltiplos arquivos
   find "João" *.csv
   
   # Busca com findstr (regex)
   findstr "Linha [5-8]" arquivo_grande.txt
   findstr /i "erro timeout" arquivo_grande.txt
   ```

4. **PowerShell para visualização avançada:**
   ```cmd
   # Primeiras linhas
   powershell "Get-Content arquivo_grande.txt | Select-Object -First 3"
   
   # Últimas linhas
   powershell "Get-Content arquivo_grande.txt | Select-Object -Last 3"
   ```

**Desafios:**
- Encontre todas as linhas que contêm números
- Busque por uma palavra em todos os arquivos .txt da pasta
- Conte quantas vezes a palavra "dados" aparece nos arquivos

---

## **EXERCÍCIO 5: Atributos de Arquivos e Comparação**

### **Objetivo:** Trabalhar com atributos de arquivos e comparação entre arquivos.

**Tarefas:**

1. **Explorar atributos de arquivos:**
   ```cmd
   # Ver atributos atuais
   attrib readme.txt
   attrib config.ini
   
   # Modificar atributos
   attrib +h config.ini        # Tornar oculto
   dir                         # Verificar se ainda aparece
   dir /a                      # Ver arquivos ocultos
   attrib -h config.ini        # Remover atributo oculto
   
   # Somente leitura
   attrib +r readme.txt        # Tornar somente leitura
   echo "tentativa de alteração" >> readme.txt  # Tentar modificar
   attrib -r readme.txt        # Remover proteção
   ```

2. **Criar arquivos para comparação:**
   ```cmd
   # Criar dois arquivos similares
   echo "Linha 1" > arquivo1.txt
   echo "Linha 2" >> arquivo1.txt
   echo "Linha 3" >> arquivo1.txt
   
   echo "Linha 1" > arquivo2.txt
   echo "Linha 2 modificada" >> arquivo2.txt
   echo "Linha 3" >> arquivo2.txt
   echo "Linha 4 nova" >> arquivo2.txt
   ```

3. **Comparar arquivos:**
   ```cmd
   # Comparação de arquivos texto
   fc arquivo1.txt arquivo2.txt
   
   # Criar arquivo binário para teste
   copy /b readme.txt arquivo_binario.bin
   comp readme.txt arquivo_binario.bin
   ```

4. **Listar com diferentes opções:**
   ```cmd
   # Diferentes formas de listagem
   dir /o:n          # Ordenado por nome
   dir /o:s          # Ordenado por tamanho
   dir /o:d          # Ordenado por data
   dir /a:h          # Apenas arquivos ocultos
   dir /s            # Recursivo
   ```

**Questões:**
- Quais são os diferentes atributos que um arquivo pode ter?
- O que acontece quando você tenta modificar um arquivo somente leitura?
- Como você pode ver arquivos ocultos usando o comando dir?

---

## **EXERCÍCIO 6: Projeto Prático - Sistema de Logs**


**Cenário:** Você é responsável por organizar e analisar logs de um sistema. Precisa criar uma estrutura para armazenar diferentes tipos de logs e fazer análises básicas.

**Tarefas:**

1. **Preparação do ambiente:**
   ```cmd
   cd %USERPROFILE%\Documents
   md Sistema_Logs
   cd Sistema_Logs
   md Logs_Aplicacao
   md Logs_Sistema
   md Logs_Seguranca
   md Relatorios
   md Arquivo_Historico
   ```

2. **Simular logs de aplicação:**
   ```cmd
   cd Logs_Aplicacao
   echo "2024-01-15 10:30:15 INFO: Aplicacao iniciada" > app_20240115.log
   echo "2024-01-15 10:31:20 INFO: Usuario admin logou" >> app_20240115.log
   echo "2024-01-15 10:45:33 ERROR: Falha na conexão com BD" >> app_20240115.log
   echo "2024-01-15 10:46:01 INFO: Reconectando..." >> app_20240115.log
   echo "2024-01-15 10:46:15 INFO: Conexão reestabelecida" >> app_20240115.log
   echo "2024-01-15 11:00:00 WARNING: Memoria em 80%" >> app_20240115.log
   echo "2024-01-15 11:15:30 ERROR: Timeout na API externa" >> app_20240115.log
   echo "2024-01-15 11:30:45 INFO: Backup realizado com sucesso" >> app_20240115.log
   
   cd ..
   ```

3. **Simular logs de sistema:**
   ```cmd
   cd Logs_Sistema
   echo "2024-01-15 09:00:00 SYSTEM: Boot concluído" > system_20240115.log
   echo "2024-01-15 09:01:15 SYSTEM: Serviços iniciados" >> system_20240115.log
   echo "2024-01-15 10:30:00 SYSTEM: Alta utilização de CPU" >> system_20240115.log
   echo "2024-01-15 12:00:00 SYSTEM: Limpeza automática realizada" >> system_20240115.log
   echo "2024-01-15 14:30:20 ERROR: Falha no driver de rede" >> system_20240115.log
   echo "2024-01-15 14:31:00 SYSTEM: Driver reiniciado" >> system_20240115.log
   
   cd ..
   ```

4. **Análise dos logs:**
   ```cmd
   # Buscar todos os erros
   find /i "error" Logs_Aplicacao\*.log
   find /i "error" Logs_Sistema\*.log
   
   # Contar número de erros por arquivo
   find /c /i "error" Logs_Aplicacao\*.log
   find /c /i "error" Logs_Sistema\*.log
   
   # Buscar eventos específicos
   findstr /i "conexão falha timeout" Logs_Aplicacao\*.log
   
   # Extrair apenas informações importantes
   find "ERROR:" Logs_Aplicacao\*.log > Relatorios\erros_aplicacao.txt
   find "ERROR:" Logs_Sistema\*.log > Relatorios\erros_sistema.txt
   ```

5. **Organização e backup:**
   ```cmd
   # Copiar logs para arquivo histórico
   copy Logs_Aplicacao\*.log Arquivo_Historico\
   copy Logs_Sistema\*.log Arquivo_Historico\
   
   # Criar relatório consolidado
   echo "=== RELATÓRIO DIÁRIO DE LOGS ===" > Relatorios\relatorio_diario.txt
   echo "Data: %date%" >> Relatorios\relatorio_diario.txt
   echo. >> Relatorios\relatorio_diario.txt
   echo "RESUMO DE ERROS:" >> Relatorios\relatorio_diario.txt
   find /c /i "error" Logs_Aplicacao\*.log >> Relatorios\relatorio_diario.txt
   find /c /i "error" Logs_Sistema\*.log >> Relatorios\relatorio_diario.txt
   ```

6. **Verificação final:**
   ```cmd
   # Visualizar estrutura completa
   tree /f
   
   # Verificar conteúdo dos relatórios
   type Relatorios\relatorio_diario.txt
   type Relatorios\erros_aplicacao.txt
   ```

**Questões do Projeto:**
1. Quantos erros foram encontrados em cada tipo de log?
2. Qual foi o primeiro erro registrado no sistema?
3. Como você automatizaria esse processo de análise diária?
4. Que outras informações seriam úteis extrair dos logs?

---

## **EXERCÍCIO 7: Desafio Final - Limpeza e Organização**

### **Objetivo:** Demonstrar domínio completo dos comandos básicos em um cenário de limpeza e organização.

**Cenário:** Sua pasta de Downloads está desorganizada e você precisa categorizá-la usando apenas linha de comando.

**Preparação:** Crie uma simulação da pasta Downloads desorganizada:
```cmd
cd %USERPROFILE%\Documents
md Downloads_Simulacao
cd Downloads_Simulacao

# Criar arquivos diversos
echo "conteudo" > documento1.txt
echo "conteudo" > documento2.docx
echo "conteudo" > planilha1.xlsx
echo "conteudo" > apresentacao1.pptx
echo "conteudo" > foto1.jpg
echo "conteudo" > foto2.png
echo "conteudo" > musica1.mp3
echo "conteudo" > video1.mp4
echo "conteudo" > programa1.exe
echo "conteudo" > arquivo_compactado.zip
echo "conteudo" > script_antigo.bat
echo "conteudo" > backup_important.bak
```

**Tarefas:**

1. **Análise inicial:**
   ```cmd
   # Analise o conteúdo atual
   dir
   dir /o:s    # Por tamanho
   dir /o:d    # Por data
   ```

2. **Criar estrutura organizacional:**
   ```cmd
   # Crie pastas por categoria
   md Documentos
   md Planilhas
   md Apresentacoes
   md Imagens
   md Audio
   md Video
   md Programas
   md Arquivos_Compactados
   md Scripts
   md Backups
   md Outros
   ```

3. **Organizar arquivos por extensão:**
   ```cmd
   # Mover cada tipo para sua pasta apropriada
   move *.txt Documentos\
   move *.docx Documentos\
   move *.xlsx Planilhas\
   move *.pptx Apresentacoes\
   move *.jpg Imagens\
   move *.png Imagens\
   move *.mp3 Audio\
   move *.mp4 Video\
   move *.exe Programas\
   move *.zip Arquivos_Compactados\
   move *.bat Scripts\
   move *.bak Backups\
   ```

4. **Verificação e relatório:**
   ```cmd
   # Verificar organização
   tree
   
   # Criar relatório da organização
   echo "=== RELATÓRIO DE ORGANIZAÇÃO ===" > relatorio_organizacao.txt
   echo "Data da organização: %date%" >> relatorio_organizacao.txt
   echo. >> relatorio_organizacao.txt
   
   echo "DOCUMENTOS:" >> relatorio_organizacao.txt
   dir Documentos\ >> relatorio_organizacao.txt
   echo. >> relatorio_organizacao.txt
   
   echo "IMAGENS:" >> relatorio_organizacao.txt
   dir Imagens\ >> relatorio_organizacao.txt
   ```

5. **Limpeza final:**
   ```cmd
   # Remover arquivos desnecessários (se houver)
   # Verificar se sobrou algo na pasta raiz
   dir
   
   # Mover relatório para pasta apropriada
   move relatorio_organizacao.txt Documentos\
   ```

**Desafios Extras:**
- Renomeie todos os arquivos para seguir um padrão (ex: doc_001.txt, img_001.jpg)
- Crie um script .bat para automatizar esse processo
- Encontre arquivos duplicados (mesmo nome, extensões diferentes)

---
