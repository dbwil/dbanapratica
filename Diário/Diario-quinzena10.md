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


