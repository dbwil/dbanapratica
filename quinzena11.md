# Quinzena 11 - Administração de roles e privilégios no PostgreSQL

Tema: Roles, grupos, privilégios, herança e administração de acessos

Sistema Operacional: Rocky Linux

Dedicação: aproximadamente 30 minutos por dia

---

# Objetivo da quinzena

Na Quinzena 9 você começou a entender autenticação e autorização.

Na Quinzena 10 você avançou para os objetos dentro de um database e começou a trabalhar com permissões sobre schemas e tabelas.

Agora vamos aprofundar esse assunto.

Até aqui, você trabalhou principalmente com uma role diretamente:

    appuser
       |
       +-- acesso ao database
       |
       +-- acesso ao schema
       |
       +-- acesso à tabela


Em um ambiente real, administrar permissões individualmente para cada usuário pode se tornar difícil.

Imagine uma aplicação com:

    usuário 1
    usuário 2
    usuário 3
    usuário 4
    usuário 5

Se todos precisam das mesmas permissões, não é necessário configurar cada usuário separadamente.

Podemos organizar os privilégios utilizando roles que representam grupos de acesso:

    role de acesso
          |
          +-- SELECT
          +-- INSERT
          +-- UPDATE
          |
          +-- usuário 1
          +-- usuário 2
          +-- usuário 3


Nesta quinzena vamos estudar:

- roles;
- atributos de roles;
- roles com e sem LOGIN;
- roles utilizadas como grupos;
- membership;
- `GRANT` e `REVOKE` para roles;
- herança de privilégios;
- `SET ROLE`;
- privilégios em diferentes níveis;
- administração segura de acessos;
- identificação e correção de permissões excessivas.

O objetivo não é decorar comandos.

O objetivo é começar a pensar:

> Quem precisa acessar o quê, e por meio de qual role?

---

# Como estudar

Continue utilizando aproximadamente 30 minutos por dia.

Para cada atividade:

1. Leia o material relacionado ao assunto.
2. Pesquise utilizando as palavras-chave fornecidas.
3. Tente executar os procedimentos no ambiente de laboratório.
4. Observe o resultado.
5. Quando encontrar um erro, tente entender o motivo.
6. Registre as descobertas no `diario.md`.
7. Ao terminar uma atividade, tente explicar o conceito sem consultar o material.

Não copie simplesmente os comandos encontrados na documentação.

Antes de executar um comando, procure entender:

- o que ele modifica;
- em qual objeto ele atua;
- qual role está executando a operação;
- qual efeito terá sobre o acesso.

---

# Material de apoio

## Documentação principal

PostgreSQL Documentation:

https://www.postgresql.org/docs/current/

Database Roles:

https://www.postgresql.org/docs/current/user-manag.html

Role Attributes:

https://www.postgresql.org/docs/current/role-attributes.html

Role Membership:

https://www.postgresql.org/docs/current/role-membership.html

GRANT:

https://www.postgresql.org/docs/current/sql-grant.html

REVOKE:

https://www.postgresql.org/docs/current/sql-revoke.html

ALTER ROLE:

https://www.postgresql.org/docs/current/sql-alterrole.html

Privileges:

https://www.postgresql.org/docs/current/ddl-priv.html

Schemas:

https://www.postgresql.org/docs/current/ddl-schemas.html

---

# Material complementar

Para conteúdos em português, pesquise no YouTube por:

- PostgreSQL roles
- PostgreSQL usuários e roles
- PostgreSQL permissões
- PostgreSQL GRANT REVOKE
- PostgreSQL role membership
- PostgreSQL usuários e grupos
- PostgreSQL privilégios
- PostgreSQL SET ROLE

Não é necessário assistir a vários vídeos sobre o mesmo assunto.

Escolha um material que consiga acompanhar e utilize a documentação oficial para complementar o estudo.

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

Quando encontrar um comportamento ou erro interessante, registre também:

- o que você tentou fazer;
- qual foi o resultado;
- o que você esperava que acontecesse;
- por que o resultado aconteceu;
- como resolveu.

Nesta quinzena, dê atenção especial aos testes de permissões.

Sempre que possível, registre:

    role utilizada
    |
    +-- objeto acessado
    |
    +-- operação realizada
    |
    +-- resultado
    |
    +-- motivo

---

# Atividade 1 - Revisando o modelo de roles

Nível: Básico

Dificuldade: Baixa

Tempo estimado: 30 minutos

---

## Enunciado

Na Quinzena 9 você criou a role `appuser`.

Na Quinzena 10 você utilizou essa role para testar acesso ao database e aos objetos existentes dentro dele.

Agora vamos voltar um passo e entender melhor o conceito de role.

Pesquise e responda:

- O que é uma role no PostgreSQL?
- Uma role precisa obrigatoriamente possuir `LOGIN`?
- Qual é a diferença entre uma role que possui `LOGIN` e uma que não possui?
- O que significa utilizar uma role como grupo?
- Qual é a relação entre roles, usuários e grupos no PostgreSQL?
- Qual é a diferença entre uma role e um usuário Linux?

Depois investigue as roles existentes no seu ambiente.

Identifique:

- quais roles possuem `LOGIN`;
- quais não possuem;
- quais são superusuárias;
- quais podem criar databases;
- quais podem criar outras roles.

Não altere nenhuma role nesta atividade.

O objetivo é somente observar o ambiente existente.

---

## Dica

Não confunda:

    usuário Linux
          |
          X
    role PostgreSQL

São mecanismos diferentes.

Uma role PostgreSQL pode representar um usuário que faz login ou pode ser utilizada para organizar privilégios.

---

## Palavras-chave para pesquisa

- PostgreSQL role
- PostgreSQL role LOGIN
- PostgreSQL role group
- PostgreSQL role attributes
- PostgreSQL database user
- PostgreSQL role versus Linux user

---

## Ao concluir esta atividade você deverá ser capaz de:

- explicar o que é uma role;
- diferenciar role PostgreSQL de usuário Linux;
- explicar o significado de `LOGIN`;
- identificar atributos básicos de uma role;
- explicar por que uma role pode ser utilizada como grupo.

---

## Checklist

- [x] Roles existentes investigadas.
- [x] Roles com `LOGIN` identificadas.
- [x] Roles sem `LOGIN` identificadas.
- [x] Atributos básicos pesquisados.
- [x] Diferença entre role e usuário Linux compreendida.
- [x] Conceito de role como grupo compreendido.
- [x] Diário de bordo atualizado.

---

# Atividade 2 - Criando uma role de acesso

Nível: Básico

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

Agora você vai criar uma role que será utilizada somente para representar um grupo de acesso.

Crie uma role chamada:

`app_readonly`

Essa role deverá representar usuários que podem consultar dados da aplicação, mas não devem alterar os dados.

Pesquise antes de criá-la:

- uma role de grupo precisa de `LOGIN`?
- qual é a diferença entre uma role utilizada para login e uma role utilizada como grupo?
- quais atributos devem ser evitados em uma role que existe somente para organização de privilégios?

Depois crie a role.

Não crie uma senha para ela com o objetivo de realizar login.

A role será utilizada como um grupo de privilégios.

No diário de bordo, explique por que ela foi criada dessa maneira.

---

## Dica

Pense na função da role:

    app_readonly
         |
         +-- representa um conjunto de privilégios
         |
         +-- não representa necessariamente uma pessoa
         |
         +-- não precisa ser utilizada diretamente para login

---

## Palavras-chave para pesquisa

- PostgreSQL group role
- PostgreSQL role NOLOGIN
- PostgreSQL CREATE ROLE
- PostgreSQL role group
- PostgreSQL login role

---

## Ao concluir esta atividade você deverá ser capaz de:

- criar uma role utilizada como grupo;
- diferenciar uma role de login de uma role de acesso;
- explicar por que uma role de grupo normalmente não precisa de `LOGIN`.

---

## Checklist

- [x] Role `app_readonly` criada.
- [x] Finalidade da role compreendida.
- [x] `LOGIN` pesquisado.
- [x] Atributos da role verificados.
- [x] Diário de bordo atualizado.

---

# Atividade 3 - Colocando privilégios em uma role de grupo

Nível: Intermediário

Dificuldade: Moderada

Tempo estimado: 45 minutos

---

## Enunciado

Agora vamos utilizar a role:

`app_readonly`

Ela deverá representar usuários que podem consultar a tabela:

`app.pessoas`

mas não podem modificar seus dados.

Você deverá pesquisar como conceder privilégios para:

- database;
- schema;
- tabela.

Depois configure `app_readonly` para que ela tenha somente os privilégios necessários para consultar a tabela `app.pessoas`.

Não conceda:

- `INSERT`;
- `UPDATE`;
- `DELETE`;
- privilégios administrativos;
- superusuário.

Depois verifique os privilégios concedidos.

Seu objetivo é conseguir explicar o caminho:

    database
       |
       v
    schema
       |
       v
    table
       |
       v
    SELECT

---

## Dica

Conseguir conectar ao database não significa necessariamente conseguir utilizar uma tabela.

Da mesma forma, conseguir utilizar um schema não significa necessariamente possuir todas as permissões sobre as tabelas existentes nele.

Pense em cada nível separadamente.

---

## Palavras-chave para pesquisa

- PostgreSQL GRANT CONNECT
- PostgreSQL GRANT USAGE schema
- PostgreSQL GRANT SELECT table
- PostgreSQL schema privileges
- PostgreSQL table privileges
- PostgreSQL privileges hierarchy

---

## Ao concluir esta atividade você deverá ser capaz de:

- conceder privilégios a uma role;
- diferenciar privilégios de database, schema e tabela;
- configurar uma role somente para leitura;
- verificar os privilégios existentes.

---

## Checklist

- [ ] Privilégio sobre o database investigado.
- [ ] Privilégio sobre o schema investigado.
- [ ] `SELECT` concedido à role.
- [ ] Privilégios de alteração não concedidos.
- [ ] Privilégios verificados.
- [ ] Diário de bordo atualizado.

---

# Atividade 4 - Fazendo um usuário herdar os privilégios da role

Nível: Intermediário

Dificuldade: Moderada

Tempo estimado: 45 minutos

---

## Enunciado

Agora vamos utilizar uma segunda role de login.

Crie uma role chamada:

`appreader`

Essa role deverá ser capaz de fazer login.

Depois faça com que `appreader` seja membro da role:

`app_readonly`

O objetivo é que `appreader` receba os privilégios de leitura por meio da membership.

O resultado esperado conceitualmente é:

    app_readonly
         |
         +-- SELECT em app.pessoas
         |
         v
    appreader
         |
         +-- LOGIN
         |
         +-- pode consultar app.pessoas


Teste utilizando `appreader`.

Verifique:

- consegue conectar ao database?
- consegue acessar o schema?
- consegue consultar `app.pessoas`?
- consegue inserir?
- consegue alterar?
- consegue remover?

Registre todos os resultados.

---

## Dica

Aqui aparece um conceito importante:

> O privilégio não precisa ser concedido diretamente ao usuário.

Ele pode ser concedido à role de grupo e o usuário pode receber esse privilégio por ser membro dela.

---

## Palavras-chave para pesquisa

- PostgreSQL role membership
- PostgreSQL GRANT role TO user
- PostgreSQL role inheritance
- PostgreSQL INHERIT
- PostgreSQL group role privileges

---

## Ao concluir esta atividade você deverá ser capaz de:

- criar uma role de login;
- adicionar uma role como membro de outra;
- explicar o conceito de membership;
- testar privilégios herdados;
- diferenciar privilégio direto de privilégio recebido por membership.

---

## Checklist

- [ ] Role `appreader` criada.
- [ ] `LOGIN` configurado.
- [ ] Membership configurada.
- [ ] Conexão testada.
- [ ] `SELECT` testado.
- [ ] `INSERT` testado.
- [ ] `UPDATE` testado.
- [ ] `DELETE` testado.
- [ ] Resultados registrados.
- [ ] Diário de bordo atualizado.

---

# Atividade 5 - Removendo privilégios e membership

Nível: Intermediário

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

Agora vamos praticar a remoção de acessos.

Primeiro, retire da role `appreader` a membership em:

`app_readonly`

Depois teste novamente o acesso à tabela.

Observe o que mudou.

Em seguida, restaure a membership e confirme que o acesso voltou a funcionar.

Depois investigue a diferença entre:

- remover um privilégio;
- remover uma membership;
- remover uma role.

Não execute a remoção definitiva das roles nesta atividade.

O objetivo é entender as diferenças entre essas operações.

---

## Dica

Imagine o seguinte cenário:

    appreader
        |
        +-- membro de app_readonly
                    |
                    +-- SELECT

Se você remove a membership, o que acontece com o `SELECT`?

O privilégio continua existindo na role `app_readonly`, mas quem deixa de recebê-lo?

---

## Palavras-chave para pesquisa

- PostgreSQL REVOKE role membership
- PostgreSQL REVOKE privilege
- PostgreSQL DROP ROLE
- PostgreSQL revoke access
- PostgreSQL role membership

---

## Ao concluir esta atividade você deverá ser capaz de:

- remover uma membership;
- restaurar uma membership;
- explicar a diferença entre `REVOKE` de privilégio e `REVOKE` de membership;
- explicar por que remover uma role é diferente de remover seu acesso.

---

## Checklist

- [ ] Membership removida.
- [ ] Acesso testado após remoção.
- [ ] Membership restaurada.
- [ ] Acesso testado novamente.
- [ ] Diferença entre privilégio e membership compreendida.
- [ ] `DROP ROLE` pesquisado.
- [ ] Diário de bordo atualizado.

---

# Atividade 6 - Investigando privilégios excessivos

Nível: Intermediário

Dificuldade: Desafiadora

Tempo estimado: 45 minutos

---

## Enunciado

Agora vamos fazer um exercício de investigação.

Imagine que alguém configurou:

`appreader`

para acessar `app.pessoas`.

Mas, além de consultar os dados, essa role também consegue:

- inserir;
- alterar;
- remover registros.

Isso não deveria acontecer.

Sua tarefa será investigar o ambiente e descobrir:

1. quais privilégios `appreader` possui;
2. quais privilégios são diretos;
3. quais privilégios são recebidos por membership;
4. qual role concede os privilégios;
5. quais privilégios são realmente necessários;
6. quais privilégios devem ser removidos.

Depois corrija a configuração.

O resultado final deve ser:

    appreader
         |
         +-- pode conectar
         |
         +-- pode acessar o schema
         |
         +-- pode SELECT
         |
         X-- não pode INSERT
         X-- não pode UPDATE
         X-- não pode DELETE


Não conceda privilégios adicionais somente para fazer o acesso funcionar.

---

## Dica

Quando uma permissão existe, não assuma imediatamente que ela foi concedida diretamente ao usuário.

Pergunte:

> De onde veio esse privilégio?

Pode existir uma cadeia como:

    appreader
        |
        v
    app_readonly
        |
        v
    SELECT


Antes de modificar alguma coisa, descubra a origem do acesso.

---

## Palavras-chave para pesquisa

- PostgreSQL effective privileges
- PostgreSQL role membership privileges
- PostgreSQL table privileges
- PostgreSQL information_schema table_privileges
- PostgreSQL privileges role
- PostgreSQL privilege inheritance

---

## Ao concluir esta atividade você deverá ser capaz de:

- investigar privilégios efetivos;
- identificar a origem de um privilégio;
- distinguir privilégio direto de privilégio recebido;
- remover privilégios desnecessários;
- corrigir uma configuração de acesso sem conceder privilégios excessivos.

---

## Checklist

- [ ] Privilégios de `appreader` investigados.
- [ ] Membership investigada.
- [ ] Origem dos privilégios identificada.
- [ ] Privilégios desnecessários identificados.
- [ ] Privilégios removidos.
- [ ] Acesso correto testado.
- [ ] Operações não autorizadas testadas.
- [ ] Diário de bordo atualizado.

---

# Atividade 7 - Desafio final: desenhando um modelo de acesso

Nível: Intermediário

Dificuldade: Desafiadora

Tempo estimado: 45 minutos

---

## Enunciado

Agora você deverá criar uma pequena estrutura de acesso para uma aplicação.

Imagine que existem dois tipos de usuários:

    leitores
    operadores

Os leitores precisam:

- conectar ao database;
- acessar o schema `app`;
- consultar `app.pessoas`.

Os operadores precisam:

- conectar ao database;
- acessar o schema `app`;
- consultar `app.pessoas`;
- inserir registros;
- alterar registros;
- remover registros.

Crie duas roles de grupo:

`app_readonly`

`app_operator`

Depois crie duas roles de login:

`usuario_leitura`

`usuario_operacao`

Organize as memberships de forma que:

    app_readonly
        |
        +-- SELECT
        |
        +-- usuario_leitura


    app_operator
        |
        +-- SELECT
        +-- INSERT
        +-- UPDATE
        +-- DELETE
        |
        +-- usuario_operacao


Não conceda privilégios diretamente às roles de login se você conseguir estruturar o acesso utilizando as roles de grupo.

Depois teste cada usuário.

Monte uma tabela no seu diário:

    Role                  SELECT  INSERT  UPDATE  DELETE

    usuario_leitura       ?       ?       ?       ?

    usuario_operacao      ?       ?       ?       ?

Preencha com os resultados reais dos testes.

---

## Dica

Tente pensar primeiro no desenho e somente depois nos comandos.

Primeiro:

    quem?
      |
      v
    qual grupo?
      |
      v
    quais privilégios?
      |
      v
    quais objetos?


Depois implemente.

---

## Palavras-chave para pesquisa

- PostgreSQL role based access control
- PostgreSQL group roles
- PostgreSQL role membership
- PostgreSQL GRANT role
- PostgreSQL privileges
- PostgreSQL least privilege

---

## Ao concluir esta atividade você deverá ser capaz de:

- desenhar uma estrutura simples de acesso;
- separar usuários de roles de grupo;
- organizar privilégios por função;
- testar diferentes perfis de acesso;
- identificar privilégios excessivos.

---

## Checklist

- [ ] Role `app_readonly` configurada.
- [ ] Role `app_operator` configurada.
- [ ] `usuario_leitura` criada.
- [ ] `usuario_operacao` criada.
- [ ] Memberships configuradas.
- [ ] Privilégios de leitura testados.
- [ ] Privilégios de alteração testados.
- [ ] Acessos indevidos testados.
- [ ] Resultado documentado.
- [ ] Diário de bordo atualizado.

---

# Entregável final - Modelo de controle de acesso

Ao final da quinzena, você deverá conseguir representar o ambiente aproximadamente desta forma:

    PostgreSQL
        |
        +-- appdb
              |
              +-- app
                    |
                    +-- pessoas
                          |
                          +-- SELECT
                          +-- INSERT
                          +-- UPDATE
                          +-- DELETE


    app_readonly
        |
        +-- SELECT
        |
        +-- usuario_leitura


    app_operator
        |
        +-- SELECT
        +-- INSERT
        +-- UPDATE
        +-- DELETE
        |
        +-- usuario_operacao


Você deverá conseguir explicar:

- o que é uma role;
- o que é uma role de login;
- o que é uma role utilizada como grupo;
- o que é membership;
- o que é herança de privilégios;
- como conceder membership;
- como remover membership;
- como conceder privilégios;
- como remover privilégios;
- como investigar a origem de um privilégio;
- por que separar usuários de roles de acesso.

---

# Resumo obrigatório

Escreva um texto com no máximo 20 linhas respondendo:

1. O que é uma role no PostgreSQL?
2. Uma role precisa ter `LOGIN`?
3. Qual a diferença entre uma role de login e uma role de grupo?
4. O que é membership?
5. Para que serve `GRANT`?
6. Para que serve `REVOKE`?
7. Qual a diferença entre conceder um privilégio a uma role e concedê-lo diretamente a um usuário?
8. O que significa herdar privilégios?
9. Para que serve `SET ROLE`?
10. O que acontece quando uma membership é removida?
11. Qual a diferença entre remover um privilégio e remover uma role?
12. Como descobrir quais privilégios uma role possui?
13. Como descobrir de onde um privilégio pode estar vindo?
14. Por que é interessante utilizar roles de grupo?
15. O que significa conceder somente os privilégios necessários?
16. Qual foi a maior dificuldade encontrada?
17. O que você ainda precisa estudar melhor?

---

# Perguntas para avaliação

Durante nosso próximo encontro, esteja preparado para responder:

1. O que é uma role no PostgreSQL?

2. Qual a diferença entre uma role PostgreSQL e um usuário Linux?

3. Uma role precisa possuir `LOGIN` para existir?

4. Para que serve o atributo `LOGIN`?

5. O que significa utilizar uma role como grupo?

6. O que é membership?

7. Como uma role pode receber os privilégios de outra role?

8. Qual é a finalidade de `GRANT`?

9. Qual é a finalidade de `REVOKE`?

10. Qual a diferença entre `GRANT SELECT ON tabela TO usuario` e conceder `SELECT` a uma role de grupo da qual o usuário é membro?

11. O que acontece quando uma membership é removida?

12. O privilégio deixa de existir ou somente deixa de estar disponível para aquele membro?

13. Qual a diferença entre uma role com `LOGIN` e uma role sem `LOGIN`?

14. O que significa `INHERIT`?

15. Para que serve `SET ROLE`?

16. Uma role de grupo precisa necessariamente possuir `LOGIN`?

17. Como descobrir quem é membro de determinada role?

18. Como verificar os privilégios de uma tabela?

19. Como descobrir se um acesso é direto ou vem de uma role da qual o usuário é membro?

20. Se um usuário consegue consultar uma tabela mas também consegue alterá-la, onde você investigaria?

21. Se um usuário perdeu acesso depois que uma membership foi removida, o que aconteceu?

22. Qual a vantagem de organizar privilégios em roles de grupo?

23. Por que não devemos conceder privilégios administrativos simplesmente para resolver um problema de acesso?

24. Explique o conceito de menor privilégio.

25. Desenhe uma estrutura em que dois usuários possuam permissões diferentes sobre a mesma tabela sem precisar conceder os privilégios diretamente a cada usuário.







# Quinzena 11 - Administração de roles e privilégios no PostgreSQL


# Atividade 1 - Revisando o modelo de roles

1.Entrando no PostgreSQL e descobrindo quem somos dentro do PostgreSQL

```text


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


2. Investigando as Roles
```text

appdb=# SELECT
    rolname,
    rolcanlogin,
    rolsuper,
    rolcreatedb,
    rolcreaterole,
    rolinherit,
    rolreplication,
    rolbypassrls
FROM pg_roles
ORDER BY rolname;
           rolname           | rolcanlogin | rolsuper | rolcreatedb | rolcreaterole | rolinherit | rolreplication | rolbypassrls 
-----------------------------+-------------+----------+-------------+---------------+------------+----------------+--------------
 appuser                     | t           | f        | f           | f             | t          | f              | f
 pg_checkpoint               | f           | f        | f           | f             | t          | f              | f
 pg_create_subscription      | f           | f        | f           | f             | t          | f              | f
 pg_database_owner           | f           | f        | f           | f             | t          | f              | f
 pg_execute_server_program   | f           | f        | f           | f             | t          | f              | f
 pg_maintain                 | f           | f        | f           | f             | t          | f              | f
 pg_monitor                  | f           | f        | f           | f             | t          | f              | f
 pg_read_all_data            | f           | f        | f           | f             | t          | f              | f
 pg_read_all_settings        | f           | f        | f           | f             | t          | f              | f
 pg_read_all_stats           | f           | f        | f           | f             | t          | f              | f
 pg_read_server_files        | f           | f        | f           | f             | t          | f              | f
 pg_signal_backend           | f           | f        | f           | f             | t          | f              | f
 pg_stat_scan_tables         | f           | f        | f           | f             | t          | f              | f
 pg_use_reserved_connections | f           | f        | f           | f             | t          | f              | f
 pg_write_all_data           | f           | f        | f           | f             | t          | f              | f
 pg_write_server_files       | f           | f        | f           | f             | t          | f              | f
 postgres                    | t           | t        | t           | t             | t          | t              | t
(17 linhas)

appdb=# 
```

```text


appdb=# \du
                                                  Lista de funções de banco de dados (roles)
 Nome da função de banco de dados (role) |                                             Atributos                                              
-----------------------------------------+----------------------------------------------------------------------------------------------------
 appuser                                 | 
 postgres                                | Superusuário, Cria função de banco de dados (role), Cria banco de dados, Replicação, Contornar RLS

appdb=# 

```

3. Identificando somente as Roles com LOGIN
```text
appdb=# SELECT
    rolname,
    rolcanlogin
FROM pg_roles
WHERE rolcanlogin = true
ORDER BY rolname;
 rolname  | rolcanlogin 
----------+-------------
 appuser  | t
 postgres | t
(2 linhas)
```

4. Identificando somente as Roles com SEM LOGIN

```text

appdb=# SELECT
    rolname,
    rolcanlogin
FROM pg_roles
WHERE rolcanlogin = false
ORDER BY rolname;
           rolname           | rolcanlogin 
-----------------------------+-------------
 pg_checkpoint               | f
 pg_create_subscription      | f
 pg_database_owner           | f
 pg_execute_server_program   | f
 pg_maintain                 | f
 pg_monitor                  | f
 pg_read_all_data            | f
 pg_read_all_settings        | f
 pg_read_all_stats           | f
 pg_read_server_files        | f
 pg_signal_backend           | f
 pg_stat_scan_tables         | f
 pg_use_reserved_connections | f
 pg_write_all_data           | f
 pg_write_server_files       | f
(15 linhas)

appdb=# 
```


5. Identificando Superusers

```text
appdb=# SELECT
    rolname,
    rolsuper
FROM pg_roles
WHERE rolsuper = true
ORDER BY rolname;
 rolname  | rolsuper 
----------+----------
 postgres | t
(1 linha)

appdb=# 
```

6.Identificando quem pode criar databases
```text
appdb=# SELECT
    rolname,
    rolcreatedb
FROM pg_roles
WHERE rolcreatedb = true
ORDER BY rolname;
 rolname  | rolcreatedb 
----------+-------------
 postgres | t
(1 linha)

appdb=#

```
7. Identificando quem pode criar outras Roles

```text
appdb=# SELECT
    rolname,
    rolcreaterole
FROM pg_roles
WHERE rolcreaterole = true
ORDER BY rolname;
 rolname  | rolcreaterole 
----------+---------------
 postgres | t
(1 linha)

appdb=# 
```


# Atividade 2 - Criando uma role de acesso


1.Criando uma role de acesso
```text
[postgres@localhost ~]$ psql -d appdb
psql (17.11)
Digite "help" para obter ajuda.

appdb=# CREATE ROLE app_readonly NOLOGIN;
CREATE ROLE
appdb=# 
```

2. Conferindo se realmente a ROLE foi criada

```text

appdb=# CREATE ROLE app_readonly NOLOGIN;
CREATE ROLE
appdb=# SELECT
    rolname,
    rolcanlogin,
    rolsuper,
    rolcreatedb,
    rolcreaterole
FROM pg_roles
WHERE rolname = 'app_readonly';
   rolname    | rolcanlogin | rolsuper | rolcreatedb | rolcreaterole 
--------------+-------------+----------+-------------+---------------
 app_readonly | f           | f        | f           | f
(1 linha)

appdb=# 
```

3. Outra verificação ainda mais simples.
```text
appdb=# \du app_readonly
            Lista de funções de banco de dados (roles)
 Nome da função de banco de dados (role) |        Atributos        
-----------------------------------------+-------------------------
 app_readonly                            | Não é possível conectar

appdb=# 
```


# Atividade 3 - Colocando privilégios em uma role de grupo

1. Verificando o estado atual e a role que criei.
```text
[postgres@localhost ~]$ psql -d appdb
psql (17.11)
Digite "help" para obter ajuda.

appdb=# SELECT current_user, current_database();
 current_user | current_database 
--------------+------------------
 postgres     | appdb
(1 linha)
appdb=# \du app_readonly
            Lista de funções de banco de dados (roles)
 Nome da função de banco de dados (role) |        Atributos        
-----------------------------------------+-------------------------
 app_readonly                            | Não é possível conectar

appdb=# 

```


2. Primeiro nível: DATABASE dando à role o privilégio de conexão ao appdb

```text

appdb=# GRANT CONNECT ON DATABASE appdb TO app_readonly;
GRANT
appdb=# 
```

3. Segundo nível: SCHEMA permitindo que a role utilize o schema app
```text
appdb=# GRANT USAGE ON SCHEMA app TO app_readonly;
GRANT
appdb=# 
```

4.Terceiro nível: TABLE  permitindo que pp_readonly possa consultar app.pessoas.
```text
appdb=# GRANT SELECT ON TABLE app.pessoas TO app_readonly;
GRANT
appdb=# 
```
5. Verificar novamente o DATABASE de duas formas
```text
appdb=# \l+ appdb
                                                                          Lista
 de bancos de dados
 Nome  |  Dono   | Codificação | Provedor de localidade |  Ordenação  |    Ctyp
e    | Locale | Regras ICU | Privilégios de acesso  | Tamanho | Espaço de tabel
as | Descrição 
-------+---------+-------------+------------------------+-------------+--------
-----+--------+------------+------------------------+---------+----------------
---+-----------
 appdb | appuser | UTF8        | libc                   | pt_BR.UTF-8 | pt_BR.U
TF-8 |        |            | =Tc/appuser           +| 7862 kB | pg_default     
   | 
       |         |             |                        |             |        
     |        |            | appuser=CTc/appuser   +|         |                
   | 
       |         |             |                        |             |        
     |        |            | app_readonly=c/appuser |         |                
   | 
(1 linha)

appdb=# 
```

```text
appdb=# SELECT has_database_privilege(
    'app_readonly',
    'appdb',
    'CONNECT'
);
 has_database_privilege 
------------------------
 t
(1 linha)

appdb=# 
```

6. Verificando o SCHEMA de duas formas.
```text
appdb=# \dn+ app
                  Lista de esquemas
 Nome |  Dono   | Privilégios de acesso  | Descrição 
------+---------+------------------------+-----------
 app  | appuser | appuser=UC/appuser    +| 
      |         | app_readonly=U/appuser | 
(1 linha)

appdb=# 
```

```text

appdb=# SELECT has_schema_privilege(
    'app_readonly',
    'app',
    'USAGE'
);
 has_schema_privilege 
----------------------
 t
(1 linha)

appdb=# 
```

7. Verificar novamente a TABLE de duas formas
```text
appdb=# \dp app.pessoas
                                    Privilégios de acesso
 Esquema |  Nome   |  Tipo  |   Privilégios de acesso    | Privilégios de coluna | Políticas 
---------+---------+--------+----------------------------+-----------------------+-----------
 app     | pessoas | tabela | postgres=arwdDxtm/postgres+|                       | 
         |         |        | appuser=arwd/postgres     +|                       | 
         |         |        | app_readonly=r/postgres    |                       | 
(1 linha)

appdb=# 
```

```text

appdb=# SELECT
    grantee,
    table_schema,
    table_name,
    privilege_type
FROM information_schema.role_table_grants
WHERE grantee = 'app_readonly'
  AND table_schema = 'app'
  AND table_name = 'pessoas'
ORDER BY privilege_type;
   grantee    | table_schema | table_name | privilege_type 
--------------+--------------+------------+----------------
 app_readonly | app          | pessoas    | SELECT
(1 linha)

appdb=# 
```


8. Testarndocomo os privilégios da role app_readonly se comportam.
```text
appdb=# SET ROLE app_readonly;
SET
appdb=> 
```

```text

appdb=# SET ROLE app_readonly;
SET
appdb=> SELECT current_user;
 current_user 
--------------
 app_readonly
(1 linha)

appdb=>
```

9. Testando novamente SELECT
```text

 appdb=> SELECT * FROM app.pessoas;
 id |      nome      |          email           | data_nascimento 
----+----------------+--------------------------+-----------------
  2 | Maria Teste    | joao.teste@email.com     | 1992-05-10
  1 | Outro João     | outro.joao@email.com     | 1991-02-02
  7 | Bruno Silva    | bruno.silva@email.com    | 1988-07-22
  8 | Carla Oliveira | carla.oliveira@email.com | 2000-11-05
  9 | Daniel Santos  | daniel.santos@email.com  | 1992-01-30
  6 | Ana Souza      | ana.souza.novo@email.com | 1995-03-15
(6 linhas)

appdb=> 
```

10. Testando UPDATE
```text

appdb=> UPDATE app.pessoas
SET email = 'teste@teste.com'
WHERE id = 1;
ERRO:  permissão negada para tabela pessoas
appdb=> 
```
Significa:

app_readonly não possui UPDATE.

11. Testando INSERT
```text
appdb=> INSERT INTO app.pessoas
(nome, email, data_nascimento)
VALUES
('Teste Readonly', 'teste.readonly@email.com', '1990-01-01');
ERRO:  permissão negada para tabela pessoas
appdb=> 
```
ignifica:

app_readonly não possui INSERT


12. Testando DELETE
```text
appdb=> DELETE FROM app.pessoas
WHERE id = 1;
ERRO:  permissão negada para tabela pessoas
appdb=> 
```
Significa:

app_readonly não possui DELETE



# Atividade 4 - Quem pode entrar? Conhecendo o pg_hba.conf

1. Confirmando que estou conectado como postgres
```bash
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

2. Verificando se appreader já existe
```bash
appdb=# SELECT
    rolname,
    rolcanlogin,
    rolinherit
FROM pg_roles
WHERE rolname = 'appreader';
 rolname | rolcanlogin | rolinherit 
---------+-------------+------------
(0 linha)

appdb=# 
```

3. Criando appreader
```bash

appdb=# CREATE ROLE appreader LOGIN;
CREATE ROLE
appdb=#
```

4. Colocando uma senha no appreader
```bash
appdb=# \password appreader
Digite a nova senha para o usuário "appreader": 
Digite novamente: 
appdb=#
```
5. Verificar a criação
```bash
appdb=# SELECT
    rolname,
    rolcanlogin,
    rolinherit,
    rolsuper,
    rolcreatedb,
    rolcreaterole
FROM pg_roles
WHERE rolname = 'appreader';
  rolname  | rolcanlogin | rolinherit | rolsuper | rolcreatedb | rolcreaterole 
-----------+-------------+------------+----------+-------------+---------------
 appreader | t           | t          | f        | f           | f
(1 linha)

appdb=# 
```

6. Colocando appreader para pertence ao grupo app_readonly (MEMBERSHIP).
```bash
appdb=# GRANT app_readonly TO appreader;
GRANT ROLE
appdb=# 
```

7. Verificando a membership
```bash

appdb=# \du appreader
     Lista de funções de banco de dados (roles)
 Nome da função de banco de dados (role) | Atributos 
-----------------------------------------+-----------
 appreader                               | 

appdb=# 

```

8. Descobrindo se a herança realmente funciona através de testes
```bash
appdb=# \q
[postgres@localhost ~]$ psql -h localhost -U appreader -d appdb
Senha para o usuário appreader: 
psql (17.11)
Digite "help" para obter ajuda.

appdb=>
```
9. Descobrindo quem somos
```bash

    appdb=> SELECT current_user, session_user, current_database();
 current_user | session_user | current_database 
--------------+--------------+------------------
 appreader    | appreader    | appdb
(1 linha)

appdb=>

```
10. Testando o SELECT

```bash
appdb=> SELECT * FROM app.pessoas;
 id |      nome      |          email           | data_nascimento 
----+----------------+--------------------------+-----------------
  2 | Maria Teste    | joao.teste@email.com     | 1992-05-10
  1 | Outro João     | outro.joao@email.com     | 1991-02-02
  7 | Bruno Silva    | bruno.silva@email.com    | 1988-07-22
  8 | Carla Oliveira | carla.oliveira@email.com | 2000-11-05
  9 | Daniel Santos  | daniel.santos@email.com  | 1992-01-30
  6 | Ana Souza      | ana.souza.novo@email.com | 1995-03-15
(6 linhas)

appdb=> 
```

11. Testando INSERT
```bash
appdb=> INSERT INTO app.pessoas
(nome, email, data_nascimento)
VALUES
('Teste Appreader', 'teste.appreader@email.com', '1990-01-01');
ERRO:  permissão negada para tabela pessoas
appdb=> 
```
Porque app_readonly não possui INSERT

12. Testando UPDATE

```bash
appdb=> UPDATE app.pessoas
SET email = 'teste.appreader@email.com'
WHERE id = 1;
ERRO:  permissão negada para tabela pessoas
appdb=> 
```

13. Testando DELETE
```bash
appdb=> DELETE FROM app.pessoas
WHERE id = 1;
ERRO:  permissão negada para tabela pessoas
appdb=>
```

14.
14. 
