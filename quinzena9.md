# dbanapratica

# Quinzena 9 - Administração básica do PostgreSQL

**Tema:** Instância, `psql`, roles, databases, autenticação e permissões

**Sistema Operacional:** Rocky Linux

**Dedicação:** aproximadamente 30 minutos por dia

---

# Objetivo da quinzena

Na quinzena anterior você instalou o PostgreSQL, iniciou o serviço e criou seu primeiro banco de dados.

Agora vamos começar a administrar o PostgreSQL.

Nesta quinzena você vai aprender a responder a uma pergunta que será muito importante durante toda a sua formação:

> "Por que determinado usuário consegue ou não consegue acessar determinado banco de dados?"

Para isso, vamos introduzir:

- `psql`;
- instância PostgreSQL;
- roles;
- databases;
- autenticação;
- `pg_hba.conf`;
- privilégios básicos;
- `GRANT` e `REVOKE`.

Ao final da quinzena você deverá conseguir criar uma role, preparar um database para ela, permitir sua conexão, testar o acesso e explicar o que está acontecendo quando uma conexão é aceita ou recusada.

---

# Como estudar

Não é necessário memorizar os comandos.

Para cada atividade:

1. Leia o material relacionado ao assunto.
2. Pesquise utilizando as palavras-chave fornecidas.
3. Tente realizar a tarefa no ambiente de laboratório.
4. Se encontrar um erro, procure entender o motivo antes de tentar corrigi-lo.
5. Registre no `diário.md` o que descobriu.

Uma boa parte do aprendizado desta quinzena acontecerá justamente quando algo não funcionar.

---

# Material de apoio

## Documentação principal

PostgreSQL Documentation:

https://www.postgresql.org/docs/current/

`psql`:

https://www.postgresql.org/docs/current/app-psql.html

Database Roles:

https://www.postgresql.org/docs/current/user-manag.html

Managing Databases:

https://www.postgresql.org/docs/current/manage-ag-overview.html

Client Authentication:

https://www.postgresql.org/docs/current/client-authentication.html

The pg_hba.conf File:

https://www.postgresql.org/docs/current/auth-pg-hba-conf.html

Privileges:

https://www.postgresql.org/docs/current/ddl-priv.html

---

# Material complementar

Para conteúdos em portugues, pesquise no YouTube por:

- PostgreSQL para iniciantes
- PostgreSQL administração
- PostgreSQL usuários e roles
- PostgreSQL pg_hba.conf
- PostgreSQL autenticação
- PostgreSQL permissions
- PostgreSQL GRANT REVOKE

Não é necessário assistir vários vídeos sobre o mesmo assunto.

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

Quando encontrar um erro interessante, registre também a mensagem de erro.

---

# Atividade 1 - Retomando o PostgreSQL

**Nível:** Básico

**Dificuldade:** Baixa

---

## Enunciado

Você ficou algumas semanas sem trabalhar com o servidor PostgreSQL.

Antes de começar uma nova configuração, você precisa verificar se ainda consegue administrar a instalação criada anteriormente.

Entre no servidor e tente realizar as tarefas sem consultar imediatamente o material da quinzena anterior.

Você deverá:

- verificar se o PostgreSQL está instalado;
- identificar sua versão;
- verificar o estado do serviço;
- acessar o PostgreSQL utilizando o `psql`;
- listar os databases existentes;
- localizar o database `laboratório`;
- conectar-se a ele;
- descobrir qual usuário/role está sendo utilizado na sessão;
- sair do `psql`.

Se não lembrar como realizar alguma tarefa, pesquise.

O objetivo não é lembrar todos os comandos utilizados anteriormente. O objetivo é verificar se você consegue recuperar a informação necessária.

---

## Palavras-chave para pesquisa

- PostgreSQL psql
- PostgreSQL list databases
- PostgreSQL current user
- PostgreSQL service systemctl
- psql meta commands

---

## Ao concluir esta atividade você deverá ser capaz de:

- verificar se uma instância PostgreSQL está funcionando;
- acessar o PostgreSQL utilizando o `psql`;
- identificar o database utilizado;
- identificar a role da sessão;
- utilizar a documentação para recuperar informações esquecidas.

---

## Checklist

- [ ] Serviço verificado.
- [ ] Versão identificada.
- [ ] `psql` utilizado.
- [ ] Databases listados.
- [ ] `laboratório` localizado.
- [ ] Conexão com `laboratório` realizada.
- [ ] Usuário/role da sessão identificado.
- Diário de bordo atualizado.

---

# Atividade 2 - Roles nao sao usuarios Linux

**Nível:** Básico

**Dificuldade:** Baixa

---

## Enunciado

Na administração Linux você já trabalhou com usuários e grupos.

Agora vamos descobrir como esse conceito aparece no PostgreSQL.

Investigue as seguintes questões:

- O que é uma role no PostgreSQL?
- Por que o PostgreSQL utiliza o termo "role"?
- Existe uma role chamada `postgres`?
- Existe um usuário Linux chamado `postgres`?
- Qual é a relação entre eles?
- Uma role PostgreSQL precisa obrigatoriamente ter um usuário Linux correspondente?
- Uma pessoa pode utilizar uma role PostgreSQL para acessar o banco sem possuir uma conta Linux no servidor?

Depois da pesquisa, utilize seu ambiente para identificar:

- as roles existentes;
- quais delas podem realizar login;
- quais possuem privilégios administrativos.

Não crie nenhuma role ainda.

O objetivo desta atividade é construir o modelo mental antes de começar a criar usuários.

---

## Palavras-chave para pesquisa

- PostgreSQL role
- PostgreSQL database roles
- PostgreSQL role LOGIN
- PostgreSQL superuser
- PostgreSQL list roles
- Linux user PostgreSQL role

---

## Ao concluir esta atividade você deverá ser capaz de:

- explicar o conceito de role;
- diferenciar uma role PostgreSQL de um usuário Linux;
- listar as roles existentes;
- identificar características básicas de uma role;
- reconhecer uma role com privilégios administrativos.

---

## Checklist

- [ ] Roles existentes identificadas.
- [ ] Role `postgres` investigada.
- [ ] Usuario Linux `postgres` investigado.
- [ ] Diferença entre os dois compreendida.
- [ ] Roles com login identificadas.
- [ ] Roles administrativas identificadas.
- Diário de bordo atualizado.

---

# Atividade 3 - Criando uma role e um database

**Nível:** Intermediário

**Dificuldade:** Moderada

---

## Enunciado

Agora você recebeu uma solicitação para preparar um ambiente simples para uma aplicação.

A aplicação utilizará:

- Role: `appuser`
- Database: `appdb`

Sua tarefa será criar esses dois recursos.

A role `appuser` deverá:

- poder realizar login;
- possuir uma senha;
- não possuir privilégios de superusuário.

O database `appdb` deverá ser criado de forma que `appuser` possa utilizá-lo.

Antes de executar os comandos, pesquise:

- como criar uma role;
- como habilitar login;
- como definir uma senha;
- como criar um database;
- como definir o proprietário de um database;
- como verificar as propriedades de uma role;
- como verificar as propriedades de um database.

Depois de criar os recursos, faça uma primeira tentativa de conexão utilizando `appuser`.

Registre o resultado.

Se a conexão funcionar, explique por que.

Se não funcionar, não tente simplesmente contornar o problema. Registre o erro e investigue o motivo.

---

## Palavras-chave para pesquisa

- PostgreSQL CREATE ROLE
- PostgreSQL CREATE DATABASE
- PostgreSQL role LOGIN
- PostgreSQL database owner
- PostgreSQL ALTER ROLE
- PostgreSQL psql connect database

---

## Ao concluir esta atividade você deverá ser capaz de:

- criar uma role;
- configurar uma role para realizar login;
- definir uma senha;
- criar um database;
- definir o proprietário de um database;
- consultar as propriedades desses recursos;
- realizar uma primeira investigação de problemas de conexão.

---

## Checklist

- [ ] `app user` criada.
- [ ] Login habilitado.
- [ ] Senha configurada.
- [ ] Superusuário não habilitado.
- [ ] `appdb` criado.
- Proprietário identificado.
- [ ] Propriedades verificadas.
- [ ] Tentativa de conexão realizada.
- [ ] Resultado documentado.
- Diário de bordo atualizado.

---

# Atividade 4 - Quem pode entrar? Conhecendo o pg_hba.conf

**Nível:** Intermediário

**Dificuldade:** Moderada

---

## Enunciado

Você criou `appuser` e `appdb`.

Agora chegou o momento de entender como o PostgreSQL decide se uma tentativa de conexão deve ser aceita ou recusada.

O PostgreSQL utiliza o arquivo:

`pg_hba.conf`

Esse arquivo contém regras de autenticação para as conexões realizadas contra a instância.

Sua primeira tarefa será localizar o arquivo utilizado pela sua instalação.

Depois, pesquise e responda:

- Para que serve o `pg_hba.conf`?
- Onde ele está localizado?
- Como descobrir qual arquivo `pg_hba.conf` está sendo utilizado?
- O que significa uma regra `local`?
- O que significa uma regra `host`?
- O que representa o campo de database?
- O que representa o campo de user?
- O que representa o endereço de origem?
- O que representa o método de autenticação?
- O que significa `peer`?
- O que significa `scram-sha-256`?
- O que significa `trust`?
- O PostgreSQL analisa todas as regras até encontrar uma que funcione ou existe outra lógica?

Não altere o arquivo imediatamente.

Primeiro leia as regras existentes e tente entender o que elas significam.

Depois documente sua interpretação.

---

## Palavras-chave para pesquisa

- PostgreSQL pg_hba.conf
- PostgreSQL client authentication
- PostgreSQL local authentication
- PostgreSQL host authentication
- PostgreSQL peer authentication
- PostgreSQL scram-sha-256
- PostgreSQL pg_hba.conf order

---

## Ao concluir esta atividade você deverá ser capaz de:

- localizar o `pg_hba.conf`;
- explicar para que ele serve;
- interpretar uma regra básica;
- diferenciar `local` e `host`;
- compreender database, user e address em uma regra;
- explicar o conceito básico dos métodos `peer`, `scram-sha-256` e `trust`;
- compreender que a ordem das regras é importante.

---

## Checklist

- [ ] `pg_hba.conf` localizado.
- Localização confirmada pelo PostgreSQL.
- [ ] Regras existentes analisadas.
- [ ] `local` compreendido.
- [ ] `host` compreendido.
- Database/user/address compreendidos.
- Métodos de autenticação pesquisados.
- Ordem das regras compreendida.
- Diário de bordo atualizado.

---

# Atividade 5 - Fazendo uma conexão funcionar

**Nível:** Intermediário

**Dificuldade:** Desafiadora

---

## Enunciado

Agora você deverá juntar os conhecimentos adquiridos.

Você possui:

- uma instância PostgreSQL;
- uma role `appuser`;
- um database `appdb`;
- um arquivo `pg_hba.conf`.

Seu objetivo é permitir que `appuser` consiga realizar uma conexão autenticada com `appdb`.

Primeiro tente realizar a conexão utilizando `appuser`.

Se ela for recusada, investigue o motivo.

Analise:

- a role;
- o database;
- as regras do `pg_hba.conf`;
- o método de autenticação utilizado;
- as permissões existentes.

Depois faça a alteração necessária no `pg_hba.conf`.

O acesso deverá utilizar autenticação por senha.

Após a alteração:

1. teste novamente;
2. confirme que `appuser` consegue acessar `appdb`;
3. registre qual regra do `pg_hba.conf` permitiu a conexão;
4. explique por que a conexão anterior foi recusada;
5. registre como a alteração foi aplicada ao PostgreSQL.

Não utilize `trust` para resolver o problema.

O objetivo é compreender o processo de autenticação, e não apenas fazer a conexão funcionar.

---

## Palavras-chave para pesquisa

- PostgreSQL pg_hba.conf password authentication
- PostgreSQL scram-sha-256
- PostgreSQL reload configuration
- PostgreSQL pg_hba.conf connection refused
- PostgreSQL authentication error

---

## Ao concluir esta atividade você deverá ser capaz de:

- diagnosticar uma falha simples de autenticação;
- relacionar uma tentativa de conexão com uma regra do `pg_hba.conf`;
- configurar uma regra básica de autenticação por senha;
- aplicar uma alteração de configuração;
- testar uma conexão novamente;
- explicar por que uma conexão foi aceita ou recusada.

---

## Checklist

- [ ] `appuser` testada.
- [ ] `appdb` testado.
- [ ] Falha de conexão investigada.
- [ ] Regra adequada identificada.
- Autenticação por senha configurada.
- [ ] Alteração aplicada.
- [ ] Conexão realizada com sucesso.
- [ ] Motivo da falha anterior explicado.
- Diário de bordo atualizado.

---

# Atividade 6 - Autenticação não é permissão

**Nível:** Intermediário

**Dificuldade:** Moderada

---

## Enunciado

Agora que `appuser` consegue se conectar ao `appdb`, uma nova dúvida aparece:

> Se `appuser` conseguiu entrar no database, significa que pode fazer qualquer coisa dentro dele?

A resposta deverá ser investigada na prática.

Utilizando `appuser`, verifique o que ela consegue fazer.

Depois pesquise:

- o que é autenticação;
- o que e autorização;
- o que são privilégios;
- o que e `GRANT`;
- o que e `REVOKE`;
- qual a diferença entre ser proprietário e possuir um privilégio concedido;
- quais privilégios existem para databases.

Documente a diferença entre:

```text
Autenticação
     |
     +-- posso entrar?

Autorização
     |
     +-- o que posso fazer depois que entrei?
```

Não é necessário criar tabelas nesta atividade.

O objetivo é entender a diferença entre conseguir estabelecer uma conexão e possuir autorização para realizar determinadas operações.

---

## Palavras-chave para pesquisa

- PostgreSQL authentication authorization
- PostgreSQL privileges
- PostgreSQL GRANT
- PostgreSQL REVOKE
- PostgreSQL database privileges
- PostgreSQL CONNECT privilege

---

## Ao concluir esta atividade você deverá ser capaz de:

- diferenciar autenticação de autorização;
- compreender o conceito de privilégio;
- explicar a função de `GRANT`;
- explicar a função de `REVOKE`;
- compreender que conseguir conectar-se não significa possuir todos os privilégios.

---

## Checklist

- [ ] Autenticação compreendida.
- [ ] Autorização compreendida.
- [ ] Privilégios pesquisados.
- [ ] `GRANT` pesquisado.
- [ ] `REVOKE` pesquisado.
- [ ] Privilégios de database pesquisados.
- [ ] Diferença entre autenticação e autorização documentada.
- [ ] Diário de bordo atualizado.

---

# Entregáveis

Ao final da quinzena você deverá apresentar:

- [ ] `diário.md` atualizado.
- [ ] Role `appuser`.
- [ ] Database `appdb`.
- [ ] Configuração de autenticação documental.
- [ ] Teste de conexão utilizando `appuser`.
- [ ] Explicação sobre a regra do `pg_hba.conf`.
- [ ] Explicação sobre autenticação e autorização.
- [ ] Resumo da quinzena.

---

# Resumo obrigatório

Escreva um texto com no máximo 20 linhas respondendo:

1. O que você aprendeu nesta quinzena?
2. O que foi mais importante na diferença entre usuários Linux e roles PostgreSQL?
3. O que é uma role?
4. O que é um database?
5. O que é o `pg_hba.conf`?
6. Como o PostgreSQL decide se uma conexão será aceita?
7. O que significa autenticação?
8. O que significa autorização?
9. Qual a diferença entre conseguir conectar e possuir permissão?
10. Qual foi a maior dificuldade encontrada?
11. O que você ainda precisa estudar melhor?

---




***Atividade 1 - Retomando o PostgreSQL***

1. Verificando se o PostgreSQL está instalado.

```bash
root@localhost wsantos]# rpm -qa | grep postgresql
postgresql17-libs-17.11-1PGDG.rhel9.x86_64
postgresql17-17.11-1PGDG.rhel9.x86_64
postgresql17-server-17.11-1PGDG.rhel9.x86_64
[root@localhost wsantos]#
```

2. Descobrindo a versão qual a versão do PostgreSQL está instalada.

```bash   
[root@localhost wsantos]# psql --version
psql (PostgreSQL) 17.11 
```

3. Verificando o serviço (O PostgreSQL está funcionando?)

```bash
[root@localhost wsantos]# systemctl status postgresql-17
● postgresql-17.service - PostgreSQL 17 database server
     Loaded: loaded (/usr/lib/systemd/system/postgresql-17.service; enabled; preset>
     Active: active (running) since Mon 2026-08-31 20:00:29 -03; 1h 32min ago
       Docs: https://www.postgresql.org/docs/17/static/
    Process: 978 ExecStartPre=/usr/pgsql-17/bin/postgresql-17-check-db-dir ${PGDATA>
   Main PID: 984 (postgres)
      Tasks: 7 (limit: 10515)
     Memory: 5.5M (peak: 27.7M)
        CPU: 192ms
     CGroup: /system.slice/postgresql-17.service
             ├─ 984 /usr/pgsql-17/bin/postgres -D /var/lib/pgsql/17/data/
             ├─1022 "postgres: logger "
             ├─1065 "postgres: checkpointer "
             ├─1066 "postgres: background writer "
             ├─1079 "postgres: walwriter "
             ├─1080 "postgres: autovacuum launcher "
             └─1081 "postgres: logical replication launcher "

ago 31 20:00:28 localhost.localdomain systemd[1]: Starting PostgreSQL 17 database s>
ago 31 20:00:28 localhost.localdomain postgres[984]: 2026-08-31 20:00:28.956 -03 [9>
ago 31 20:00:28 localhost.localdomain postgres[984]: 2026-08-31 20:00:28.956 -03 [9>
ago 31 20:00:29 localhost.localdomain systemd[1]: Started PostgreSQL 17 database se>
lines 1-22/22 (END)...skipping...
● postgresql-17.service - PostgreSQL 17 database server
     Loaded: loaded (/usr/lib/systemd/system/postgresql-17.service; enabled; preset>
     Active: active (running) since Mon 2026-08-31 20:00:29 -03; 1h 32min ago
       Docs: https://www.postgresql.org/docs/17/static/
    Process: 978 ExecStartPre=/usr/pgsql-17/bin/postgresql-17-check-db-dir ${PGDATA>
   Main PID: 984 (postgres)
      Tasks: 7 (limit: 10515)
     Memory: 5.5M (peak: 27.7M)
        CPU: 192ms
     CGroup: /system.slice/postgresql-17.service
             ├─ 984 /usr/pgsql-17/bin/postgres -D /var/lib/pgsql/17/data/
             ├─1022 "postgres: logger "
             ├─1065 "postgres: checkpointer "
             ├─1066 "postgres: background writer "
             ├─1079 "postgres: walwriter "
             ├─1080 "postgres: autovacuum launcher "
             └─1081 "postgres: logical replication launcher "

ago 31 20:00:28 localhost.localdomain systemd[1]: Starting PostgreSQL 17 database s>
ago 31 20:00:28 localhost.localdomain postgres[984]: 2026-08-31 20:00:28.956 -03 [9>
ago 31 20:00:28 localhost.localdomain postgres[984]: 2026-08-31 20:00:28.956 -03 [9>
ago 31 20:00:29 localhost.localdomain systemd[1]: Started PostgreSQL 17 database se>
~
~
```
4. Acessando o usuário postgres
```bash
[root@localhost wsantos]# su - postgres
[postgres@localhost ~]$
```

5. Entrando no PostgreSQL
```bash
[postgres@localhost ~]$ psql
psql (17.11)
Digite "help" para obter ajuda.

postgres=# 
```

6. Descobrindo onde estamos conectados
```bash
postgres=# \conninfo
Você está conectado ao banco de dados "postgres" como usuário "postgres" via soquete em "/run/postgresql" na porta "5432".
postgres=# 
```
7. Listando os bancos existentes e encontrando o banco laboratorio
```bash
postgres=# \l
                                                        Lista de bancos de dados
    Nome     |   Dono   | Codificação | Provedor de localidade |  Ordenação  |    Ct
ype    | Locale | Regras ICU | Privilégios de acesso 
-------------+----------+-------------+------------------------+-------------+------
-------+--------+------------+-----------------------
 laboratorio | postgres | UTF8        | libc                   | pt_BR.UTF-8 | pt_BR
.UTF-8 |        |            | 
 postgres    | postgres | UTF8        | libc                   | pt_BR.UTF-8 | pt_BR
.UTF-8 |        |            | 
 template0   | postgres | UTF8        | libc                   | pt_BR.UTF-8 | pt_BR
.UTF-8 |        |            | =c/postgres          +
             |          |             |                        |             |      
       |        |            | postgres=CTc/postgres
 template1   | postgres | UTF8        | libc                   | pt_BR.UTF-8 | pt_BR
.UTF-8 |        |            | =c/postgres          +
             |          |             |                        |             |      
       |        |            | postgres=CTc/postgres
(4 linhas)

postgres=# 
```

8. Conectando ao database laboratorio
```bash
postgres=# \c laboratorio
Agora você está conectado ao banco de dados "laboratorio" como usuário "postgres".
laboratorio=#
```
9. Confirmando a conexão
```bash
laboratorio=# \conninfo
Você está conectado ao banco de dados "laboratorio" como usuário "postgres" via soquete em "/run/postgresql" na porta "5432".
laboratorio=# 
```

10. Descobrindo o usuário/role
```bash
laboratorio=# SELECT current_user;
 current_user 
--------------
 postgres
(1 linha)

laboratorio=# 
```
11. Saindo do psql
```bash
laboratorio=# \q
[postgres@localhost ~]$ 
```
12. Voltar para o meu usuário
```bash
[postgres@localhost ~]$ exit
sair
[root@localhost wsantos]# 
```


***Atividade 2 - Roles nao sao usuarios Linux***

1. Verificando o usuário Linux postgres

```bash
[root@localhost wsantos]# id postgres
uid=26(postgres) gid=26(postgres) grupos=26(postgres)
[root@localhost wsantos]# 
```

2. Investigando o PostgreSQL

```bash
[root@localhost wsantos]# su - postgres
[postgres@localhost ~]$ psql
psql (17.11)
Digite "help" para obter ajuda.
```

3. Descobrindo as roles existentes

```bash
postgres=# \du
                                                  Lista de funções de banco de dado
s (roles)
 Nome da função de banco de dados (role) |                                         
    Atributos                                              
-----------------------------------------+-----------------------------------------
-----------------------------------------------------------
 postgres                                | Superusuário, Cria função de banco de da
dos (role), Cria banco de dados, Replicação, Contornar RLS
```

4. Descobrindo se a role postgres pode fazer login
```bash

postgres=# SELECT rolname, rolcanlogin, rolsuper FROM pg_roles;
           rolname           | rolcanlogin | rolsuper 
-----------------------------+-------------+----------
 postgres                    | t           | t
 pg_database_owner           | f           | f
 pg_read_all_data            | f           | f
 pg_write_all_data           | f           | f
 pg_monitor                  | f           | f
 pg_read_all_settings        | f           | f
 pg_read_all_stats           | f           | f
 pg_stat_scan_tables         | f           | f
 pg_read_server_files        | f           | f
 pg_write_server_files       | f           | f
 pg_execute_server_program   | f           | f
 pg_signal_backend           | f           | f
 pg_checkpoint               | f           | f
 pg_maintain                 | f           | f
 pg_use_reserved_connections | f           | f
 pg_create_subscription      | f           | f
(16 linhas)

```
***O (t) significa (true), verdadeiro.***
***O (f) significa (false), falso.***

5. Descobrindo somente as roles que podem fazer login
```bash
postgres=# SELECT rolname
FROM pg_roles
WHERE rolcanlogin = true;
 rolname  
----------
 postgres
(1 linha)

postgres=# 
```

6. Descobrindo quais são administrativas
```bash

postgres=# SELECT rolname FROM pg_roles WHERE rolsuper = true;
 rolname  
----------
 postgres
(1 linha)

postgres=# 
```

***Atividade 3 - Criando uma role e um database***

1. Entrando no PostgreSQL
```bash
[root@localhost wsantos]# su - postgres
[postgres@localhost ~]$ psql
psql (17.11)
Digite "help" para obter ajuda.

postgres=#
```
2. Criando a role appuser
```bash
postgres=# CREATE ROLE appuser LOGIN PASSWORD '0912063';
CREATE ROLE
postgres=#
```

3. Verificando se a role ficou correta
```bash
postgres=# \du appuser
     Lista de funções de banco de dados (roles)
 Nome da função de banco de dados (role) | Atributos 
-----------------------------------------+-----------
 appuser                                 | 

postgres=# 
```
Segunda forma de verificar
                               | 
```bash

postgres=# SELECT rolname, rolcanlogin, rolsuper
FROM pg_roles
WHERE rolname = 'appuser';
 rolname | rolcanlogin | rolsuper 
---------+-------------+----------
 appuser | t           | f
(1 linha)

postgres=# 
```

4. Criando o database appdb

```bash
postgres=# CREATE DATABASE appdb OWNER appuser;
CREATE DATABASE
postgres=# 
```

5. Verificando o database
```bash
postgres=# \l
                                                        Lista de bancos de dad
os
    Nome     |   Dono   | Codificação | Provedor de localidade |  Ordenação  |
    Ctype    | Locale | Regras ICU | Privilégios de acesso 
-------------+----------+-------------+------------------------+-------------+
-------------+--------+------------+-----------------------
 appdb       | appuser  | UTF8        | libc                   | pt_BR.UTF-8 |
 pt_BR.UTF-8 |        |            | 
 laboratorio | postgres | UTF8        | libc                   | pt_BR.UTF-8 |
 pt_BR.UTF-8 |        |            | 
 postgres    | postgres | UTF8        | libc                   | pt_BR.UTF-8 |
 pt_BR.UTF-8 |        |            | 
 template0   | postgres | UTF8        | libc                   | pt_BR.UTF-8 |
 pt_BR.UTF-8 |        |            | =c/postgres          +
             |          |             |                        |             |
             |        |            | postgres=CTc/postgres
 template1   | postgres | UTF8        | libc                   | pt_BR.UTF-8 |
 pt_BR.UTF-8 |        |            | =c/postgres          +
--Mais--
```


6. Primeira tentativa de conexão como appuser

Nesta primeira tentativa, houve um problema por conta da forma como o PostgreSQL tentou autenticar a conexão. O banco recebeu uma tentativa de conexão local para o appuser, mas usou o método de autenticação peer.
Nesse tipo de conexão, a regra peer compara: Usuário do Linux → Usuário do PostgreSQL
No momento do teste, o terminal estava como postgres@localhost:
Usuário Linux: postgres
Usuário PostgreSQL: appuser
Como os usuários são diferentes, o PostgreSQL recusou o acesso e retornou o erro: FATAL: A autenticação do tipo peer falhou para o usuário "appuser"


```bash
[postgres@localhost ~]$ psql -U appuser -d appdb
psql: erro: a conexão com o servidor no soquete "/run/postgresql/.s.PGSQL.5432" falhou: FATAL:  A autenticação do tipo peer falhou para o usuário "appuser"
```

7. 2º tentativa de conexão como appuser
```bash
[postgres@localhost ~]$ psql -U appuser -d appdb -h localhost
Senha para o usuário appuser: 
psql (17.11)
Digite "help" para obter ajuda.

appdb=>
```
8. Confirmando qual usuário está conectado

```bash
appdb=> SELECT current_user;
 current_user 
--------------
 appuser
(1 linha)

appdb=> 
```
9. Confirmando o database
```bash
appdb=> SELECT current_database();
 current_database 
------------------
 appdb
(1 linha)

appdb=> 
```
10.Verificando as informações da conexão
```bash
appdb=> SELECT current_database();
 current_database 
------------------
 appdb
(1 linha)

appdb=> \conninfo
Você está conectado ao banco de dados "appdb" como usuário "appuser" no hospedeiro "localhost" (endereço ::1") na porta "5432".
appdb=> 

```

11. Verificando a propriedade da role
```bash
appdb=> \du appuser
     Lista de funções de banco de dados (roles)
 Nome da função de banco de dados (role) | Atributos 
-----------------------------------------+-----------
 appuser                                 | 

appdb=>

```
12. Confirmando o proprietário do appdb
```bash
appdb=> ^C
appdb=> SELECT datname, pg_get_userbyid(datdba) AS owner
FROM pg_database
WHERE datname = 'appdb';
 datname |  owner  
---------+---------
 appdb   | appuser
(1 linha)

appdb=> 
```

# Atividade 4 - Quem pode entrar? Conhecendo o pg_hba.conf

1. Entrando no PostgreSQL e entrando no psql
```bash
[root@localhost wsantos]# su - postgres
[postgres@localhost ~]$ psql
psql (17.11)
Digite "help" para obter ajuda.

postgres=# 

```

2. Descobrindo qual pg_hba.conf está sendo usado

Utilizei SHOW hba_file; para descobrir o arquivo pg_hba.conf utilizado pela instância PostgreSQL.
```bash
postgres=# SHOW hba_file;
              hba_file              
------------------------------------
 /var/lib/pgsql/17/data/pg_hba.conf
(1 linha)

postgres=# 
```




