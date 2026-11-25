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
