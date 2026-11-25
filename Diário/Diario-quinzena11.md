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


