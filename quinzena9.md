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


