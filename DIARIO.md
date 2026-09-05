
# QUINZENA 8

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


# QUINZENA 9
 DATA 31/08/2026
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



Data: 03/09/2026
# Atividade 3 - Criando uma role e um database
Para relembrar o conceito de role, decidi começar assistindo a um vídeo explicativo no YouTube. https://www.youtube.com/playlist?list=PLucm8g_ezqNoAkYKXN_zWupyH6hQCAwxY
Nesta atividade criei a role appuser no PostgreSQL, habilitei o login e configurei uma senha, mantendo a role sem privilégios de superusuário. Em seguida, criei o database appdb e defini appuser como seu proprietário.

Depois verifiquei as propriedades da role e do database e realizei uma tentativa de conexão utilizando appuser. A primeira tentativa, utilizando psql -U appuser -d appdb, apresentou erro de autenticação do tipo peer.

###  Problema de Autenticação na Conexão Local (`peer`)

**O que aconteceu:**
Na primeira tentativa de conexão, ocorreu uma falha por conta do método de autenticação padrão do PostgreSQL para conexões.

**Comando executado:**
psql -U appuser -d appdb

**Mensagem de erro:**
psql: erro: a conexão com o servidor no soquete "/run/postgresql/.s.PGSQL.5432" falhou: FATAL: A autenticação do tipo peer falhou para o usuário "appuser"

**Causa do erro:**
A conexão foi solicitada via soquete local, onde a regra `peer` exige correspondência exata entre o usuário do sistema Linux e a role do PostgreSQL:

* **Usuário do Linux (OS):** `postgres`
* **Usuário do PostgreSQL:** `appuser`

Como as contas do sistema operacional e do banco de dados são diferentes, o PostgreSQL bloqueou a autenticação.

**Solução:**
Adicionei o parâmetro `-h localhost` para forçar a conexão via rede TCP/IP, alterando o método de autenticação para verificação por senha.
psql -U appuser -d appdb -h localhost

O que aprendi: aprendi a criar e configurar uma role no PostgreSQL, habilitar login, definir senha, criar um database, definir seu proprietário e verificar as propriedades desses recursos. Também aprendi que o método de autenticação utilizado pelo PostgreSQL pode influenciar o resultado de uma tentativa de conexão e que os erros devem ser investigados antes de alterar configurações.


## Data

05/09/2026

## Atividade

Atividade 4 — Quem pode entrar? Conhecendo o `pg_hba.conf`

## O que precisava fazer

Entender como o PostgreSQL decide se uma conexão será aceita ou recusada e conhecer o funcionamento do arquivo `pg_hba.conf`, que controla as regras de autenticação dos clientes.

Precisava localizar o arquivo utilizado pela instalação do PostgreSQL, confirmar sua localização pelo próprio PostgreSQL e analisar as regras existentes sem fazer alterações.

Também precisava entender os significados de `local`, `host`, banco de dados, usuário, endereço de origem e método de autenticação, além dos métodos `peer`, `scram-sha-256` e `trust`, e compreender a ordem de avaliação das regras.

## O que pesquisei

Pesquisei sobre o arquivo `pg_hba.conf` e entendi que ele é responsável por definir regras de autenticação e acesso ao PostgreSQL.

Também pesquisei a diferença entre conexões `local` e `host`, os métodos de autenticação e como o PostgreSQL escolhe qual regra utilizar.

Para descobrir o arquivo utilizado pelo PostgreSQL, utilizei:

`SHOW hba_file;`

Depois saí do `psql` com:

`\q`

E consultei o conteúdo do arquivo encontrado com:

`cat /var/lib/pgsql/17/data/pg_hba.conf`

## O que encontrei

O arquivo utilizado está localizado em:

`/var/lib/pgsql/17/data/pg_hba.conf`

As principais regras encontradas foram:

`local   all   all   peer`

`host    all   all   127.0.0.1/32   scram-sha-256`

`host    all   all   ::1/128        scram-sha-256`

Também encontrei regras específicas para conexões de replicação utilizando `peer` e `scram-sha-256`.

## O que entendi sobre as regras

### local

Indica uma conexão feita através do socket Unix local do sistema.

A regra:

`local   all   all   peer`

significa que qualquer banco e qualquer role podem utilizar uma conexão local, mas a autenticação será feita pelo método `peer`.

### host

Indica uma conexão realizada através de TCP/IP.

As regras para:

`127.0.0.1/32`

e

`::1/128`

permitem conexões de localhost utilizando IPv4 e IPv6, respectivamente.

Nessas conexões o método utilizado é:

`scram-sha-256`

que realiza a autenticação através de senha.

### DATABASE

Indica para qual banco de dados a regra se aplica.

No arquivo analisado aparece `all`, indicando todos os bancos de dados normais.

### USER

Indica qual role PostgreSQL pode utilizar a regra.

Também aparece `all`, permitindo qualquer role.

### ADDRESS

Indica de qual endereço a conexão pode ser realizada.

`127.0.0.1/32` representa o localhost em IPv4.

`::1/128` representa o localhost em IPv6.

### METHOD

Define como o usuário será autenticado.

No meu arquivo encontrei principalmente:

`peer` — utiliza a identidade do usuário do sistema operacional para realizar a autenticação em conexões locais.

`scram-sha-256` — utiliza autenticação por senha através do mecanismo SCRAM.

Também encontrei `trust` descrito nos comentários do arquivo. Esse método permite a conexão sem solicitar autenticação por senha e, por isso, deve ser utilizado com bastante cuidado.

## Dificuldade encontrada

A principal dificuldade foi entender por que anteriormente a conexão com o usuário `appuser` funcionou de uma maneira quando utilizei:

`psql -U appuser -d appdb`

e de outra maneira quando utilizei:

`psql -U appuser -d appdb -h localhost`

Depois de analisar o `pg_hba.conf`, consegui entender o motivo.

Quando não utilizo `-h`, a conexão é local e utiliza a regra:

`local   all   all   peer`

Eu estava conectado no Linux como usuário `postgres`, mas estava tentando acessar o PostgreSQL como `appuser`. Como os nomes não correspondiam, a autenticação `peer` foi recusada.

Quando utilizei `-h localhost`, a conexão passou a ser TCP/IP e utilizou uma das regras `host`, com autenticação `scram-sha-256`. Nesse caso, o PostgreSQL solicitou a senha da role `appuser`.

## Como resolvi

Analisei o erro em vez de simplesmente alterar o arquivo de configuração.

Comparei os dois comandos de conexão e depois consultei o `pg_hba.conf` para descobrir qual regra estava sendo utilizada em cada situação.

Isso permitiu entender que o comportamento diferente não era um erro aleatório, mas consequência do tipo de conexão e do método de autenticação definido no arquivo.

## O que aprendi

Aprendi que o `pg_hba.conf` é um dos principais arquivos responsáveis pelo controle de autenticação do PostgreSQL.

Aprendi que `local` representa conexões pelo socket Unix e `host` representa conexões TCP/IP.

Também entendi que `peer` relaciona a identidade do usuário Linux com a role PostgreSQL, enquanto `scram-sha-256` utiliza senha.

Aprendi ainda que o PostgreSQL analisa as regras do `pg_hba.conf` de cima para baixo e utiliza a primeira regra que corresponde à conexão. Se a autenticação dessa regra falhar, ele não continua procurando outra regra que possa funcionar.

A atividade também ajudou a entender na prática o erro de autenticação que ocorreu com o `appuser`.

## Conclusão

Consegui localizar o `pg_hba.conf`, confirmar o arquivo utilizado pelo PostgreSQL e interpretar suas principais regras.

Também consegui relacionar as regras do arquivo com os testes de conexão realizados anteriormente, entendendo por que uma conexão utilizando `peer` foi recusada e por que a conexão utilizando `localhost` e `scram-sha-256` funcionou.

Não alterei o arquivo de configuração nesta atividade, apenas analisei as regras existentes.

