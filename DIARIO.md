
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









Data: 12/09/2026

## Atividade 5


Atividade 5 — Fazendo uma conexão funcionar

O que precisava fazer
O objetivo era fazer com que a role PostgreSQL `appuser` conseguisse realizar uma conexão autenticada com o banco `appdb`, utilizando autenticação por senha. Também precisava investigar uma tentativa de conexão recusada, identificar a regra responsável no `pg_hba.conf`, realizar a alteração necessária, aplicar a configuração e testar novamente.

O que pesquisei
Pesquisei sobre autenticação por senha no PostgreSQL, o funcionamento do `pg_hba.conf`, o método `scram-sha-256`, a ordem das regras e a necessidade de recarregar a configuração após uma alteração.

O que fiz
Primeiro analisei as regras ativas do `pg_hba.conf` utilizando:

```bash
grep -vE '^[[:space:]]*#|^[[:space:]]*$' /var/lib/pgsql/17/data/pg_hba.conf

```

Identifiquei que a primeira regra para conexões locais era:

```text
local all all peer

```

Ao tentar conectar com:

```bash
psql -U appuser -d appdb

```

A conexão foi recusada porque o método `peer` verifica o usuário do sistema operacional. Eu estava utilizando o usuário Linux `postgres`, mas tentando acessar o PostgreSQL como `appuser`.

Para resolver o problema, fiz uma cópia de segurança do arquivo `pg_hba.conf` e adicionei antes da regra genérica uma regra específica:

```text
local appdb appuser scram-sha-256

```

Essa regra determina que o usuário `appuser`, ao acessar o banco `appdb` por uma conexão local, deverá ser autenticado utilizando senha.

Depois da alteração, recarreguei a configuração do PostgreSQL utilizando:

```sql
SELECT pg_reload_conf();

```

Também consultei as regras através da visão `pg_hba_file_rules` para verificar se a nova configuração foi reconhecida pelo PostgreSQL.

Teste realizado
Testei novamente:

```bash
psql -U appuser -d appdb

```

Dessa vez, o PostgreSQL solicitou a senha do `appuser` e permitiu a conexão.

Depois confirmei a identidade da sessão com:

```sql
SELECT current_user;

```

E confirmei o banco com:

```sql
SELECT current_database();

```

O resultado confirmou que a conexão estava sendo realizada como `appuser` no banco `appdb`.

O que aprendi
Aprendi que o `pg_hba.conf` é analisado de cima para baixo e que a primeira regra que corresponde à conexão é utilizada.

También entendi na prática a diferença entre `peer` e `scram-sha-256`. O `peer` utiliza a identidade do usuário Linux em conexões locais, enquanto o `scram-sha-256` utiliza autenticação por senha.

Aprendi ainda que uma alteração no `pg_hba.conf` não precisa necessariamente de um restart do PostgreSQL. É possível realizar um reload da configuração para que as novas regras sejam carregadas.

A principal aprendizagem foi perceber que uma falha de conexão deve ser investigada pela combinação de usuário, banco, tipo de conexão e regra correspondente no `pg_hba.conf`, em vez de simplesmente alterar configurações sem entender o motivo do erro.

Conclusão
Consegui configurar uma regra específica para que `appuser` acessasse `appdb` através de uma conexão local utilizando autenticação por senha com `scram-sha-256`. A conexão que anteriormente era recusada pelo método `peer` passou a funcionar após a alteração e o recarregamento da configuração. Não utilizei o método `trust`.

```

```
Atividade 6 — Autenticação não é permissão

Data
14/09/2026

Atividade
Investigar a diferença entre autenticação e autorização no PostgreSQL, verificando os privilégios da role appuser no banco appdb.

O que precisava ser feito
Verificar o que a role appuser pode fazer depois de conseguir se conectar ao banco appdb, além de compreender os conceitos de role, privilégios, GRANT, REVOKE, proprietário e privilégios de banco de dados.

O que foi pesquisado e praticado
Foi realizada uma conexão com o banco utilizando `psql -U appuser -d appdb`. O comando `SELECT current_user;` confirmou que a sessão estava utilizando a role `appuser`.

Também foram utilizados os comandos `\l appdb` e `\l+ appdb`, que mostraram que o banco `appdb` pertence à role `appuser`.

A role foi analisada através de `SELECT ... FROM pg_roles` e `\du appuser`. Foi verificado que `appuser` pode fazer login, mas não é superusuária e não possui os atributos `CREATEDB` e `CREATEROLE`.

Também foram verificados os privilégios efetivos sobre o banco utilizando `has_database_privilege()`. O resultado mostrou que `appuser` possui os privilégios `CONNECT`, `CREATE` e `TEMPORARY` no banco `appdb`.

Dificuldades encontradas
Durante uma consulta foi colocado uma vírgula após `rolcanlogin`, antes do `FROM`, causando um erro de sintaxe. O erro foi analisado e a consulta foi corrigida removendo a vírgula.

O que aprendi
Aprendi que autenticação e autorização são conceitos diferentes. Autenticação determina se a conexão pode ser validada e estabelecida, enquanto autorização determina quais ações a role pode realizar depois de conectada.

Também aprendi que o `pg_hba.conf` participa do processo de autenticação, enquanto os privilégios, a propriedade dos objetos e os atributos das roles estão relacionados à autorização.

Aprendi ainda que `OWNER` significa proprietário, `GRANT` concede privilégios e `REVOKE` remove privilégios concedidos. Também entendi que conseguir conectar ao banco não significa automaticamente possuir todos os privilégios sobre todos os objetos existentes nele.


# QUINZENA 10





## Atividade 1 — O que existe dentro de um database?
**Data:** 16/09/2026

### O que precisava fazer
Acessar o database `appdb` e investigar sua estrutura interna, verificando quais *schemas* existem, identificar o *schema* `public`, verificar os objetos existentes, listar tabelas, entender como descobrir o *schema* de uma tabela, verificar quais tabelas pertencem ao usuário `appuser` e identificar o *schema* utilizado por padrão na sessão.
Conforme orientação da atividade, não foi criada nenhuma tabela.

### O que pesquisei
Pesquisei os conceitos de:
- PostgreSQL database e *schema*;
- *schema* `public`;
- Listagem de *schemas*;
- Listagem de tabelas;
- `search_path`;
- Diferença entre database e *schema*.

Também utilizei comandos do `psql` para observar a estrutura do `appdb`.

### Procedimentos realizados
Primeiro confirmei a conexão com:

```sql
\conninfo

```

A conexão estava sendo realizada no database `appdb`, utilizando o usuário `appuser`.
Depois listei os *schemas* existentes:

```sql
\dn

```

Foi identificado o *schema* `public`.
Em seguida consultei os detalhes do *schema*:

```sql
\dn+ public

```

También verifiquei as tabelas existentes:

```sql
\dt

```

O PostgreSQL informou que nenhuma relação havia sido encontrada.
Utilizei também:

```sql
\dt *.*

```

para ampliar a consulta e observar objetos de diferentes *schemas*.
Depois consultei o `search_path`:

```sql
SHOW search_path;

```

O resultado foi:

```text
"$user", public

```

Por fim, consultei quais tabelas pertenciam ao usuário `appuser`:

```sql
SELECT schemaname, tablename, tableowner
FROM pg_tables
WHERE tableowner = 'appuser';

```

O resultado foi de 0 linhas, indicando que o usuário ainda não possui tabelas.

### Dificuldades encontradas

Durante a atividade, tive alguns erros de execução.
Ao tentar consultar o caminho padrão, digitei:

```sql
SHOW search_patch;

```

O PostgreSQL retornou: *parâmetro de configuração "search_patch" desconhecido*. Entendi que havia digitado o nome do parâmetro incorretamente. O correto é `search_path`.

Também executei:

```sql
\d *.*

```

e apareceu uma grande quantidade de objetos, incluindo objetos relacionados ao `information_schema`. No início fiquei em dúvida se aqueles objetos eram tabelas que eu havia criado. Depois entendi que o comando estava mostrando objetos de diferentes *schemas* do PostgreSQL e que isso não significava que eu havia criado tabelas no `appdb`.

### Como resolvi

Corrigi o comando para:

```sql
SHOW search_path;

```

e consegui visualizar o caminho padrão da sessão.
Também utilizei `\dt` para verificar especificamente as tabelas e confirmei que não havia nenhuma tabela criada no banco.

### O que aprendi

* Aprendi que o database é o banco de dados como um todo, enquanto o *schema* é uma forma de organizar os objetos existentes dentro de um database.
* Aprendi que o *schema* `public` é o *schema* padrão disponível no database.
* Também aprendi a utilizar comandos do `psql` para investigar a estrutura de um database, como `\dn`, `\dt` e `\conninfo`.
* Compreendi que o `search_path` define a ordem dos *schemas* que o PostgreSQL utiliza para procurar objetos quando o *schema* não é informado explicitamente.
* Também aprendi que erros de digitação em comandos podem gerar mensagens do PostgreSQL que ajudam a identificar o problema.

### Resultado da atividade

Acessei o `appdb`, listei os *schemas*, identifiquei o `public`, verifiquei as tabelas existentes, consultei o `search_path` e verifiquei as tabelas pertencentes ao `appuser`.
Não criei nenhuma tabela, conforme solicitado no exercício.






## Atividade 2 — Criando uma estrutura para a aplicação
**Data:** 17/09/2026

### O que precisava fazer
Criar uma estrutura para a aplicação `appdb`, que precisa armazenar informações sobre pessoas.
A atividade solicitava a criação de um *schema* chamado `app` e, dentro dele, uma tabela chamada `pessoas`.
A tabela deveria possuir, no mínimo, um identificador, nome, e-mail e data de nascimento.
Antes da criação, precisava pesquisar sobre colunas, tipos de dados, números inteiros, texto, datas, chave primária e identificação única dos registros.

### O que pesquisei
Pesquisei o conceito de coluna, tipos de dados básicos do PostgreSQL, tipos para números inteiros, tipos para texto, o tipo `date`, chave primária e criação de *schemas* e tabelas.
Também procurei compreender por que uma tabela precisa de uma identificação única para seus registros.

### O que foi criado
Foi criado o *schema*:
```sql
app

```

Dentro dele foi criada a tabela:

```sql
app.pessoas

```

A tabela foi definida com as seguintes colunas:

* `id` — `integer`, utilizado como identificador;
* `nome` — `text`, utilizado para armazenar o nome;
* `email` — `text`, utilizado para armazenar o e-mail;
* `data_nascimento` — `date`, utilizado para armazenar a data de nascimento.

O campo `id` foi definido como `PRIMARY KEY` e utilizando `GENERATED ALWAYS AS IDENTITY`, permitindo que o PostgreSQL gere automaticamente os identificadores.

### Justificativa das escolhas

* Escolhi `integer` para o identificador porque ele representa números inteiros e é adequado para uma identificação numérica dos registros.
* Utilizei `text` para `nome` e `email` porque essas informações são compostas por caracteres e não precisam ser tratadas como números ou datas.
* Utilizei `date` para `data_nascimento` porque essa coluna representa uma data e deve ser armazenada como tal.
* A chave primária foi definida no campo `id` porque cada registro precisa possuir uma identificação única dentro da tabela.

### Dificuldades encontradas

Durante a atividade, a principal dificuldade foi entender a diferença entre uma coluna e seu tipo de dado.
Também foi necessário compreender a finalidade da chave primária e por que o identificador não deve depender do nome da pessoa.

### Como resolvi

Analisei cada informação que a tabela precisava armazenar e relacionei cada uma ao tipo de dado correspondente.
Também compreendi que o `id` serve para diferenciar os registros, mesmo quando duas pessoas possuem o mesmo nome.

### O que aprendi

* Aprendi que uma tabela é formada por colunas e que cada coluna possui um tipo de dado que determina o tipo de informação que ela armazena.
* Aprendi a criar um *schema* utilizando `CREATE SCHEMA` e uma tabela utilizando `CREATE TABLE`.
* Também aprendi a utilizar a notação `schema.tabela`, como `app.pessoas`, para indicar exatamente onde a tabela está localizada.
* Compreendi a finalidade da chave primária e aprendi que `GENERATED ALWAYS AS IDENTITY` permite que o PostgreSQL gere automaticamente os identificadores.
* Também aprendi a verificar a estrutura criada utilizando os comandos `\dt` e `\d`.

### Resultado da atividade

* O *schema* `app` foi criado.
* A tabela `pessoas` foi criada dentro do *schema* `app`, contendo as colunas `id`, `nome`, `email` e `data_nascimento`.
* A coluna `id` foi definida como chave primária e identificador gerado automaticamente.
* A estrutura da tabela foi verificada após sua criação.



# Diário de Bordo

## Data
18/09/2026

## Atividade
Atividade 3 - Constraints: fazendo o banco ajudar

## O que precisava fazer

Verificar a estrutura da tabela `app.pessoas`, pesquisar sobre as
constraints PRIMARY KEY, NOT NULL, UNIQUE, CHECK e DEFAULT, aplicar
as restrições necessárias e realizar testes para observar como o
PostgreSQL impede a entrada de dados inválidos.

## O que pesquisei

Pesquisei sobre:

- PRIMARY KEY
- NOT NULL
- UNIQUE
- CHECK
- DEFAULT
- Restrições de integridade no PostgreSQL

Entendi que as constraints permitem que o próprio banco de dados
controle algumas regras dos dados, evitando informações inválidas
ou inconsistentes.

## O que foi realizado

Primeiro verifiquei a estrutura da tabela com:

\d app.pessoas

A tabela já possuía uma PRIMARY KEY na coluna `id`, utilizando
também uma coluna `GENERATED ALWAYS AS IDENTITY`.

Depois alterei a coluna `nome` para não aceitar valores nulos:

ALTER TABLE app.pessoas
ALTER COLUMN nome SET NOT NULL;

Em seguida criei uma restrição UNIQUE para impedir e-mails repetidos:

ALTER TABLE app.pessoas
ADD CONSTRAINT pessoas_email_unique UNIQUE (email);

Também criei uma restrição CHECK para impedir datas de nascimento
futuras:

ALTER TABLE app.pessoas
ADD CONSTRAINT pessoas_data_nascimento_check
CHECK (data_nascimento <= CURRENT_DATE);

Depois utilizei \d app.pessoas para verificar se as novas
restrições estavam presentes na tabela.

## Dificuldades encontradas

Durante uma tentativa de inserção apareceu um erro de sintaxe:

ERRO: erro de sintaxe em ou próximo a "~"


Entendi que esse erro não estava relacionado às constraints da
tabela, mas à forma como o comando foi escrito no terminal.
Executei novamente o comando sem os caracteres extras e a inserção
foi realizada corretamente.

## Testes realizados

### Teste do NOT NULL

Tentei inserir um registro sem informar o nome:

INSERT INTO app.pessoas (email, data_nascimento)
VALUES ('teste_null@email.com', '1990-01-01');

O PostgreSQL rejeitou a operação:

ERRO: o valor nulo na coluna "nome" da relação "pessoas" viola a
restrição de não-nulo

Com isso confirmei que a constraint NOT NULL estava funcionando.

### Teste do UNIQUE

Tentei inserir outro registro utilizando um e-mail que já existia:

INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Carlos Teste', 'joao.teste@email.com', '1995-06-15');

O PostgreSQL rejeitou a operação:

ERRO: duplicar valor da chave viola a restrição de unicidade
"pessoas_email_unique"

DETALHE: Chave (email)=(joao.teste@email.com) já existe.

Com isso confirmei que a constraint UNIQUE estava funcionando.

### Teste do CHECK

Tentei inserir uma pessoa com uma data de nascimento futura:

INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Pessoa Futuro', 'futuro@email.com', '2035-01-01');

O PostgreSQL rejeitou a operação:

ERRO: a nova linha da relação "pessoas" viola a restrição de
verificação "pessoas_data_nascimento_check"

Com isso confirmei que a constraint CHECK estava funcionando.

### Teste da PRIMARY KEY

Tentei inserir outro registro utilizando um `id` que já existia:

INSERT INTO app.pessoas (id, nome, email, data_nascimento)
OVERRIDING SYSTEM VALUE
VALUES (1, 'Pedro Teste', 'pedro.teste@email.com', '1993-03-03');

O PostgreSQL rejeitou a operação:

ERRO: duplicar valor da chave viola a restrição de unicidade
"pessoas_pkey"

DETALHE: Chave (id)=(1) já existe.

Com isso confirmei que a PRIMARY KEY impede a existência de dois
registros com o mesmo identificador.


## O que aprendi

Aprendi que as constraints fazem o banco de dados ajudar a controlar
a qualidade dos dados.

A PRIMARY KEY identifica cada registro de forma única.

A NOT NULL impede que uma coluna obrigatória fique sem valor.

A UNIQUE impede valores duplicados em uma coluna.

A CHECK permite criar uma regra que os valores precisam obedecer.

Também aprendi que uma tentativa de INSERT que viola uma constraint
é rejeitada pelo PostgreSQL e o registro não é inserido.

Além disso, aprendi a interpretar as mensagens de erro do PostgreSQL
para identificar qual regra foi violada.

## Resultado final

A tabela `app.pessoas` ficou com dois registros válidos:

2 | Maria Teste | joao.teste@email.com | 1992-05-10
1 | Outro João  | outro.joao@email.com | 1991-02-02

As tentativas que violaram as constraints não permaneceram na tabela.

## Links consultados

- https://www.postgresql.org/docs/17/ddl-constraints.html
- https://www.postgresql.org/docs/17/ddl-default.html



Claro. Vou montar o **Diário de Bordo da Atividade 4** seguindo o mesmo formato que seu professor pediu e usando os resultados que você realmente obteve.

# Diário de Bordo

## Data

19/09/2026

## Atividade

Atividade 4 - Inserindo e consultando dados

## O que precisava fazer

Inserir registros válidos na tabela `app.pessoas`, realizar uma tentativa de inserir um registro inválido e uma tentativa de violar uma constraint.

Depois, realizar consultas utilizando `SELECT`, `WHERE`, `ORDER BY` e `COUNT`, observando e interpretando os resultados.

## O que pesquisei

Pesquisei sobre os comandos e conceitos:

* PostgreSQL INSERT
* PostgreSQL SELECT
* PostgreSQL WHERE
* PostgreSQL ORDER BY
* PostgreSQL COUNT
* PostgreSQL query

Entendi que o `INSERT` é utilizado para inserir registros, enquanto o `SELECT` é utilizado para consultar dados.

Também pesquisei a estrutura básica de uma consulta:

```sql
SELECT
FROM
WHERE
ORDER BY
```

Entendi que cada parte possui uma função diferente na consulta.

## O que foi realizado

Primeiramente inseri cinco novos registros válidos na tabela `app.pessoas`:

* Ana Souza
* Bruno Silva
* Carla Oliveira
* Daniel Santos
* Fernanda Costa

Todos os comandos retornaram:

```text
INSERT 0 1
```

Isso confirmou que cada registro foi inserido com sucesso.

A tabela, que possuía dois registros anteriormente, passou a possuir sete registros.

## Teste de registro inválido

Realizei uma tentativa de inserir uma pessoa sem informar o nome:

```sql
INSERT INTO app.pessoas (email, data_nascimento)
VALUES ('sem.nome@email.com', '1990-05-10');
```

O PostgreSQL rejeitou a operação e apresentou:

```text
ERRO: o valor nulo na coluna "nome" da relação "pessoas" viola a restrição de não-nulo
DETALHE: Registro que falhou contém (11, null, sem.nome@email.com, 1990-05-10).
```

Entendi que isso aconteceu porque a coluna `nome` possui a constraint `NOT NULL`, criada na atividade anterior.

Também observei que o registro não foi inserido na tabela.

## Teste de violação de constraint

Realizei uma tentativa de inserir um novo registro utilizando um e-mail que já existia:

```sql
INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Gabriel Almeida', 'joao.teste@email.com', '1997-04-12');
```

O PostgreSQL rejeitou a operação:

```text
ERRO: duplicar valor da chave viola a restrição de unicidade "pessoas_email_unique"
DETALHE: Chave (email)=(joao.teste@email.com) já existe.
```

Entendi que a constraint `UNIQUE` impede que dois registros possuam o mesmo e-mail.

## Consulta de todos os registros

Utilizei:

```sql
SELECT * FROM app.pessoas;
```

O resultado apresentou os sete registros existentes e todas as colunas da tabela:

* `id`
* `nome`
* `email`
* `data_nascimento`

Com isso entendi que o `*` representa todas as colunas.

## Consulta utilizando colunas específicas

Depois utilizei:

```sql
SELECT nome, email
FROM app.pessoas;
```

Nesse caso, o PostgreSQL apresentou somente as colunas `nome` e `email`.

Aprendi que não é necessário sempre consultar todas as colunas. Podemos escolher exatamente quais informações queremos visualizar.

## Consulta utilizando WHERE

Utilizei:

```sql
SELECT nome, data_nascimento
FROM app.pessoas
WHERE data_nascimento > '1995-01-01';
```

O resultado apresentou três pessoas:

* Ana Souza
* Carla Oliveira
* Fernanda Costa

Entendi que o `WHERE` é utilizado para filtrar os registros de acordo com uma condição.

Nesse caso, foram mostradas somente as pessoas cuja data de nascimento era posterior a `01/01/1995`.

## Consulta utilizando ORDER BY

Utilizei:

```sql
SELECT nome, data_nascimento
FROM app.pessoas
ORDER BY data_nascimento;
```

O PostgreSQL apresentou os registros ordenados pela data de nascimento, começando pela data mais antiga e terminando pela mais recente.

Entendi que o `ORDER BY` é utilizado para definir a ordem em que os resultados serão apresentados.

## Consulta utilizando COUNT

Por último, utilizei:

```sql
SELECT COUNT(*)
FROM app.pessoas;
```

O resultado foi:

```text
count
-----
7
```

Com isso confirmei que a tabela possuía sete registros.

Entendi que o `COUNT` é utilizado para realizar uma contagem dos registros retornados pela consulta.

## Dificuldades encontradas

Durante a atividade não tive dificuldade com os comandos principais de consulta.

As principais situações observadas foram as tentativas de inserção rejeitadas pelas constraints.

Uma tentativa de inserir um registro sem nome foi rejeitada pela constraint `NOT NULL`.

Outra tentativa de inserir um e-mail que já existia foi rejeitada pela constraint `UNIQUE`.

Esses erros foram importantes para entender que o PostgreSQL não apenas armazena os dados, mas também verifica as regras definidas para a tabela.

## Como resolvi

Analisei as mensagens apresentadas pelo PostgreSQL e identifiquei qual regra estava sendo violada.

No caso do registro sem nome, verifiquei que a coluna `nome` não aceita valores nulos.

No caso do e-mail duplicado, verifiquei que já existia um registro utilizando aquele e-mail e que a constraint `pessoas_email_unique` impedia a duplicação.

Depois utilizei `SELECT * FROM app.pessoas` para confirmar os registros que realmente permaneceram na tabela.

## O que aprendi

Aprendi a inserir registros utilizando `INSERT INTO` e `VALUES`.

Aprendi que o `SELECT *` mostra todas as colunas e todos os registros da consulta.

Aprendi que é possível selecionar somente algumas colunas, como `nome` e `email`.

Aprendi que `WHERE` serve para filtrar registros de acordo com uma condição.

Aprendi que `ORDER BY` serve para ordenar os resultados de uma consulta.

Aprendi que `COUNT(*)` permite contar a quantidade de registros.

Também aprendi que as constraints criadas anteriormente continuam sendo aplicadas durante a inserção de novos dados.

Uma observação importante foi perceber que tentativas de inserção que violam constraints são rejeitadas e não permanecem na tabela.

## Resultado final

Ao final da atividade, a tabela `app.pessoas` ficou com sete registros válidos:

```text
id | nome           | email                    | data_nascimento
---+----------------+--------------------------+----------------
2  | Maria Teste    | joao.teste@email.com     | 1992-05-10
1  | Outro João     | outro.joao@email.com     | 1991-02-02
6  | Ana Souza      | ana.souza@email.com      | 1995-03-15
7  | Bruno Silva    | bruno.silva@email.com    | 1988-07-22
8  | Carla Oliveira | carla.oliveira@email.com | 2000-11-05
9  | Daniel Santos  | daniel.santos@email.com  | 1992-01-30
10 | Fernanda Costa | fernanda.costa@email.com | 1998-09-18
```

A consulta com `COUNT(*)` confirmou:

```text
7 registros
```

## Conclusão

A atividade permitiu praticar a inserção e consulta de dados no PostgreSQL.

Foi possível perceber na prática a diferença entre inserir informações e consultar informações, além de entender como `WHERE`, `ORDER BY` e `COUNT` modificam o resultado de uma consulta.

Também foi possível observar novamente o funcionamento das constraints, que impediram a entrada de dados que não respeitavam as regras definidas para a tabela.

## Links consultados

* PostgreSQL - INSERT
* PostgreSQL - SELECT
* PostgreSQL - WHERE
* PostgreSQL - ORDER BY
* PostgreSQL - COUNT
* PostgreSQL - SQL Queries


# DIÁRIO DE ATIVIDADES – QUINZENA 10

## Data

21/09/2026

## Atividade

Atividade 5 – Alterando e removendo dados

## O que precisava fazer

Nesta atividade, precisava praticar a alteração e a remoção de registros da tabela `app.pessoas`.

As tarefas solicitadas foram:

* Escolher um registro e alterar uma informação;
* Remover um registro específico;
* Pesquisar sobre os comandos `UPDATE`, `DELETE` e `WHERE`;
* Entender a importância da utilização do `WHERE`;
* Verificar os resultados depois das alterações;
* Compreender o que poderia acontecer caso um `UPDATE` ou `DELETE` fosse executado sem um `WHERE` correto.

A atividade recomendava seguir a sequência:

**Consultar → Confirmar → Alterar → Verificar novamente.**

## O que pesquisei

Pesquisei sobre os comandos utilizados para modificar dados no PostgreSQL:

### UPDATE

O comando `UPDATE` é utilizado para alterar informações que já existem em uma tabela.

Exemplo utilizado:

```sql
UPDATE app.pessoas
SET email = 'ana.souza.novo@email.com'
WHERE id = 6;
```

Nesse comando:

* `UPDATE app.pessoas` indica a tabela que será alterada;
* `SET` informa qual coluna será modificada;
* `email = 'ana.souza.novo@email.com'` define o novo valor;
* `WHERE id = 6` determina qual registro será alterado.

### DELETE

O comando `DELETE` é utilizado para remover registros de uma tabela.

Exemplo utilizado:

```sql
DELETE FROM app.pessoas
WHERE id = 10;
```

Nesse caso, somente o registro cujo `id` era igual a `10` foi removido.

### WHERE

O `WHERE` é utilizado para definir uma condição e selecionar quais registros serão afetados.

Ele é muito importante em comandos de alteração e exclusão, porque evita que todos os registros da tabela sejam modificados ou removidos.

## Como foi realizada a atividade

Primeiro, consultei o registro de ID 6 para confirmar qual pessoa seria alterada:

```sql
SELECT *
FROM app.pessoas
WHERE id = 6;
```

O resultado mostrou:

```text
6 | Ana Souza | ana.souza@email.com | 1995-03-15
```

Depois alterei o e-mail da Ana:

```sql
UPDATE app.pessoas
SET email = 'ana.souza.novo@email.com'
WHERE id = 6;
```

O PostgreSQL retornou:

```text
UPDATE 1
```

Isso indicou que um registro foi alterado.

Depois fiz uma nova consulta para confirmar a alteração:

```sql
SELECT *
FROM app.pessoas
WHERE id = 6;
```

O resultado mostrou o novo e-mail:

```text
6 | Ana Souza | ana.souza.novo@email.com | 1995-03-15
```

Em seguida, antes de excluir um registro, consultei o ID 10:

```sql
SELECT *
FROM app.pessoas
WHERE id = 10;
```

O resultado mostrou:

```text
10 | Fernanda Costa | fernanda.costa@email.com | 1998-09-18
```

Depois removi o registro:

```sql
DELETE FROM app.pessoas
WHERE id = 10;
```

O PostgreSQL retornou:

```text
DELETE 1
```

Isso indicou que um registro foi excluído.

Para confirmar a exclusão, executei novamente:

```sql
SELECT *
FROM app.pessoas
WHERE id = 10;
```

O resultado foi:

```text
(0 linha)
```

Isso confirmou que o registro de ID 10 não estava mais na tabela.

Por fim, consultei todos os registros ordenados pelo ID:

```sql
SELECT *
FROM app.pessoas
ORDER BY id;
```

O resultado final apresentou 6 registros.

## Dificuldades encontradas

A principal dificuldade foi compreender a importância do `WHERE` nos comandos `UPDATE` e `DELETE`.

Foi necessário entender que não basta saber escrever o comando. Também é necessário definir corretamente quais registros devem ser afetados.

## Como resolvi

Resolvi a dificuldade seguindo a orientação da atividade de primeiro consultar o registro, confirmar o ID e somente depois executar o `UPDATE` ou `DELETE`.

Também compreendi, por meio dos exemplos, o risco de executar esses comandos sem uma condição `WHERE`.

Por exemplo:

```sql
UPDATE app.pessoas
SET email = 'novo@email.com';
```

poderia alterar o e-mail de todos os registros da tabela.

Da mesma forma:

```sql
DELETE FROM app.pessoas;
```

poderia remover todos os registros da tabela.

Esses comandos foram utilizados apenas como exemplos para compreender o risco e não foram executados.

## O que aprendi

Aprendi a utilizar os comandos `UPDATE` e `DELETE` para modificar e remover dados no PostgreSQL.

Também aprendi que o `WHERE` é fundamental para determinar exatamente quais registros serão afetados.

Entendi a importância de sempre conferir o registro antes de realizar uma alteração ou exclusão e verificar novamente depois da operação.

A sequência que passei a utilizar foi:

**Consultar → Confirmar → Alterar/Remover → Verificar.**

Também compreendi que uma pequena diferença em um comando, principalmente a ausência ou utilização incorreta do `WHERE`, pode fazer com que uma operação atinja vários registros em vez de apenas um.

## Resultado final

Após a alteração e a exclusão, a tabela `app.pessoas` ficou com 6 registros:

```text
1 | Outro João     | outro.joao@email.com
2 | Maria Teste    | joao.teste@email.com
6 | Ana Souza      | ana.souza.novo@email.com
7 | Bruno Silva    | bruno.silva@email.com
8 | Carla Oliveira | carla.oliveira@email.com
9 | Daniel Santos  | daniel.santos@email.com
```

A alteração do e-mail da Ana foi confirmada e o registro da Fernanda foi removido e posteriormente conferido.

## Links consultados

* Documentação oficial do PostgreSQL sobre `UPDATE`.
* Documentação oficial do PostgreSQL sobre `DELETE`.
* Documentação oficial do PostgreSQL sobre `WHERE`.

