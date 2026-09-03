
QUINZENA 8

1 -No dia 18/07/2026 no momento da atulização dos pacotes o que eu fiz quando o terminal ficou preso:
Quando coloquei o sistema para baixar a atualização de 1.3 GB, a tela do meu terminal ficou totalmente ocupada mostrando o progresso do download, e eu não conseguia digitar mais nenhum comando ali.

A solução: Para não interromper o download e poder continuar trabalhando, eu abri uma nova aba no terminal usando o atalho Ctrl + Shift + T. Com isso, o Linux continuou baixando o Kernel e os arquivos em segundo plano de forma segura, enquanto eu ganhei uma tela livre para continuar minhas tarefas sem travar meu fluxo.


2 - Tentei instalar o htop e deu ruim (mas resolvi)
O que aconteceu:
Tentei instalar o monitor de sistema htop direto pelo terminal do Rocky Linux 9, mas o gerenciador de pacotes retornou o erro "Impossível de encontrar uma correspondência: htop". Isso aconteceu porque o htop não vem na lista de aplicativos básicos de fábrica do sistema.
O Rocky Linux de fábrica só vem com os repositórios básicos da Red Hat. Como o htop é uma ferramenta extra, o gerenciador de pacotes simplesmente não sabia onde achar o instalador.

Como resolvi o problema:
Descobri que para esse tipo de programa rodar em servidor corporativo, a gente precisa ativar uma "loja de pacotes" da comunidade chamada EPEL (Extra Packages for Enterprise Linux).

Para resolver isso, entendi que precisava ativar um repositório extra de programas no Linux. Usei o operador && para juntar dois comandos em uma única linha e disparar a solução:

sudo yum install epel-release && sudo yum install htop

Com essa instrução, eu primeiro instalei o repositório EPEL (que funciona como uma loja expandida de ferramentas para servidores corporativos) e, logo em seguida, o sistema passou a reconhecer e instalar o htop com sucesso a partir dessa nova fonte.

TAREFA 2

Não consegui confimar e verificar a versão instalada do PostegreSQL em um primeiro momento porque, quando instalO o PostgreSQL através do repositório oficial (PGDG), os binários executáveis (postgres, psql, etc.) não são colocados na pasta padrão de comandos do sistema (/usr/bin/).

Eles ficam salvos em um diretório específico da versão, como /usr/pgsql-17/bin/
Solução: Executar o comando indicando o caminho completo
Para testar qual versão está instalada, chame o executável direto da pasta do repositório oficial.
2 opção
Como o terminal não busca comandos nessa pasta por padrão, ele diz que o comando não foi encontrado e sugere instalar o pacote padrão do Rocky Linux (postgresql-server), o que não devo fazer.

Como resolver?
Para que o terminal reconheça os comandos psql e postgres em qualquer lugar, devo criar links simbólicos executando estes dois comandos:

ln -s /usr/pgsql-17/bin/psql /usr/bin/psql
ln -s /usr/pgsql-17/bin/postgres /usr/bin/postgres

 TAREFA 3

Colocar o PostgreSQL em funcionamento.

Problema encontrado:
Nenhum problema relevante.

O que aprendi:
Aprendi a iniciar, parar, reiniciar e verificar o status do serviço, além de acessar o psql e criar meu primeiro banco de dados.

Tarefa 4
A). Onde estão localizados os itens do PostgreSQL?

Os principais itens do PostgreSQL estão localizados em diferentes diretórios do sistema. Os programas executáveis (binários) ficam em /usr/pgsql-17/bin, enquanto os arquivos de dados e configuração ficam em /var/lib/pgsql/17/data.

B). Onde ficam armazenados os arquivos de configuração?

Os arquivos de configuração estão armazenados no diretório /var/lib/pgsql/17/data. Os principais são postgresql.conf, responsável pelas configurações gerais do servidor, e pg_hba.conf, responsável pelo controle de acesso dos usuários.

C). Onde ficam armazenados os bancos de dados?

Os bancos de dados ficam armazenados dentro do diretório /var/lib/pgsql/17/data/base, onde o PostgreSQL grava os arquivos físicos de cada banco criado.

D). Onde ficam os arquivos de log?

Os logs do PostgreSQL podem ser consultados pelo systemd utilizando o comando journalctl -u postgresql-17, que exibe os registros de funcionamento, inicialização e possíveis erros do serviço.

E). Qual serviço do systemd foi criado?

O serviço criado pelo systemd é o postgresql-17.service, responsável por iniciar, parar, reiniciar e verificar o status do PostgreSQL.

F). Qual usuário é responsável pela execução do PostgreSQL?

O PostgreSQL é executado pelo usuário postgres, criado automaticamente durante a instalação para administrar o banco de dados com segurança.

G). Quais diretórios você considera mais importantes para um administrador conhecer?

Na minha opinião, os diretórios mais importantes são /usr/pgsql-17/bin, onde ficam os programas do PostgreSQL; /var/lib/pgsql/17/data, que é o diretório principal (PGDATA); /var/lib/pgsql/17/data/base, onde são armazenados os bancos de dados; e os arquivos postgresql.conf e pg_hba.conf, responsáveis pelas configurações e pelo controle de acesso. Também considero importante saber consultar os logs do serviço com o comando journalctl -u postgresql-17, pois eles ajudam a identificar problemas e acompanhar o funcionamento do PostgreSQL.

Durante as semas aprendi como preparar um servidor Rocky Linux para receber o PostgreSQL utilizando o repositório oficial PGDG. Também aprendi a instalar o banco de dados, verificar se a instalação foi concluída corretamente, iniciar e configurar o serviço para iniciar automaticamente com o sistema, acessar o PostgreSQL pelo psql e criar meu primeiro banco de dados. A atividade que mais exigiu atenção foi conhecer a estrutura da instalação, principalmente localizar o diretório de dados e entender a função dos arquivos de configuração. O maior problema encontrado foi a dificuldade para identificar onde o PostgreSQL armazenava seus dados, gravar os comandos e as muitas informações.  


QUIZENA 9 DATA 31/08/2026
# Atividade: Atividade 1 - Retomando o PostgreSQL

### O que precisava fazer:
Verificar se a instalação do PostgreSQL realizada anteriormente continuava funcionando.

### O que pesquisei:
Pesquisei como verificar pacotes instalados, versão do PostgreSQL, status do serviço e como utilizar o `psql`.

### Dificuldades encontradas:
* Tive dificuldade para lembrar o comando que verifica se o PostgreSQL está instalado, precisando recorrer a exercícios anteriores para relembrar o comando `rpm -qa | grep postgresql`.
* Tive dificuldade inicialmente para diferenciar os comandos do ambiente Linux dos comandos executados dentro do `psql`.
* Enfrentei problemas com a formatação no GitHub por não estar envolvendo os blocos de código com três crases (```) no início e no final.

### Como resolvi:
Revisei a documentação e os comandos anteriores, compreendendo que `systemctl` e `rpm` são utilitários do terminal Linux, enquanto metacomandos como `\l`, `\c` e `\q` pertencem exclusivamente ao ambiente do `psql`.

### O que aprendi:
Aprendi novamente a verificar a instalação do pacote, consultar a versão, verificar o status do serviço no sistema, acessar o `psql`, listar os bancos de dados, conectar ao banco `laboratorio` e identificar a role/usuário em execução na sessão.

### Resultado:
Consegui acessar o PostgreSQL com sucesso, localizar o banco de dados `laboratorio`, realizar a conexão e confirmar a sessão com o usuário `postgres`.


Data: 02/09/2026

# Atividade: Atividade 2 - Funções não são usuários Linux

O que precisava fazer:
Investigar a diferença entre usuários do Linux e roles do PostgreSQL.

O que pesquisei:
Pesquisei sobre roles, login, superusuário e o comando \du.

O que fiz:
Utilizei o comando id postgres para verificar o usuário postgres no Linux.
Depois entrei no PostgreSQL e utilizei \du e consultas na pg_roles para
identificar as roles existentes, quais podem fazer login e quais possuem
atribuições administrativas.

O que aprendi:
Aprendi que o usuário postgres do Linux e a role postgres do PostgreSQL
são identidades diferentes, apesar de possuírem o mesmo nome. Também
aprendi que uma role pode controlar o acesso ao PostgreSQL sem precisar
existir um usuário Linux correspondente.
O usuário postgres pertence ao Rocky Linux e é utilizado pelo sistema operacional para executar o PostgreSQL. A role postgres pertence ao PostgreSQL e controla uma identidade e seus privilégios dentro do banco. Eles possuem o mesmo nome, mas são coisas diferentes.

Dificuldade:
Minha principal dificuldade foi entender a diferença entre o usuário
do sistema operacional e a role do banco de dados.

Resultado:
Consegui identificar as roles existentes, verificar quais podem fazer
login e identificar a role com privilégios administrativos.
