
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



Perfeito. Com os resultados que você trouxe, dá para montar o diário **sem inventar nenhum resultado**. Inclusive, os erros que apareceram fazem parte do aprendizado e vale a pena registrá-los.

Um ponto importante: no início, `appuser` aparecia como proprietário da tabela e possuía vários privilégios. Depois da configuração, você mudou o proprietário para `postgres` e deixou explicitamente para `appuser` apenas **SELECT, INSERT, UPDATE e DELETE**. Os testes finais confirmaram que essas quatro operações continuam funcionando.

# DIÁRIO DE ATIVIDADES – QUINZENA 10

## Data

24/09/2026

## Atividade

Atividade 6 – Quem pode acessar a tabela?

## O que precisava fazer

Nesta atividade, precisava investigar as permissões da role `appuser` sobre a tabela `app.pessoas`.

O objetivo era verificar se `appuser` conseguia consultar, inserir, alterar e remover registros, identificar o proprietário da tabela, verificar os privilégios existentes e configurar somente as permissões necessárias para trabalhar com a tabela.

Também precisava compreender a diferença entre conseguir acessar o database e possuir permissões sobre uma tabela.

---

## O que pesquisei

Pesquisei sobre:

* privilégios de tabelas no PostgreSQL;
* `GRANT`;
* `SELECT`, `INSERT`, `UPDATE` e `DELETE`;
* proprietário de tabela;
* proprietário de schema;
* `information_schema.role_table_grants`;
* `pg_tables`;
* privilégio `USAGE` em schema;
* diferença entre autenticação e autorização.

Também retomei o conceito de que o acesso ocorre em diferentes camadas:

**Conexão → Autenticação → Database → Schema → Tabela → Privilégios.**

---

## 1. Testando o acesso como `appuser`

Primeiro conectei ao database utilizando a role `appuser`:

```bash
psql -h localhost -U appuser -d appdb
```

Depois confirmei a identidade e o database:

```sql
SELECT current_user, current_database();
```

O resultado foi:

```text
current_user | current_database
-------------+----------------
appuser      | appdb
```

Isso confirmou que os testes estavam sendo realizados realmente como `appuser` dentro do database `appdb`.

---

## 2. Testando `SELECT`

Executei:

```sql
SELECT *
FROM app.pessoas;
```

A consulta funcionou e retornou os seis registros existentes na tabela.

Isso demonstrou que `appuser` conseguia consultar a tabela `app.pessoas`.

---

## 3. Identificando o proprietário da tabela

Consultei as informações da tabela através de `pg_tables`:

```sql
SELECT
    schemaname,
    tablename,
    tableowner
FROM pg_tables
WHERE schemaname = 'app'
  AND tablename = 'pessoas';
```

O resultado mostrou:

```text
schemaname | tablename | tableowner
-----------+-----------+-----------
app        | pessoas   | appuser
```

Descobri que, inicialmente, o proprietário da tabela `app.pessoas` era `appuser`.

---

## 4. Investigando os privilégios existentes

Utilizei:

```sql
SELECT
    grantee,
    table_schema,
    table_name,
    privilege_type
FROM information_schema.role_table_grants
WHERE table_schema = 'app'
  AND table_name = 'pessoas'
  AND grantee = 'appuser';
```

Inicialmente, foram apresentados sete privilégios:

```text
INSERT
SELECT
UPDATE
DELETE
TRUNCATE
REFERENCES
TRIGGER
```

Isso mostrou que `appuser` possuía mais privilégios do que os quatro necessários para trabalhar normalmente com os dados da tabela.

---

## 5. Testando INSERT, UPDATE e DELETE

Para testar as operações sem deixar alterações permanentes na tabela, utilizei transações com `BEGIN` e `ROLLBACK`.

### INSERT

Executei:

```sql
BEGIN;

INSERT INTO app.pessoas
    (nome, email, data_nascimento)
VALUES
    ('Teste Permissao', 'teste.permissao@email.com', '1990-01-01');

ROLLBACK;
```

O PostgreSQL retornou:

```text
INSERT 0 1
```

Isso demonstrou que `appuser` conseguia inserir registros.

O `ROLLBACK` desfez a inserção para que o registro de teste não permanecesse na tabela.

### UPDATE

Depois testei:

```sql
BEGIN;

UPDATE app.pessoas
SET email = 'teste.permissao@email.com'
WHERE id = 1;

ROLLBACK;
```

O resultado foi:

```text
UPDATE 1
```

Isso demonstrou que `appuser` conseguia alterar registros.

Novamente utilizei `ROLLBACK` para desfazer o teste.

### DELETE

Depois testei:

```sql
BEGIN;

DELETE FROM app.pessoas
WHERE id = 1;

ROLLBACK;
```

O resultado foi:

```text
DELETE 1
```

Isso demonstrou que `appuser` conseguia remover registros.

O `ROLLBACK` garantiu que o registro não fosse realmente removido.

---

## 6. Identificando o proprietário do schema

Como administrador, consultei:

```sql
SELECT
    schema_name,
    schema_owner
FROM information_schema.schemata
WHERE schema_name = 'app';
```

O resultado foi:

```text
schema_name | schema_owner
------------+-------------
app         | appuser
```

Descobri que o schema `app` também tinha `appuser` como proprietário naquele momento.

---

## 7. Separando proprietário e usuário da aplicação

Para aplicar o princípio de conceder somente os privilégios necessários, alterei o proprietário da tabela:

```sql
ALTER TABLE app.pessoas OWNER TO postgres;
```

O PostgreSQL retornou:

```text
ALTER TABLE
```

A partir desse momento, `postgres` passou a ser o proprietário da tabela.

A intenção foi separar a função administrativa da função da role utilizada para trabalhar com os dados.

---

## 8. Concedendo acesso ao schema

Executei:

```sql
GRANT USAGE ON SCHEMA app TO appuser;
```

O comando concedeu a `appuser` o privilégio `USAGE` sobre o schema `app`.

Esse privilégio permite que a role utilize objetos do schema para os quais também possua as permissões necessárias.

---

## 9. Concedendo somente os privilégios necessários

Executei:

```sql
GRANT SELECT, INSERT, UPDATE, DELETE
ON app.pessoas
TO appuser;
```

Esses privilégios correspondem às operações necessárias sobre os dados:

* `SELECT` → consultar;
* `INSERT` → inserir;
* `UPDATE` → alterar;
* `DELETE` → remover.

Depois consultei novamente os privilégios:

```sql
SELECT
    grantee,
    table_schema,
    table_name,
    privilege_type
FROM information_schema.role_table_grants
WHERE table_schema = 'app'
  AND table_name = 'pessoas'
  AND grantee = 'appuser'
ORDER BY privilege_type;
```

O resultado final apresentou somente:

```text
DELETE
INSERT
SELECT
UPDATE
```

Assim, os privilégios foram reduzidos aos quatro necessários para trabalhar com os dados da tabela.

---

## 10. Testando novamente as permissões

Depois da configuração, voltei a conectar como `appuser`:

```bash
psql -h localhost -U appuser -d appdb
```

Confirmei novamente:

```sql
SELECT current_user, current_database();
```

Resultado:

```text
appuser | appdb
```

Depois consultei a tabela:

```sql
SELECT *
FROM app.pessoas;
```

A consulta continuou funcionando.

Também repeti os testes de `INSERT`, `UPDATE` e `DELETE` utilizando `BEGIN` e `ROLLBACK`.

### INSERT

Resultado:

```text
INSERT 0 1
```

### UPDATE

Resultado:

```text
UPDATE 1
```

### DELETE

Resultado:

```text
DELETE 1
```

Todos os testes funcionaram e foram revertidos com `ROLLBACK`.

---

## Dificuldades encontradas

A principal dificuldade ocorreu ao tentar voltar para a role administrativa `postgres`.

Primeiro tentei:

```bash
sudo -u postgres psql -d appdb
```

e recebi:

```text
postgres não está no arquivo sudoers.
```

Também tentei:

```bash
psql -h localhost -U postgres -d appdb
```

mas a autenticação por senha falhou.

Durante uma dessas tentativas, digitei a senha diretamente no terminal Linux e recebi:

```text
bash: 091206: comando não encontrado
```

---

## Como resolvi

Percebi que já estava conectado ao sistema operacional como o usuário Linux `postgres`.

Por isso, não era necessário utilizar `sudo -u postgres`.

Também não era necessário utilizar a conexão TCP com senha.

Utilizei:

```bash
psql -d appdb
```

A conexão funcionou e confirmei:

```sql
SELECT current_user, current_database();
```

Resultado:

```text
postgres | appdb
```

Com isso, consegui acessar o PostgreSQL como a role administrativa `postgres`.

Esse problema ajudou a compreender a diferença entre:

* usuário Linux `postgres`;
* role PostgreSQL `postgres`;
* autenticação por senha;
* autenticação local através do método configurado no `pg_hba.conf`.

---

## Erros interessantes

Durante a investigação também ocorreram erros de digitação em algumas consultas, como nomes incorretos de colunas e objetos.

Em uma tentativa, a consulta ficou incompleta e precisei utilizar:

```text
Ctrl + C
```

para cancelar a operação e voltar ao prompt.

Isso ajudou a entender que erros de sintaxe ou consultas incompletas não significam necessariamente um problema nas permissões ou no PostgreSQL; primeiro é necessário verificar exatamente o comando que foi enviado.

---

## O que aprendi

Aprendi que estar conectado a um database não significa automaticamente possuir todas as permissões sobre suas tabelas.

Aprendi a identificar o proprietário de uma tabela utilizando `pg_tables` e o proprietário de um schema utilizando `information_schema.schemata`.

Também aprendi a consultar privilégios através de `information_schema.role_table_grants`.

Compreendi a diferença entre ser proprietário de uma tabela e possuir privilégios concedidos por `GRANT`.

Aprendi a utilizar `GRANT USAGE` para o schema e `GRANT SELECT, INSERT, UPDATE, DELETE` para permitir que uma role trabalhe com os dados de uma tabela.

Também aprendi a utilizar `BEGIN` e `ROLLBACK` para realizar testes sem deixar alterações permanentes no banco.

A atividade mostrou, na prática, a importância do princípio de conceder somente os privilégios necessários.

---

## Resultado final

A configuração final ficou com:

```text
Database: appdb
    |
    └── Schema: app
          |
          └── Tabela: pessoas
```

Proprietário da tabela:

```text
postgres
```

Role utilizada para trabalhar com os dados:

```text
appuser
```

Privilégios finais de `appuser` sobre `app.pessoas`:

```text
SELECT
INSERT
UPDATE
DELETE
```

Os quatro privilégios foram testados com sucesso.

Os testes de inserção, alteração e exclusão foram realizados dentro de transações e revertidos com `ROLLBACK`, preservando os dados originais da tabela.

## Links consultados

* Documentação oficial do PostgreSQL sobre privilégios e `GRANT`.
* Documentação oficial do PostgreSQL sobre `information_schema`.
* Documentação oficial do PostgreSQL sobre roles e permissões.
  :::



# QUINZENA 11

# Diário de Bordo — Quinzena 11 — Atividade 1

**Data:** 28/09/2026
**Atividade:** Revisando o modelo de roles

## O que precisava fazer

Investigar o conceito de roles no PostgreSQL e analisar as roles existentes no ambiente de laboratório sem realizar alterações.

A atividade tinha como objetivo identificar quais roles possuem LOGIN, quais não possuem, quais são superusuárias, quais podem criar databases e quais podem criar outras roles.

Também era necessário compreender a diferença entre uma role PostgreSQL e um usuário Linux, além de entender como uma role pode ser utilizada como grupo de acesso.

## O que foi pesquisado

Foram estudados os conceitos de:

* PostgreSQL Role;
* LOGIN;
* roles com e sem LOGIN;
* roles utilizadas como grupos;
* membership;
* herança de privilégios;
* atributos de roles;
* superuser;
* CREATEDB;
* CREATEROLE;
* diferença entre usuário Linux e role PostgreSQL.

Também foram utilizadas consultas sobre a visão `pg_roles` para investigar os atributos das roles existentes.

## Procedimentos realizados

Primeiramente foi verificado o usuário Linux utilizado no laboratório com:

`whoami`

O objetivo foi diferenciar a identidade do sistema operacional da identidade utilizada dentro do PostgreSQL.

Depois foi realizada a conexão com o database `appdb` utilizando:

`psql -d appdb`

Após a conexão, foi utilizada a consulta:

`SELECT current_user, current_database();`

para identificar a role PostgreSQL utilizada na sessão e confirmar o database conectado.

Em seguida, foram investigadas as roles existentes por meio da visão `pg_roles`, consultando atributos como:

* `rolname`;
* `rolcanlogin`;
* `rolsuper`;
* `rolcreatedb`;
* `rolcreaterole`;
* `rolinherit`;
* `rolreplication`;
* `rolbypassrls`.

Também foi utilizado o comando `\du` do cliente `psql` para visualizar as roles de maneira resumida.

Foram realizadas consultas específicas para identificar:

* roles que possuem LOGIN;
* roles que não possuem LOGIN;
* roles superusuárias;
* roles que podem criar databases;
* roles que podem criar outras roles.

A role `appuser`, utilizada nas atividades anteriores, também foi analisada individualmente.

## Dificuldades encontradas

A principal dificuldade conceitual foi diferenciar um usuário Linux de uma role PostgreSQL.

Embora possa existir um usuário Linux chamado `postgres` e uma role PostgreSQL chamada `postgres`, são mecanismos diferentes.

Também foi necessário compreender que uma role não precisa obrigatoriamente possuir LOGIN.

Uma role sem LOGIN pode ser utilizada como uma role de grupo para organizar privilégios e posteriormente ser associada a outras roles por meio de membership.

## O que aprendi

Aprendi que no PostgreSQL o conceito de role é mais abrangente que simplesmente "usuário".

Uma role pode representar uma conta que realiza login ou pode funcionar como uma estrutura de organização de privilégios.

O atributo LOGIN determina se a role pode ser utilizada para autenticação.

Também compreendi que atributos como SUPERUSER, CREATEDB e CREATEROLE representam capacidades administrativas diferentes e devem ser analisados cuidadosamente.

Aprendi ainda que roles podem ser utilizadas como grupos de acesso. Dessa maneira, em vez de conceder os mesmos privilégios individualmente para diversos usuários, podemos organizar esses privilégios em uma role e controlar quais usuários pertencem a ela.

Outro aprendizado importante foi a diferença entre:

* usuário Linux;
* role PostgreSQL;
* database;
* schema;
* tabela;
* privilégio;
* membership.

## Resultado

A atividade permitiu investigar o modelo de roles existente no ambiente sem modificar nenhuma configuração.

O principal aprendizado foi começar a analisar o acesso não apenas perguntando "qual privilégio o usuário possui?", mas também:

> Quem é a role, quais são seus atributos, de quais roles ela participa e de onde vêm seus privilégios?

## Links consultados

PostgreSQL Documentation:
https://www.postgresql.org/docs/current/

Database Roles:
https://www.postgresql.org/docs/current/user-manag.html

Role Attributes:
https://www.postgresql.org/docs/current/role-attributes.html

Role Membership:
https://www.postgresql.org/docs/current/role-membership.html




# Quinzena 11 — Atividade 2: Criando uma role de acesso

**Data:** 30/09/2026

## Objetivo

Criar uma role chamada `app_readonly`, destinada a representar um grupo de acesso no PostgreSQL, sem permitir login direto ou conceder privilégios administrativos.

## Atividade realizada

Antes de criar a role, estudei a diferença entre uma role utilizada para login e uma role utilizada como grupo de privilégios.

Aprendi que uma role com o atributo `LOGIN` pode ser utilizada para se conectar ao PostgreSQL, enquanto uma role com `NOLOGIN` não pode realizar login diretamente. Uma role de grupo permite organizar privilégios e, posteriormente, concedê-los aos usuários que fizerem parte desse grupo.

Para criar a role, utilizei o comando:

```sql
CREATE ROLE app_readonly NOLOGIN;
```

O comando foi executado com sucesso, retornando `CREATE ROLE`.

Em seguida, consultei os atributos da role por meio da tabela `pg_roles`, utilizando o comando:

```sql
SELECT
    rolname,
    rolcanlogin,
    rolsuper,
    rolcreatedb,
    rolcreaterole
FROM pg_roles
WHERE rolname = 'app_readonly';
```

O resultado confirmou que a role foi criada com os seguintes atributos:

* `rolcanlogin`: `false` — não pode realizar login;
* `rolsuper`: `false` — não é superusuário;
* `rolcreatedb`: `false` — não pode criar bancos de dados;
* `rolcreaterole`: `false` — não pode criar outras roles.

Também utilizei o comando `\du app_readonly` para visualizar os atributos da role. O PostgreSQL apresentou a informação “Não é possível conectar”, confirmando que ela não possui permissão para realizar login.

## O que aprendi

Compreendi que uma role de grupo não precisa representar uma pessoa ou uma conta de acesso individual. Sua finalidade é organizar privilégios que poderão ser compartilhados entre diferentes usuários.

A role `app_readonly` foi criada com `NOLOGIN` porque será utilizada como um grupo de acesso, e não como uma conta para conexão direta ao banco de dados.

Também aprendi que é importante evitar atributos administrativos, como `SUPERUSER`, `CREATEDB` e `CREATEROLE`, quando a finalidade da role é apenas organizar permissões.


## Conclusão

A atividade foi concluída com sucesso. A role `app_readonly` foi criada com `NOLOGIN` e sem os atributos administrativos verificados. Com isso, compreendi a diferença entre uma role de login e uma role de grupo, além da importância de organizar os privilégios de acesso no PostgreSQL.




# Quinzena 11 — Atividade 3: Colocando privilégios em uma role de grupo

**Data:** 02/10/2026

## Objetivo

Configurar a role `app_readonly` para representar usuários que podem consultar os dados da tabela `app.pessoas`, mas não podem modificar seus registros.

A atividade teve como objetivo compreender que os privilégios no PostgreSQL são aplicados em diferentes níveis: database, schema e tabela.

## Atividade realizada

Primeiramente, confirmei que estava conectado ao database `appdb` utilizando a role administrativa `postgres`.

Em seguida, configurei os privilégios necessários para a role `app_readonly`.

No nível do **database**, concedi o privilégio `CONNECT`:

```sql
GRANT CONNECT ON DATABASE appdb TO app_readonly;
```

Esse privilégio permite que a role tenha permissão para se conectar ao database `appdb`. Aprendi que possuir `CONNECT` não significa ter acesso às tabelas do banco.

No nível do **schema**, concedi `USAGE`:

```sql
GRANT USAGE ON SCHEMA app TO app_readonly;
```

O `USAGE` permite que a role utilize o schema `app` e consiga acessar os objetos que estão dentro dele, desde que possua os privilégios necessários nesses objetos.

No nível da **tabela**, concedi somente `SELECT`:

```sql
GRANT SELECT ON TABLE app.pessoas TO app_readonly;
```

Esse foi o privilégio que efetivamente permitiu que a role consultasse os dados da tabela `app.pessoas`.

## Entendimento do caminho dos privilégios

Compreendi que o acesso pode ser representado da seguinte maneira:

```text
DATABASE appdb
       │
    CONNECT
       ↓
   SCHEMA app
       │
     USAGE
       ↓
TABLE app.pessoas
       │
     SELECT
       ↓
  consulta dos dados
```

Cada privilégio possui uma finalidade diferente.

`CONNECT` está relacionado ao acesso ao database.

`USAGE` está relacionado à utilização do schema.

`SELECT` está relacionado à leitura dos dados da tabela.

Portanto, possuir um desses privilégios não significa automaticamente possuir os demais.

## Privilégios que não foram concedidos

Como a função da `app_readonly` é somente leitura, não foram concedidos privilégios de alteração dos dados:

```text
INSERT  ❌
UPDATE  ❌
DELETE  ❌
```

Também não foram concedidos privilégios administrativos ou de criação de objetos.

A role continua sendo uma role de grupo com `NOLOGIN`, criada na atividade anterior.

## Verificação dos privilégios

Depois das concessões, consultei os privilégios do database, schema e tabela para confirmar a configuração.

Também utilizei:

```sql
SET ROLE app_readonly;
```

para testar os privilégios da role.

Com a identidade `app_readonly`, o comando:

```sql
SELECT * FROM app.pessoas;
```

funcionou, confirmando que a role possui permissão de leitura.

Também foram realizados testes de `INSERT`, `UPDATE` e `DELETE`. Esses comandos foram bloqueados por falta de privilégio, confirmando que a role não possui permissões para modificar os dados.

Após os testes, utilizei:

```sql
RESET ROLE;
```

para retornar à role administrativa `postgres`.

## O que aprendi

Nesta atividade, compreendi que administrar permissões no PostgreSQL não significa simplesmente conceder `SELECT` em uma tabela.

É necessário entender o caminho que o acesso percorre:

**database → schema → tabela → operação.**

Também aprendi que uma role de grupo pode receber privilégios sem possuir `LOGIN`, podendo posteriormente ser utilizada para organizar os acessos de diferentes usuários.

Outro aprendizado importante foi o princípio do menor privilégio: a role `app_readonly` recebeu somente os privilégios necessários para sua finalidade, sem receber `INSERT`, `UPDATE`, `DELETE` ou privilégios administrativos.

## Conclusão

A atividade foi concluída com sucesso. A role `app_readonly` foi configurada para permitir consulta dos dados de `app.pessoas`, sem permitir alterações.

A configuração final ficou:

```text

INSERT → não permitido
UPDATE → não permitido
DELETE → não permitido
LOGIN → não permitido
SUPERUSER → não permitido
```

Com isso, compreendi na prática a diferença entre os privilégios de database, schema e tabela e como eles podem ser combinados para criar um acesso somente leitura.



# Quinzena 11 — Atividade 4: Fazendo um usuário herdar os privilégios da role

**Data:** 03/10/2026

## Objetivo

Criar uma segunda role de login chamada `appreader` e torná-la membro da role de grupo `app_readonly`, permitindo que `appreader` receba os privilégios de leitura por meio da membership.

## Atividade realizada

Primeiramente, criei a role `appreader` como uma role capaz de realizar login:

```sql
CREATE ROLE appreader LOGIN;
```

Diferentemente da role `app_readonly`, criada na atividade anterior com `NOLOGIN`, a role `appreader` representa uma conta que poderá acessar o PostgreSQL.

Em seguida, configurei uma senha para `appreader` utilizando:

```sql
\password appreader
```

Depois verifiquei os atributos da role e confirmei que ela possui `LOGIN`, não possui privilégios administrativos e está configurada para herdar privilégios das roles das quais for membro.

## Configuração da membership

O próximo passo foi adicionar `appreader` como membro da role `app_readonly`:

```sql
GRANT app_readonly TO appreader;
```

Esse comando não concede diretamente `SELECT` para `appreader`.

Ele estabelece uma relação de membership:

```text
appreader
    |
    | membro de
    v
app_readonly
```

A role `app_readonly` já possuía os privilégios necessários para leitura da tabela `app.pessoas`.

Dessa forma, `appreader` passou a receber esses privilégios por meio da membership.

## Testes realizados

Realizei o login utilizando a role `appreader` e confirmei que a conexão com o database `appdb` foi realizada com sucesso.

Também confirmei a identidade da sessão utilizando:

```sql
SELECT current_user, session_user, current_database();
```

O usuário corrente e o usuário da sessão foram identificados como `appreader`, conectado ao database `appdb`.

Em seguida, testei a consulta:

```sql
SELECT * FROM app.pessoas;
```

O `SELECT` funcionou, demonstrando que `appreader` recebeu o privilégio de leitura por meio da role `app_readonly`.

Também foram realizados testes de alteração dos dados:

```text
INSERT → não permitido
UPDATE → não permitido
DELETE → não permitido
```

Esses comandos foram rejeitados por falta de privilégio, confirmando que `appreader` recebeu somente os privilégios de leitura necessários.

## O que aprendi

O principal conceito aprendido nesta atividade foi **membership de roles**.

Compreendi que um privilégio não precisa ser concedido diretamente para cada usuário.

Em vez de fazer:

```text
appreader → SELECT
```

diretamente, podemos organizar os privilégios em uma role de grupo:

```text
app_readonly
    |
    └── SELECT em app.pessoas
```

e depois colocar o usuário como membro:

```text
appreader
    |
    └── membro de app_readonly
```

Assim, o usuário recebe os privilégios da role de grupo por meio da membership.

Também compreendi a diferença entre:

**Role de grupo:**

`app_readonly`

* `NOLOGIN`;
* organiza privilégios;
* possui `SELECT` em `app.pessoas`.

**Role de login:**

`appreader`

* possui `LOGIN`;
* representa uma conta que pode acessar o PostgreSQL;
* recebe os privilégios de `app_readonly` por membership.

## Estrutura final

A configuração ficou:

```text
app_readonly
      |
      +── CONNECT → appdb
      |
      +── USAGE → schema app
      |
      +── SELECT → app.pessoas
      |
      ↓
   membership
      ↓
 appreader
      |
      +── LOGIN
      |
      +── SELECT → permitido
      +── INSERT → não permitido
      +── UPDATE → não permitido
      +── DELETE → não permitido
```

## Conclusão

A atividade foi concluída com sucesso.

Compreendi na prática como uma role de login pode receber privilégios de uma role de grupo por meio de membership e herança.

Também ficou clara a vantagem de separar **contas de usuários** de **grupos de privilégios**. Dessa forma, os privilégios podem ser administrados na role de grupo e compartilhados com os usuários que forem membros dela, evitando a necessidade de conceder individualmente os mesmos privilégios para cada conta.






## Quinzena 11 — Atividade 5: Removendo privilégios e membership

**Data:** 07/10/2026

### Objetivo
Praticar a remoção e restauração de uma membership entre roles no PostgreSQL, verificando na prática o que acontece com os privilégios herdados por um usuário.

Também foi estudada a diferença entre remover um privilégio, remover uma membership e remover uma role.

## Atividade realizada

Primeiramente, verifiquei a membership existente entre `appreader` e `app_readonly`.

A relação existente era:

```text
appreader
    |
    | membro de
    v
app_readonly
    |
    +── SELECT → app.pessoas

```

A role `appreader` recebia os privilégios de leitura da role `app_readonly` por meio dessa membership.

Antes de realizar a remoção, testei o acesso de `appreader` à tabela:

```sql
SELECT * FROM app.pessoas;

```

O `SELECT` funcionou normalmente, confirmando que o usuário possuía acesso à tabela através da role de grupo.

### Remoção da membership

O próximo passo foi remover a membership de `appreader` em `app_readonly`:

```sql
REVOKE app_readonly FROM appreader;

```

Esse comando não remove a role `app_readonly` e também não remove os privilégios existentes nela.

Ele apenas remove a relação de membership entre as duas roles:

```text
appreader
    X
app_readonly

```

Depois da remoção, verifiquei novamente a membership e confirmei que `appreader` não fazia mais parte de `app_readonly`.

### Teste do acesso após a remoção

Após remover a membership, realizei novamente o teste:

```sql
SELECT * FROM app.pessoas;

```

O acesso foi negado.

Isso demonstrou na prática que `appreader` deixou de receber os privilégios de `app_readonly`.

A estrutura passou a ser:

```text
appreader
    |
    X  membership removida
    |
app_readonly
    |
    +── SELECT → app.pessoas

```

Mesmo com o `SELECT` continuando existente em `app_readonly`, `appreader` não conseguia mais utilizá-lo porque não fazia mais parte da role.

### Verificação do privilégio

Depois da remoção da membership, também verifiquei os privilégios existentes em `app_readonly`.

Foi possível confirmar que o privilégio `SELECT` continuava associado à role:

```text
app_readonly

```

Isso foi importante para entender que o comando:

```sql
REVOKE app_readonly FROM appreader;

```

removeu a membership, e não o privilégio `SELECT`.

### Restauração da membership

Depois de realizar os testes, restaurei a membership utilizando:

```sql
GRANT app_readonly TO appreader;

```

Esse comando fez novamente com que `appreader` se tornasse membro de `app_readonly`.

A relação voltou a ser:

```text
appreader
    |
    | membro de
    v
app_readonly
    |
    +── SELECT → app.pessoas

```

Em seguida, testei novamente:

```sql
SELECT * FROM app.pessoas;

```

O acesso voltou a funcionar, confirmando que os privilégios da role `app_readonly` foram novamente herdados por `appreader`.

## Diferença entre privilégio, membership e role

Durante a atividade, compreendi que existem diferenças importantes entre essas operações.

### Remover um privilégio

Para remover um privilégio específico, podemos utilizar:

```sql
REVOKE SELECT ON TABLE app.pessoas FROM app_readonly;

```

Nesse caso, estamos removendo o privilégio `SELECT` da role `app_readonly`.

A role continua existindo e sua membership com outras roles também continua existindo.

### Remover uma membership

Para remover a participação de `appreader` em `app_readonly`, utilizamos:

```sql
REVOKE app_readonly FROM appreader;

```

Nesse caso, o privilégio `SELECT` continua existindo em `app_readonly`, mas `appreader` deixa de recebê-lo por meio da membership.

### Remover uma role

Também pesquisei o comando:

```sql
DROP ROLE appreader;

```

Esse comando é diferente dos anteriores, pois remove a própria role do PostgreSQL.

O comando `DROP ROLE` não foi executado nesta atividade, conforme solicitado no enunciado.

## O que aprendi

O principal conceito aprendido nesta atividade foi a diferença entre privilégios e membership.

Compreendi que uma role pode receber privilégios de outra role sem que esses privilégios sejam concedidos diretamente a ela.

No caso estudado:

```text
appreader
    |
    | membro de
    v
app_readonly
    |
    +── SELECT → app.pessoas

```

Quando a membership foi removida:

```sql
REVOKE app_readonly FROM appreader;

```

o privilégio `SELECT` continuou existindo em `app_readonly`, mas `appreader` deixou de recebê-lo.

Quando a membership foi restaurada:

```sql
GRANT app_readonly TO appreader;

```

o acesso voltou a funcionar.

Também compreendi que remover um privilégio é diferente de remover uma membership.

O `REVOKE` de um privilégio remove uma autorização específica, enquanto o `REVOKE` de uma membership remove a relação entre duas roles.

Já o `DROP ROLE` remove a própria role do PostgreSQL.

### Estrutura final

Após restaurar a membership, a configuração voltou a ser:

```text
app_readonly
      |
      +── CONNECT → appdb
      |
      +── USAGE → schema app
      |
      +── SELECT → app.pessoas
      |
      ↓
   membership
      ↓
 appreader
      |
      +── LOGIN
      |
      +── SELECT → permitido
      +── INSERT → não permitido
      +── UPDATE → não permitido
      +── DELETE → não permitido

```

## Conclusão

A atividade foi concluída com sucesso.

Foi possível compreender na prática que remover uma membership não remove os privilégios da role de grupo. A operação apenas impede que o usuário continue herdando esses privilégios.

Também ficou clara a diferença entre:

* **privilégio**: define o que uma role pode fazer;
* **membership**: define de qual role/grupo uma role faz parte;
* **role**: representa a própria entidade dentro do PostgreSQL.

A atividade ajudou a compreender melhor como o PostgreSQL pode organizar permissões por meio de roles de grupo, permitindo adicionar ou remover o acesso dos usuários sem precisar alterar individualmente os privilégios armazenados na role de grupo.

```

```




# Quinzena 11 — Atividade 6: Investigando privilégios excessivos

**Data:** 08/10/2026

## Objetivo

Investigar os privilégios que o usuário `appreader` possui no PostgreSQL, identificando quais privilégios são concedidos diretamente ao usuário e quais são recebidos por meio de uma role de grupo.

O objetivo também foi verificar se o `appreader` possuía privilégios além dos necessários sobre a tabela `app.pessoas`, principalmente `INSERT`, `UPDATE` e `DELETE`, e corrigir a configuração caso fossem encontrados privilégios excessivos.

---

## Atividade realizada

Inicialmente, entrei no banco de dados `appdb` utilizando:

```bash
psql -d appdb
```

Em seguida, confirmei o usuário e o banco de dados atual:

```sql
SELECT current_user, current_database();
```

O resultado mostrou que estava conectado como `postgres` no banco `appdb`.

Depois, investiguei os atributos da role `appreader`, verificando se ela poderia fazer login, se herdava privilégios e se possuía privilégios administrativos.

O resultado mostrou que:

* `appreader` pode fazer login;
* possui `rolinherit` habilitado;
* não é superusuário;
* não pode criar bancos de dados;
* não pode criar outras roles.

Isso demonstrou que o `appreader` é um usuário comum, sem privilégios administrativos desnecessários.

---

## Investigação da membership

Depois, verifiquei de quais roles o `appreader` era membro:

```sql
SELECT
    member.rolname AS membro,
    parent.rolname AS role_grupo
FROM pg_auth_members m
JOIN pg_roles parent ON parent.oid = m.roleid
JOIN pg_roles member ON member.oid = m.member
WHERE member.rolname = 'appreader';
```

O resultado mostrou:

```text
appreader | app_readonly
```

Com isso, foi possível identificar que o `appreader` é membro da role de grupo `app_readonly`.

Portanto, parte dos privilégios do `appreader` não é concedida diretamente a ele, mas é herdada por meio dessa membership.

---

## Investigação dos privilégios da role `app_readonly`

Verifiquei os privilégios que a role `app_readonly` possui sobre a tabela `app.pessoas`.

O resultado mostrou apenas:

```text
app_readonly | app | pessoas | SELECT
```

Isso significa que a role `app_readonly` possui somente o privilégio de consulta (`SELECT`) sobre a tabela.

Não foram encontrados privilégios de:

* `INSERT`;
* `UPDATE`;
* `DELETE`.

---

## Investigação dos privilégios diretos do `appreader`

Também foi verificado se existiam privilégios concedidos diretamente ao `appreader` sobre a tabela `app.pessoas`.

O resultado foi:

```text
(0 linha)
```

Isso demonstrou que o `appreader` não possui privilégios de tabela concedidos diretamente.

O acesso à tabela ocorre por meio da membership na role `app_readonly`.

---

## Verificação dos privilégios efetivos

Para descobrir o que o `appreader` realmente consegue fazer, utilizei:

```sql
SET ROLE appreader;
```

Depois, consultei os privilégios efetivos:

```sql
SELECT
    current_user,
    has_table_privilege(current_user, 'app.pessoas', 'SELECT') AS pode_select,
    has_table_privilege(current_user, 'app.pessoas', 'INSERT') AS pode_insert,
    has_table_privilege(current_user, 'app.pessoas', 'UPDATE') AS pode_update,
    has_table_privilege(current_user, 'app.pessoas', 'DELETE') AS pode_delete;
```

O resultado foi:

```text
appreader | t | f | f | f
```

Isso demonstrou que o `appreader` possui:

* `SELECT` → permitido;
* `INSERT` → negado;
* `UPDATE` → negado;
* `DELETE` → negado.

Também foi verificado que o usuário possui `CONNECT` no banco `appdb`, `USAGE` no schema `app` e `SELECT` na tabela `app.pessoas`.

---



## O que aprendi

Nesta atividade aprendi que não basta verificar apenas os privilégios diretamente atribuídos a um usuário.

Um usuário pode receber privilégios por meio de uma **membership** em outra role.

Também aprendi a diferença entre:

* **privilégios diretos** → concedidos diretamente ao usuário;
* **privilégios herdados** → recebidos por meio de uma role da qual o usuário é membro;
* **privilégios efetivos** → aquilo que o usuário realmente consegue fazer no banco.

Aprendi também a utilizar `has_table_privilege()` para verificar os privilégios efetivos sobre uma tabela.

A investigação mostrou que o `appreader` não possuía privilégios excessivos. Por isso, não foi necessário executar nenhum `REVOKE`.

Isso também demonstrou a importância de investigar antes de alterar as permissões, evitando remover ou adicionar privilégios sem necessidade.

---

## Estrutura final

```text
appreader
    |
    | membership
    ↓
app_readonly
    |
    └── SELECT
          |
          ↓
     app.pessoas
```

Privilégios efetivos:

```text
appreader
    |
    +-- CONNECT no appdb       ✓
    |
    +-- USAGE no schema app    ✓
    |
    +-- SELECT em pessoas     ✓
    |
    X-- INSERT                 ✗
    X-- UPDATE                 ✗
    X-- DELETE                 ✗
```

---

## Conclusão

A atividade foi concluída realizando a investigação dos privilégios do usuário `appreader`, sua membership e a origem dos seus acessos.

Foi identificado que o `appreader` recebe o privilégio de `SELECT` por meio da role `app_readonly` e não possui privilégios diretos sobre a tabela.

Os testes confirmaram que o usuário consegue consultar os dados, mas não consegue inserir, alterar ou excluir registros.

Como a configuração encontrada já estava de acordo com o princípio do menor privilégio, **não foi necessário realizar nenhuma alteração de permissões**.

A atividade permitiu compreender melhor o funcionamento da herança de privilégios, das roles de grupo e da diferença entre privilégios concedidos diretamente e privilégios efetivos.




Aqui está o seu texto formatado em Markdown com a correção da tabela, blocos de código SQL e hierarquia de títulos ajustados, pronto para você copiar e atualizar no seu repositório ou diário de bordo:


## Quinzena 11 — Atividade 7: Desafio final — Desenhando um modelo de acesso

**Data:** 09/10/2026

### Objetivo
Criar um modelo de controle de acesso no PostgreSQL utilizando roles de grupo e roles de login, separando os privilégios de usuários que precisam apenas consultar informações daqueles que precisam inserir, alterar e excluir registros.

O objetivo foi aplicar o conceito de RBAC (Role-Based Access Control), organizar as memberships, testar os diferentes perfis de acesso e garantir que cada usuário possua somente os privilégios necessários para sua função.

## Atividade realizada

Inicialmente, entrei no banco de dados `appdb` e verifiquei quais das roles necessárias já existiam no PostgreSQL.

Utilizei a consulta à visão `pg_roles` para verificar os nomes das roles, se podiam fazer login e se herdavam privilégios.

Identifiquei que a role `app_readonly`, criada nas atividades anteriores, já existia. Por isso, não foi necessário criá-la novamente.

Em seguida, criei a role de grupo `app_operator` utilizando:

```sql
CREATE ROLE app_operator NOLOGIN;

```

O atributo `NOLOGIN` foi utilizado porque essa role representa um grupo de permissões, e não um usuário que precisa se autenticar diretamente.

### Configuração dos privilégios das roles de grupo

Concedi à role `app_operator` o privilégio `CONNECT` no banco `appdb`, permitindo que seus membros pudessem se conectar ao banco.

Depois, concedi `USAGE` no schema `app`, permitindo o acesso aos objetos do schema, desde que os privilégios necessários também estivessem disponíveis.

Por fim, concedi os privilégios `SELECT`, `INSERT`, `UPDATE` e `DELETE` sobre a tabela `app.pessoas`.

A consulta à visão `information_schema.role_table_grants` confirmou que:

* `app_readonly` possui somente `SELECT` na tabela `app.pessoas`;
* `app_operator` possui `SELECT`, `INSERT`, `UPDATE` e `DELETE` na mesma tabela.

Dessa forma, os dois grupos ficaram configurados com permissões diferentes, de acordo com suas funções.

### Criação dos usuários e configuração das memberships

Criei as duas roles de login:

```sql
CREATE ROLE usuario_leitura LOGIN;
CREATE ROLE usuario_operacao LOGIN;

```

O atributo `LOGIN` permite que esses usuários sejam utilizados para autenticação no PostgreSQL.

Em seguida, defini as senhas com o comando interno `\password`, sem precisar colocar as senhas diretamente em comandos SQL.

Depois, configurei as memberships:

```sql
GRANT app_readonly TO usuario_leitura;
GRANT app_operator TO usuario_operacao;

```

Esses comandos associaram cada usuário à role de grupo correspondente, permitindo que os usuários herdassem os privilégios dos seus respectivos grupos.

A consulta à visão `pg_auth_members`, relacionada à `pg_roles`, confirmou as seguintes associações:

* `usuario_leitura` é membro de `app_readonly`;
* `usuario_operacao` é membro de `app_operator`.

## Testes realizados

### Teste do usuário de leitura

Conectei-me ao banco `appdb` utilizando o usuário `usuario_leitura`.

Confirmei a identidade e o banco atual com:

```sql
SELECT current_user, session_user, current_database();

```

A consulta retornou `usuario_leitura` como usuário atual e de sessão, conectado ao banco `appdb`.

Em seguida, executei:

```sql
SELECT * FROM app.pessoas;

```

A consulta funcionou e retornou os registros da tabela.

Depois, tentei executar `INSERT`, `UPDATE` e `DELETE`. As três operações foram negadas pelo PostgreSQL com a mensagem de permissão negada para a tabela pessoas.

Esses resultados demonstraram que o usuário consegue consultar os dados, mas não consegue inserir, alterar ou excluir registros.

### Teste do usuário de operação

Conectei-me ao banco `appdb` utilizando o usuário `usuario_operacao`.

Confirmei sua identidade e executei um `SELECT` na tabela `app.pessoas`, que funcionou normalmente.

Em seguida, executei um `INSERT` para inserir um registro de teste. O PostgreSQL confirmou a operação com o resultado `INSERT 0 1`, indicando que uma linha foi inserida.

También realizei testes de `UPDATE` e `DELETE` dentro de transações, utilizando `BEGIN` e `ROLLBACK` para verificar as operações e desfazer as alterações de teste.

O `ROLLBACK` permitiu retornar ao estado anterior à transação, evitando que as alterações realizadas durante esses testes permanecessem gravadas.

### Tabela de privilégios

Com base na configuração e nos testes realizados, o modelo esperado de acesso ficou assim:

| Role | SELECT | INSERT | UPDATE | DELETE |
| --- | --- | --- | --- | --- |
| `usuario_leitura` | Permitido | Negado | Negado | Negado |
| `usuario_operacao` | Permitido | Permitido | Permitido | Permitido |

Os resultados dos testes de `SELECT` e de todas as operações de escrita confirmaram a separação entre os dois perfis de acesso.

## O que aprendi

* Aprendi a desenhar uma estrutura de acesso antes de executar os comandos, identificando quem precisa acessar o banco, qual grupo representa sua função, quais privilégios esse grupo deve possuir e sobre quais objetos eles serão aplicados.
* Compreendi a diferença entre uma role de login e uma role de grupo. A primeira representa uma identidade utilizada para autenticação, enquanto a segunda permite organizar privilégios de forma centralizada.
* Compreendi que `GRANT` pode ser utilizado tanto para conceder privilégios sobre objetos quanto para estabelecer memberships entre roles.
* Aprendi que os privilégios podem ser herdados por meio de uma membership e que não é necessário conceder os mesmos privilégios diretamente a cada usuário quando a estrutura de grupos está corretamente configurada.
* Os testes demonstraram a importância de verificar não apenas se uma operação funciona, mas também se as operações que deveriam ser proibidas são realmente negadas.
* Aprendi a utilizar transações com `BEGIN` e `ROLLBACK` para testar alterações com segurança no ambiente de laboratório.

### Estrutura final

```text
PostgreSQL
    |
    +-- appdb
          |
          +-- app
                |
                +-- pessoas


app_readonly
    |
    +-- CONNECT em appdb
    +-- USAGE em app
    +-- SELECT em app.pessoas
    |
    +-- usuario_leitura


app_operator
    |
    +-- CONNECT em appdb
    +-- USAGE em app
    +-- SELECT em app.pessoas
    +-- INSERT em app.pessoas
    +-- UPDATE em app.pessoas
    +-- DELETE em app.pessoas
    |
    +-- usuario_operacao

```

## Conclusão

A atividade foi concluída com a criação de um modelo de controle de acesso baseado em roles de grupo e roles de login.

O usuário `usuario_leitura` ficou associado à role `app_readonly`, com acesso de consulta à tabela `app.pessoas`, enquanto o usuário `usuario_operacao` ficou associado à role `app_operator`, com privilégios de consulta, inserção, alteração e exclusão.

Os testes demonstraram como a separação de responsabilidades permite que diferentes usuários acessem a mesma tabela com permissões distintas.

A atividade consolidou os conceitos de roles, memberships, herança de privilégios, `GRANT`, autenticação e menor privilégio, mostrando como organizar o acesso de maneira mais segura e fácil de administrar.

```

```
