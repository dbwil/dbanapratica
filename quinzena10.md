# dbanapratica

# Quinzena 10 - Conhecendo os objetos do PostgreSQL

**Tema:** Schemas, tabelas, tipos de dados, constraints e operações básicas

**Sistema Operacional:** Rocky Linux

**Dedicação:** aproximadamente 30 minutos por dia

---

# Objetivo da quinzena

Na Quinzena 9 você começou a entender como o PostgreSQL controla o acesso:

```text
conexão
   |
   v
pg_hba.conf
   |
   v
autenticação
   |
   v
role
   |
   v
database
   |
   v
permissões
```

Agora vamos avançar um nível.

Você já sabe criar um database e uma role. Agora precisa começar a entender o que existe **dentro de um database**.

Nesta quinzena vamos introduzir:

- schemas;
- tabelas;
- colunas;
- tipos de dados;
- chaves primárias;
- constraints;
- inserção de dados;
- consultas simples;
- alteração e remoção de dados;
- permissões sobre tabelas.

O objetivo não é transformar esta quinzena em um curso de SQL.

O objetivo é fazer você compreender a estrutura básica de um banco PostgreSQL e começar a enxergar as relações entre:

```text
Instância
   |
   +-- Database
         |
         +-- Schema
               |
               +-- Table
                     |
                     +-- Columns
                     |
                     +-- Rows
```

Ao final da quinzena você deverá conseguir criar uma pequena estrutura de banco, inserir dados, consultá-los e administrar o acesso de uma role a uma tabela.

---

# Como estudar

Não é necessário memorizar os comandos SQL.

Para cada atividade:

1. Leia o material relacionado ao assunto.
2. Pesquise utilizando as palavras-chave fornecidas.
3. Tente executar o procedimento no ambiente de laboratório.
4. Observe o resultado de cada comando.
5. Quando encontrar um erro, tente entender o motivo.
6. Registre as descobertas no `diario.md`.

Nesta quinzena haverá mais contato com SQL, mas o foco continua sendo a formação de um administrador PostgreSQL.

Você não precisa se tornar desenvolvedor.

O importante é conseguir entender a estrutura do banco e administrar os objetos existentes.

---

# Material de apoio

## Documentação principal

PostgreSQL Documentation:

https://www.postgresql.org/docs/current/

Tutorial oficial:

https://www.postgresql.org/docs/current/tutorial.html

Creating a Table:

https://www.postgresql.org/docs/current/ddl-basics.html

CREATE TABLE:

https://www.postgresql.org/docs/current/sql-createtable.html

Schemas:

https://www.postgresql.org/docs/current/ddl-schemas.html

Privileges:

https://www.postgresql.org/docs/current/ddl-priv.html

---

# Conteúdos específicos para esta quinzena

Utilize principalmente as seguintes partes do tutorial oficial:

- Creating a New Table
- Populating a Table With Rows
- Querying a Table
- Updates
- Deletions

Não é necessário estudar todo o tutorial.

O objetivo é consultar somente os assuntos relacionados às atividades.

---

# Material complementar

Para conteúdos em português, pesquise no YouTube por:

- PostgreSQL schemas
- PostgreSQL criar tabela
- PostgreSQL tipos de dados
- PostgreSQL chave primária
- PostgreSQL constraints
- PostgreSQL INSERT SELECT UPDATE DELETE
- PostgreSQL permissões em tabelas

Não é necessário assistir a vários vídeos sobre o mesmo assunto.

Escolha um material que você consiga acompanhar e utilize a documentação oficial para complementar o estudo.

---

# Diário de bordo

Continue utilizando:

`diario.md`

Para cada atividade registre:

- Data
- Atividade
- O que precisava fazer
- O que pesquisou
- Dificuldades encontradas
- Como resolveu
- O que aprendeu
- Links consultados

Quando encontrar um erro interessante, registre também:

- a mensagem de erro;
- o que você entendeu sobre o erro;
- como resolveu.

---

# Atividade 1 - O que existe dentro de um database?

**Nível:** Básico

**Dificuldade:** Baixa

**Tempo estimado:** 30 minutos

---

## Enunciado

Na quinzena anterior você criou o database `appdb`.

Agora precisamos descobrir o que existe dentro dele.

Entre no `appdb` e investigue sua estrutura.

Você deverá descobrir:

- quais schemas existem;
- qual é o schema `public`;
- quais objetos existem atualmente;
- como listar tabelas;
- como descobrir em qual schema uma tabela está;
- como descobrir quais tabelas pertencem a determinado usuário;
- como identificar o schema utilizado por padrão em uma sessão.

No final, explique com suas próprias palavras:

> Qual é a diferença entre um database e um schema?

Não crie nenhuma tabela ainda.

O objetivo desta atividade é construir o modelo mental da estrutura interna do database.

---

## Dica

Não confunda:

```text
database
schema
table
```

Eles representam níveis diferentes da organização do PostgreSQL.

Tente descobrir isso utilizando o `psql` e a documentação.

---

## Palavras-chave para pesquisa

- PostgreSQL schema
- PostgreSQL public schema
- PostgreSQL list schemas
- PostgreSQL list tables
- PostgreSQL search_path
- PostgreSQL database schema difference

---

## Ao concluir esta atividade você deverá ser capaz de:

- listar os schemas de um database;
- identificar o schema `public`;
- listar tabelas;
- explicar a diferença entre database e schema;
- compreender o papel básico do `search_path`.

---

## Checklist

- [ ] `appdb` acessado.
- [ ] Schemas listados.
- [ ] `public` identificado.
- [ ] Tabelas existentes verificadas.
- [ ] Diferença entre database e schema compreendida.
- [ ] `search_path` pesquisado.
- [ ] Diário de bordo atualizado.

---

# Atividade 2 - Criando uma estrutura para a aplicação

**Nível:** Intermediário

**Dificuldade:** Moderada

**Tempo estimado:** 45 minutos

---

## Enunciado

A aplicação `appdb` precisa armazenar informações sobre pessoas.

Você deverá criar um schema chamado:

`app`

Dentro dele deverá existir uma tabela chamada:

`pessoas`

A tabela deverá armazenar, no mínimo:

- um identificador;
- nome;
- e-mail;
- data de nascimento.

Antes de criar a tabela, pesquise:

- o que é uma coluna;
- o que é um tipo de dado;
- quais tipos podem ser utilizados para números inteiros;
- quais tipos podem ser utilizados para texto;
- qual tipo representa uma data;
- o que é uma chave primária;
- por que uma tabela deve possuir uma identificação única para seus registros.

Escolha os tipos de dados que considerar adequados e justifique suas escolhas no diário de bordo.

Não copie simplesmente a estrutura de um exemplo encontrado na Internet.

Você deverá conseguir explicar por que escolheu cada tipo.

---

## Dica

Você encontrará vários tipos de dados possíveis para armazenar texto, números e datas.

Não tente estudar todos.

Concentre-se inicialmente nos tipos necessários para construir esta tabela.

---

## Palavras-chave para pesquisa

- PostgreSQL data types
- PostgreSQL integer
- PostgreSQL text varchar
- PostgreSQL date
- PostgreSQL primary key
- PostgreSQL create table
- PostgreSQL schema create

---

## Ao concluir esta atividade você deverá ser capaz de:

- criar um schema;
- criar uma tabela;
- escolher tipos de dados básicos;
- criar uma chave primária;
- explicar a finalidade das colunas;
- justificar escolhas simples de modelagem.

---

## Checklist

- [ ] Schema `app` criado.
- [ ] Tabela `pessoas` criada.
- [ ] Colunas definidas.
- [ ] Tipos de dados escolhidos.
- [ ] Chave primária criada.
- [ ] Estrutura da tabela verificada.
- [ ] Escolhas documentadas.
- [ ] Diário de bordo atualizado.

---

# Atividade 3 - Constraints: fazendo o banco ajudar

**Nível:** Intermediário

**Dificuldade:** Moderada

**Tempo estimado:** 45 minutos

---

## Enunciado

A tabela `pessoas` já existe.

Agora imagine que a aplicação começou a receber dados incorretos.

Uma pessoa foi cadastrada sem nome.

Outra possui um e-mail repetido.

Além disso, alguém tentou inserir um registro sem identificador.

Você deverá pesquisar como o PostgreSQL pode ajudar a impedir esses problemas.

Investigue:

- `PRIMARY KEY`;
- `NOT NULL`;
- `UNIQUE`;
- `CHECK`;
- `DEFAULT`.

Depois analise a tabela `pessoas` e determine quais dessas constraints fazem sentido para ela.

Aplique as constraints que considerar adequadas.

Depois tente inserir registros que violem cada uma delas.

Observe as mensagens de erro.

Para cada teste registre:

- o que tentou fazer;
- qual constraint foi acionada;
- qual erro apareceu;
- por que o PostgreSQL recusou a operação.

---

## Dica

Uma constraint não existe apenas para "dar erro".

Ela representa uma regra que os dados precisam obedecer.

Tente pensar:

> Qual regra de negócio estou tentando proteger?

---

## Palavras-chave para pesquisa

- PostgreSQL constraints
- PostgreSQL PRIMARY KEY
- PostgreSQL NOT NULL
- PostgreSQL UNIQUE
- PostgreSQL CHECK constraint
- PostgreSQL DEFAULT

---

## Ao concluir esta atividade você deverá ser capaz de:

- explicar o objetivo de uma constraint;
- compreender `PRIMARY KEY`;
- compreender `NOT NULL`;
- compreender `UNIQUE`;
- compreender `CHECK`;
- compreender `DEFAULT`;
- interpretar erros causados por constraints.

---

## Checklist

- [ ] `PRIMARY KEY` compreendida.
- [ ] `NOT NULL` compreendida.
- [ ] `UNIQUE` compreendida.
- [ ] `CHECK` pesquisada.
- [ ] `DEFAULT` pesquisado.
- [ ] Constraints aplicadas à tabela.
- [ ] Tentativas inválidas realizadas.
- [ ] Mensagens de erro analisadas.
- [ ] Diário de bordo atualizado.

---

# Atividade 4 - Inserindo e consultando dados

**Nível:** Intermediário

**Dificuldade:** Moderada

**Tempo estimado:** 45 minutos

---

## Enunciado

Agora a estrutura existe, mas uma tabela sem dados não é muito útil.

Você deverá inserir alguns registros na tabela `app.pessoas`.

Crie pelo menos:

- 5 registros válidos;
- 1 tentativa de inserir um registro inválido;
- 1 tentativa de inserir um registro que viole uma constraint.

Depois pesquise como consultar os dados.

Você deverá realizar consultas que permitam:

- listar todos os registros;
- selecionar somente algumas colunas;
- filtrar registros;
- ordenar resultados;
- contar registros.

Depois responda:

- Qual a diferença entre `SELECT *` e selecionar colunas específicas?
- Para que serve `WHERE`?
- Para que serve `ORDER BY`?
- Para que serve `COUNT`?

Não é necessário estudar consultas complexas.

O objetivo é começar a ler e interpretar dados existentes no PostgreSQL.

---

## Dica

Não se preocupe em decorar SQL.

Procure entender a estrutura de uma consulta:

```text
SELECT
   |
   +-- o que quero visualizar?

FROM
   |
   +-- de onde vêm os dados?

WHERE
   |
   +-- quais registros quero?

ORDER BY
   |
   +-- como quero ordenar?
```

---

## Palavras-chave para pesquisa

- PostgreSQL INSERT
- PostgreSQL SELECT
- PostgreSQL WHERE
- PostgreSQL ORDER BY
- PostgreSQL COUNT
- PostgreSQL query

---

## Ao concluir esta atividade você deverá ser capaz de:

- inserir registros;
- consultar registros;
- selecionar colunas específicas;
- filtrar resultados;
- ordenar resultados;
- realizar uma contagem simples;
- interpretar resultados de uma consulta.

---

## Checklist

- [ ] Pelo menos 5 registros inseridos.
- [ ] Registro inválido testado.
- [ ] Constraint violada testada.
- [ ] `SELECT` utilizado.
- [ ] `WHERE` utilizado.
- [ ] `ORDER BY` utilizado.
- [ ] `COUNT` utilizado.
- [ ] Resultados analisados.
- [ ] Diário de bordo atualizado.

---

# Atividade 5 - Alterando e removendo dados

**Nível:** Intermediário

**Dificuldade:** Moderada

**Tempo estimado:** 30 minutos

---

## Enunciado

Agora você precisará corrigir informações cadastradas.

Escolha um registro da tabela `app.pessoas` e altere uma de suas informações.

Depois remova um registro específico.

Antes de executar as operações, pesquise:

- `UPDATE`;
- `DELETE`;
- `WHERE`.

Preste atenção especial ao uso do `WHERE`.

Imagine que você execute:

```text
UPDATE ...
```

ou:

```text
DELETE ...
```

sem especificar corretamente quais registros deseja alterar.

O que poderia acontecer?

Faça seus testes somente no ambiente de laboratório.

Depois explique no diário de bordo por que o `WHERE` é importante nessas operações.

---

## Dica

Antes de executar um `UPDATE` ou `DELETE`, faça primeiro uma consulta que mostre exatamente os registros que serão afetados.

A ideia é desenvolver um hábito de administração segura:

```text
consultar
   |
   v
confirmar
   |
   v
alterar
   |
   v
verificar novamente
```

---

## Palavras-chave para pesquisa

- PostgreSQL UPDATE
- PostgreSQL DELETE
- PostgreSQL WHERE
- PostgreSQL update row
- PostgreSQL delete row

---

## Ao concluir esta atividade você deverá ser capaz de:

- alterar registros;
- remover registros;
- utilizar `WHERE`;
- identificar os riscos de um `UPDATE` ou `DELETE` sem filtro;
- verificar o resultado de uma alteração.

---

## Checklist

- [ ] Registro alterado.
- [ ] Registro removido.
- [ ] `WHERE` utilizado.
- [ ] Resultado conferido.
- [ ] Importância do `WHERE` compreendida.
- [ ] Diário de bordo atualizado.

---

# Atividade 6 - Quem pode acessar a tabela?

**Nível:** Intermediário

**Dificuldade:** Desafiadora

**Tempo estimado:** 45 minutos

---

## Enunciado

Agora vamos voltar ao assunto principal da Quinzena 9: permissões.

A tabela `app.pessoas` existe e contém dados.

A role `appuser` consegue se conectar ao database.

Mas isso não significa necessariamente que ela tenha todas as permissões necessárias sobre a tabela.

Faça um teste utilizando `appuser`.

Investigue:

- `appuser` consegue consultar a tabela?
- `appuser` consegue inserir dados?
- `appuser` consegue alterar dados?
- `appuser` consegue remover dados?
- Quem é o proprietário da tabela?
- Quais privilégios `appuser` possui?
- Como consultar os privilégios de uma tabela?

Depois prepare uma configuração em que `appuser` tenha somente as permissões necessárias para trabalhar com a tabela.

Não conceda privilégios de superusuário.

Não transfira a propriedade da tabela simplesmente para fazer o acesso funcionar.

O objetivo é utilizar privilégios de forma consciente.

---

## Dica

Agora você terá duas camadas de controle trabalhando juntas:

```text
Conexão
   |
   v
pg_hba.conf
   |
   v
Autenticação
   |
   v
Database
   |
   v
Privilégios
   |
   v
Schema
   |
   v
Tabela
```

Tente descobrir em qual etapa um problema está ocorrendo antes de tentar corrigi-lo.

---

## Palavras-chave para pesquisa

- PostgreSQL table privileges
- PostgreSQL GRANT SELECT
- PostgreSQL GRANT INSERT
- PostgreSQL GRANT UPDATE
- PostgreSQL GRANT DELETE
- PostgreSQL table owner
- PostgreSQL information schema table privileges

---

## Ao concluir esta atividade você deverá ser capaz de:

- verificar privilégios de uma tabela;
- identificar o proprietário de uma tabela;
- conceder privilégios específicos;
- testar os privilégios concedidos;
- diferenciar acesso ao database de acesso a uma tabela;
- compreender o princípio de conceder somente os privilégios necessários.

---

## Checklist

- [ ] Acesso de `appuser` testado.
- [ ] Proprietário da tabela identificado.
- [ ] Privilégios existentes investigados.
- [ ] Permissões necessárias definidas.
- [ ] Privilégios concedidos.
- [ ] Permissões testadas.
- [ ] Operações não autorizadas testadas.
- [ ] Resultado documentado.
- [ ] Diário de bordo atualizado.

---

# Entregável final - Pequeno banco de laboratório

Ao final da quinzena, o ambiente deverá conter:

```text
appdb
  |
  +-- app
       |
       +-- pessoas
             |
             +-- id
             +-- nome
             +-- email
             +-- data_nascimento
```

E você deverá conseguir explicar:

- quem é o proprietário do database;
- quem é o proprietário do schema;
- quem é o proprietário da tabela;
- qual role pode acessar o database;
- qual role pode acessar o schema;
- qual role pode consultar a tabela;
- qual role pode inserir dados;
- qual role pode alterar dados;
- qual role pode remover dados.

---

# Resumo obrigatório

Escreva um texto com no máximo 20 linhas respondendo:

1. O que é um schema?
2. Qual a diferença entre database, schema e tabela?
3. O que é uma coluna?
4. O que é uma linha ou registro?
5. O que é uma chave primária?
6. Para que servem constraints?
7. Qual a diferença entre `NOT NULL` e `UNIQUE`?
8. O que é `SELECT`?
9. Para que serve `WHERE`?
10. Por que o `WHERE` é importante em `UPDATE` e `DELETE`?
11. Qual a diferença entre estar conectado a um database e possuir acesso a uma tabela?
12. O que você aprendeu sobre privilégios de tabela?
13. Qual foi a maior dificuldade encontrada?
14. O que você ainda precisa estudar melhor?

---

# Perguntas para avaliação

Durante nosso próximo encontro, esteja preparado para responder:

1. Qual a diferença entre uma instância PostgreSQL e um database?
2. Qual a diferença entre database e schema?
3. O que é o schema `public`?
4. O que é uma tabela?
5. O que é uma coluna?
6. O que é um registro?
7. O que é uma chave primária?
8. Para que serve `NOT NULL`?
9. Para que serve `UNIQUE`?
10. Para que serve `CHECK`?
11. Para que serve `DEFAULT`?
12. O que acontece quando uma operação viola uma constraint?
13. Qual a diferença entre `SELECT`, `INSERT`, `UPDATE` e `DELETE`?
14. Por que devemos ter cuidado com `UPDATE` e `DELETE`?
15. Qual a função do `WHERE`?
16. Uma role que consegue acessar um database consegue automaticamente fazer qualquer operação em todas as tabelas?
17. Como descobrir quem é o proprietário de uma tabela?
18. Como verificar os privilégios de uma tabela?
19. Qual a diferença entre ser proprietário de uma tabela e possuir um privilégio concedido?
20. Se `appuser` consegue conectar ao `appdb`, mas não consegue consultar `app.pessoas`, onde você investigaria o problema?
21. Se `appuser` consegue consultar a tabela, mas não consegue inserir dados, o que você investigaria?
22. Explique o caminho completo desde a tentativa de conexão até o acesso a uma tabela.




# Atividade 1 - O que existe dentro de um database?


1. Entrando no appdb e confirmando onde estou.
   
```bash
[postgres@localhost ~]$ psql -U appuser -d appdb
Senha para o usuário appuser: 
psql (17.11)
Digite "help" para obter ajuda.

appdb=> \conninfo
Você está conectado ao banco de dados "appdb" como usuário "appuser" via soquete em "/run/postgresql" na porta "5432".
appdb=> 
```


2. Descobrindo quais schemas existem

```bash

appdb=> \dn
     Lista de esquemas
  Nome  |       Dono        
--------+-------------------
 public | pg_database_owner
(1 linha)
```
Um schema é uma forma de organizar objetos dentro de um database. É uma divisão/namespace dentro do database.


3. Investigando especificamente o public
   
```bash

appdb=> \dn+ public
                                      Lista de esquemas
  Nome  |       Dono        |         Privilégios de acesso   
       |       Descrição        
--------+-------------------+----------------------------------------+------------------------
 public | pg_database_owner | pg_database_owner=UC/pg_database_owner+| standard public schema
        |                   | =U/pg_database_owner                   | 
(1 linha)

appdb=>

```

4. Verificando quais tabelas existem

```bash
appdb=> \dt
Não foi encontrada nenhuma relação.
appdb=> 

```

Como eu ainda não criei nenhuma tabela na atividade apareceu esta informação: (Não foi encontrada nenhuma relação).

5. Verificando as tabelas em qualquer schema

```bash
appdb=> \d *.*
                          Visão "information_schema._pg_foreign_data_wrappers"
            Coluna             |               Tipo                | Ordenação | Pode ser nulo | Padrão 
-------------------------------+-----------------------------------+-----------+---------------+--------
 oid                           | oid                               |           |               | 
 fdwowner                      | oid                               |           |               | 
 fdwoptions                    | text[]                            | C         |               | 
 foreign_data_wrapper_catalog  | information_schema.sql_identifier |           |               | 
 foreign_data_wrapper_name     | information_schema.sql_identifier |           |               | 
 authorization_identifier      | information_schema.sql_identifier |           |               | 
 foreign_data_wrapper_language | information_schema.character_data |           |               | 

                            Visão "information_schema._pg_foreign_servers"
            Coluna            |               Tipo                | Ordenação | Pode ser nulo | Padrão 
------------------------------+-----------------------------------+-----------+---------------+--------
 oid                          | oid                               |           |               | 
 srvoptions                   | text[]                            | C         |               | 
 foreign_server_catalog       | information_schema.sql_identifier |           |               | 
 foreign_server_name          | information_schema.sql_identifier |           |               | 
 foreign_data_wrapper_catalog | information_schema.sql_identifier |           |               | 
 foreign_data_wrapper_name    | information_schema.sql_identifier |           |               | 
 foreign_server_type          | information_schema.character_data |           |               | 
 foreign_server_version       | information_schema.character_data |           |               | 
 authorization_identifier     | information_schema.sql_identifier |           |               | 

```
Apareceu uma lista maior, porque com este comando posso encontrar objetos de outros schemas.




6. Descobrindo quais objetos existem

```bash

appdb=> \d 
Não foi encontrada nenhuma relação.
appdb=> 

```
O appdb ainda está vazio, por isso apareceu (Não foi encontrada nenhuma relação).




7. Descobrindo o search_path

```bash

appdb=> SHOW search_path;
   search_path   
-----------------
 "$user", public
(1 linha)

appdb=> 

```


8. Descobrindo quais tabelas pertencem a appuser

```bash
appdb=> SELECT schemaname, tablename, tableowner
FROM pg_tables
WHERE tableowner = 'appuser';
 schemaname | tablename | tableowner 
------------+-----------+------------
(0 linha)

appdb=> 
```

Como eu ainda não criei nenhuma tabela, o resultado é (0 linha).




# Atividade 2 - Criando uma estrutura para a aplicação

1. Confirmando que estou no banco correto

```bash
appdb=> \conninfo
Você está conectado ao banco de dados "appdb" como usuário "appuser" via soquete em "/run/postgresql" na porta "5432".
appdb=> 
```


2. Criando o schema app

```bash

appdb=> CREATE SCHEMA app;
CREATE SCHEMA
appdb=> 
```
3. Confirmando que o schema realmente existe

```bash
appdb=> \dn
     Lista de esquemas
  Nome  |       Dono        
--------+-------------------
 app    | appuser
 public | pg_database_owner
(2 linhas)

appdb=> 

```

4. Criando a tabela pessoas

   
```bash

appdb=> CREATE TABLE app.pessoas (
appdb(> id integer GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
appdb(> nome text,
appdb(> email text,
appdb(> data_nascimento date
appdb(> );
CREATE TABLE
appdb=> 
   
```

5. Verificando se a tabela foi criada

```bash

appdb=> \dt app.*
          Lista de relações
 Esquema |  Nome   |  Tipo  |  Dono   
---------+---------+--------+---------
 app     | pessoas | tabela | appuser
(1 linha)

appdb=> 
```

6. Vendo a estrutura da tabela

```bash

appdb=> \d app.pessoas
                                 Tabela "app.pessoas"
     Coluna      |  Tipo   | Ordenação | Pode ser nulo |            Padrã
o            
-----------------+---------+-----------+---------------+-----------------
-------------
 id              | integer |           | not null      | generated always
 as identity
 nome            | text    |           |               | 
 email           | text    |           |               | 
 data_nascimento | date    |           |               | 
Índices:
    "pessoas_pkey" PRIMARY KEY, btree (id)

appdb=> 

```

# Atividade 3 - Constraints: fazendo o banco ajudar

1. Verificando a estrutura atual

```bash
appdb=> \d app.pessoas
                                 Tabela "app.pessoas"
     Coluna      |  Tipo   | Ordenação | Pode ser nulo |            Padr
ão            
-----------------+---------+-----------+---------------+----------------
--------------
 id              | integer |           | not null      | generated alway
s as identity
 nome            | text    |           |               | 
 email           | text    |           |               | 
 data_nascimento | date    |           |               | 
Índices:
    "pessoas_pkey" PRIMARY KEY, btree (id)

```

2. Tornando nome obrigatório adicionando NOT NULL.

```bash
appdb=> ALTER TABLE  app.pessoas
appdb-> ALTER COLUMN nome SET NOT NULL;
ALTER TABLE
appdb=> 
```

3. Tornando o e-mail único
```bash
appdb=> ALTER TABLE app.pessoas
appdb-> ADD CONSTRAINT pessoas_email_unique UNIQUE (email);
ALTER TABLE
appdb=> 
```

4. Criando a regra da data
```bash
appdb=> ALTER TABLE app.pessoas
appdb-> ADD CONSTRAINT pessoas_data_nascimento_check
appdb-> CHECK (data_nascimento <= CURRENT_DATE);
ALTER TABLE
appdb=> 
```

5. Verificando as novas regras
```bash
appdb=> \d app.pessoas
                                 Tabela "app.pessoas"
     Coluna      |  Tipo   | Ordenação | Pode ser nulo |            Pa
drão            
-----------------+---------+-----------+---------------+--------------
----------------
 id              | integer |           | not null      | generated alw
ays as identity
 nome            | text    |           | not null      | 
 email           | text    |           |               | 
 data_nascimento | date    |           |               | 
Índices:
    "pessoas_pkey" PRIMARY KEY, btree (id)
    "pessoas_email_unique" UNIQUE CONSTRAINT, btree (email)
Restrições de verificação:
    "pessoas_data_nascimento_check" CHECK (data_nascimento <= CURRENT_DATE)

appdb=> 
```

 6. TESTE 1 — Violando NOT NULL
```bash
appdb=> INSERT INTO app.pessoas (email, data_nascimento)
VALUES ('teste_null@email.com', '1990-01-01');
ERRO:  o valor nulo na coluna "nome" da relação "pessoas" viola a restrição de não-nulo
DETALHE:  Registro que falhou contém (1, null, teste_null@email.com, 1990-01-01).
appdb=> 
```

7. TESTE 2 — Inserir um registro válido
```bash
appdb=> ^[[200~INSERT INTO app.pessoas (nome, email, data_nascimento)
appdb-> VALUES ('João Teste', 'joao.teste@email.com', '1990-01-01');~
ERRO:  erro de sintaxe em ou próximo a "
INHA 1: INSERT INTO app.pessoas (nome, email, data_nascimento)
         ^
appdb-> 
```


9. TESTE 3 — Violando CHECK
```bash
appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Pessoa Futuro', 'futuro@email.com', '2035-01-01');
ERRO:  a nova linha da relação "pessoas" viola a restrição de verificação "pessoas_data_nascimento_check"
DETALHE:  Registro que falhou contém (3, Pessoa Futuro, futuro@email.com, 2035-01-01).
appdb=> 

```

9. TESTE 4 — Violando UNIQUE
```bash
appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Carlos Teste', 'joao.teste@email.com', '1995-06-15');
ERRO:  duplicar valor da chave viola a restrição de unicidade "pessoas_email_unique"
DETALHE:  Chave (email)=(joao.teste@email.com) já existe.
appdb=> 


```

10.Ver os dados que ficaram na tabela
```bash

appdb=> SELECT * FROM app.pessoas;
 id |    nome     |        email         | data_nascimento 
----+-------------+----------------------+-----------------
  2 | Maria Teste | joao.teste@email.com | 1992-05-10
  1 | Outro João  | outro.joao@email.com | 1991-02-02
(2 linhas)

```



# Atividade 4 - Inserindo e consultando dados

1. Inserindo 5 registros válidos


- Registro 1
```bash
appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Ana Souza', 'ana.souza@email.com', '1995-03-15');
INSERT 0 1
appdb=> 
```


- Registro 2
```bash
appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Bruno Silva', 'bruno.silva@email.com', '1988-07-22');
INSERT 0 1
appdb=>
```

- Registro 3

```bash
appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Carla Oliveira', 'carla.oliveira@email.com', '2000-11-05');
INSERT 0 1
appdb=> 
```
- Registro 4
```bash

appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Daniel Santos', 'daniel.santos@email.com', '1992-01-30');
INSERT 0 1
appdb=> 
```

- Registro 5
```bash
appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento) 
VALUES ('Fernanda Costa', 'fernanda.costa@email.com', '1998-09-18');
INSERT 0 1
appdb=> 
```


2. Tentativa de registro inválido (tentativa que viole a regra NOT NULL).

Tentar inserir uma pessoa sem nome:
```bash
appdb=> INSERT INTO app.pessoas (email, data_nascimento)
VALUES ('sem.nome@email.com', '1990-05-10');
ERRO:  o valor nulo na coluna "nome" da relação "pessoas" viola a restrição de não-nulo
DETALHE:  Registro que falhou contém (11, null, sem.nome@email.com, 1990-05-10).
appdb=> 
```


3. Tentativa de registro violando a constraint UNIQUE.
Tentando utilizar o mesmo e-mail que foi usado no passado: joao.teste@email.com
```bash

appdb=> INSERT INTO app.pessoas (nome, email, data_nascimento)
VALUES ('Gabriel Almeida', 'joao.teste@email.com', '1997-04-12');
ERRO:  duplicar valor da chave viola a restrição de unicidade "pessoas_email_unique"
DETALHE:  Chave (email)=(joao.teste@email.com) já existe.
appdb=> 
```


4. Consulta a Tabela
- Primeiro SELECT
```bash

appdb=> SELECT * FROM app.pessoas;
 id |      nome      |          email           | data_nasc
imento 
----+----------------+--------------------------+----------
-------
  2 | Maria Teste    | joao.teste@email.com     | 1992-05-10
  1 | Outro João     | outro.joao@email.com     | 1991-02-02
  6 | Ana Souza      | ana.souza@email.com      | 1995-03-15
  7 | Bruno Silva    | bruno.silva@email.com    | 1988-07-22
  8 | Carla Oliveira | carla.oliveira@email.com | 2000-11-05
  9 | Daniel Santos  | daniel.santos@email.com  | 1992-01-30
 10 | Fernanda Costa | fernanda.costa@email.com | 1998-09-18
(7 linhas)

appdb=> 

```
- Segundo SELECT
```bash

appdb=> SELECT nome, email
FROM app.pessoas;
      nome      |          email           
----------------+--------------------------
 Maria Teste    | joao.teste@email.com
 Outro João     | outro.joao@email.com
 Ana Souza      | ana.souza@email.com
 Bruno Silva    | bruno.silva@email.com
 Carla Oliveira | carla.oliveira@email.com
 Daniel Santos  | daniel.santos@email.com
 Fernanda Costa | fernanda.costa@email.com
(7 linhas)

appdb=> 
```

5. Filtrando os registros com WHERE.
```bash
appdb=> SELECT nome, data_nascimento
FROM app.pessoas
WHERE data_nascimento > '1995-01-01';
      nome      | data_nascimento 
----------------+-----------------
 Ana Souza      | 1995-03-15
 Carla Oliveira | 2000-11-05
 Fernanda Costa | 1998-09-18
(3 linhas)

appdb=> 

```

6. Consultando os resultados com ORDER BY
   
```bash
appdb=> SELECT nome, data_nascimento
FROM app.pessoas
ORDER BY data_nascimento;
      nome      | data_nascimento 
----------------+-----------------
 Bruno Silva    | 1988-07-22
 Outro João     | 1991-02-02
 Daniel Santos  | 1992-01-30
 Maria Teste    | 1992-05-10
 Ana Souza      | 1995-03-15
 Fernanda Costa | 1998-09-18
 Carla Oliveira | 2000-11-05
(7 linhas)

appdb=> 
```


7. Contando quantas registros existem na tabela com COUNT

```bash
   appdb=> SELECT COUNT(*)
FROM app.pessoas;
 count 
-------
     7
(1 linha)

appdb=> 
```



# Atividade 5 - Alterando e removendo dados

1. Consultando a tabela antes de alterar os dados.
```bash
appdb=> SELECT *
FROM app.pessoas
WHERE id = 6;
 id |   nome    |        email        | data_nascimento
 
----+-----------+---------------------+----------------
-
  6 | Ana Souza | ana.souza@email.com | 1995-03-15
(1 linha)

appdb=> 
```

2. Fazendo a primeira ateração nos dados
```bash
appdb=> UPDATE app.pessoas
SET email = 'ana.souza.novo@email.com'
WHERE id = 6;
UPDATE 1
appdb=> 
```

3. Conferindo a alteração.
```bash
appdb=> SELECT *
FROM app.pessoas
WHERE id = 6;
 id |   nome    |          email           | data_nascimento 
----+-----------+--------------------------+-----------------
  6 | Ana Souza | ana.souza.novo@email.com | 1995-03-15
(1 linha)

appdb=> 
```

4. Consultando o regitro com id 10 antes de remove-lo para ter certeza que é registro correto.
```bash

appdb=> SELECT *
FROM app.pessoas
WHERE id = 10;
 id |      nome      |          email           | data_nascimento 
----+----------------+--------------------------+-----------------
 10 | Fernanda Costa | fernanda.costa@email.com | 1998-09-18
(1 linha)

appdb=> 
```

5.Depois de confirmado, fazendo o DELETE
```bash

appdb=> DELETE FROM app.pessoas
WHERE id = 10;
DELETE 1
appdb=> 

```

6. Conferindo se registro foi realmente removido.
```bash
appdb=> SELECT *
FROM app.pessoas
WHERE id = 10;
 id | nome | email | data_nascimento 
----+------+-------+-----------------
(0 linha)

appdb=> 
```

7. Verificando todos os registros.

```bash
appdb=> SELECT *
FROM app.pessoas
ORDER BY id;
 id |      nome      |          email           | d
ata_nascimento 
----+----------------+--------------------------+--
---------------
  1 | Outro João     | outro.joao@email.com     | 1
991-02-02
  2 | Maria Teste    | joao.teste@email.com     | 1992-05-10
  6 | Ana Souza      | ana.souza.novo@email.com | 1995-03-15
  7 | Bruno Silva    | bruno.silva@email.com    | 1988-07-22
  8 | Carla Oliveira | carla.oliveira@email.com | 2000-11-05
  9 | Daniel Santos  | daniel.santos@email.com  | 1992-01-30
(6 linhas)

appdb=> 
```


# Atividade 6 - Quem pode acessar a tabela?



1. Entrando como appuser
```bash
[postgres@localhost ~]$ psql -h localhost -U appuser -d appdb
Senha para o usuário appuser: 
psql (17.11)
Digite "help" para obter ajuda.

appdb=> 

```

2. Confirmando quem somos
```bash
appdb=> SELECT current_user, current_database();
 current_user | current_database 
--------------+------------------
 appuser      | appdb
(1 linha)

appdb=> 

```

3. Verificando se appuser consegue consultar a tabela
```bash
appdb=> SELECT *
FROM app.pessoas;
 id |      nome      |          email           | data_
nascimento 
----+----------------+--------------------------+------
-----------
  2 | Maria Teste    | joao.teste@email.com     | 1992-05-10
  1 | Outro João     | outro.joao@email.com     | 1991-02-02
  7 | Bruno Silva    | bruno.silva@email.com    | 1988-07-22
  8 | Carla Oliveira | carla.oliveira@email.com | 2000-11-05
  9 | Daniel Santos  | daniel.santos@email.com  | 1992-01-30
  6 | Ana Souza      | ana.souza.novo@email.com | 1995-03-15
(6 linhas)

appdb=> 
```

4. Descobrindo quem é o proprietário da tabela
```bash
appdb=> SELECT
    schemaname,
    tablename,
    tableowner
  AND tablename = 'pessoas';
 schemaname | tablename | tableowner 
------------+-----------+------------
 app        | pessoas   | appuser
(1 linha)

appdb=> 
```

5. Vendo os privilégios do appuser
```bash
appdb=> SELECT
    grantee,
    privilege_type
FROM information_schema.role_table_grants
WHERE table_schema = 'app'
  AND table_name = 'pessoas'
  AND grantee = 'appuser';
 grantee | privilege_type 
---------+----------------
 appuser | INSERT
 appuser | SELECT
 appuser | UPDATE
 appuser | DELETE
 appuser | TRUNCATE
 appuser | REFERENCES
 appuser | TRIGGER
(7 linhas)

appdb=> 

```
6. Teste INSERT
```bash

appdb=> BEGIN;
BEGIN
appdb=*> INSERT INTO app.pessoas
    (nome, email, data_nascimento)
VALUES
    ('Teste Permissao', 'teste.permissao@email.com', '1990-01-01');
INSERT 0 1
appdb=*> ROLLBACK;
ROLLBACK
appdb=>

appdb=> BEGIN;
BEGIN
appdb=*> INSERT INTO app.pessoas
    (nome, email, data_nascimento)
VALUES
    ('Teste Permissao', 'teste.permissao@email.com', '1990-01-01');
INSERT 0 1
appdb=*> ROLLBACK;
ROLLBACK
appdb=> 
```

7. Testando UPDATE
```bash
appdb=> BEGIN;
BEGIN
appdb=*> UPDATE app.pessoas
SET email = 'teste.permissao@email.com'
WHERE id = 1;
UPDATE 1
appdb=*> ROLLBACK;
ROLLBACK
appdb=> 
```

8.Testando DELETE
```bash
appdb=> BEGIN;
BEGIN
appdb=*> DELETE FROM app.pessoas
WHERE id = 1;
DELETE 1
appdb=*> ROLLBACK;
ROLLBACK
appdb=> 
```

9. Separando proprietário de usuário operacional, entrando e confirmando ROLE
```bash

appdb=> \q
[postgres@localhost ~]$ psql -d appdb
psql (17.11)
Digite "help" para obter ajuda.
appdb=# SELECT current_user, current_database();
 current_user | current_database 
--------------+------------------
 postgres     | appdb
(1 linha)

appdb=#
```

10.Verificando o proprietário da tabela
```bash

appdb=# SELECT
    schemaname,
    tablename,
    tableowner
  AND tablename = 'pessoas';
 schemaname | tablename | tableowner 
------------+-----------+------------
 app        | pessoas   | appuser
(1 linha)

appdb=# 

```
11.Verificando o proprietário do schema
```bash

appdb=# SELECT
    schema_name,
    schema_owner
FROM information_schema.schemata
WHERE schema_name = 'app';
 schema_name | schema_owner 
-------------+--------------
 app         | appuser
(1 linha)

appdb=# 
```

