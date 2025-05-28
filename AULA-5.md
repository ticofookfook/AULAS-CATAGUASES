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


