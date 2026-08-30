# dbanapratica

# Quinzena 9 - Administracao basica do PostgreSQL

**Tema:** Instancia, `psql`, roles, databases, autenticacao e permissoes

**Sistema Operacional:** Rocky Linux

**Dedicacao:** aproximadamente 30 minutos por dia

---

# Objetivo da quinzena

Na quinzena anterior voce instalou o PostgreSQL, iniciou o servico e criou seu primeiro banco de dados.

Agora vamos comecar a administrar o PostgreSQL.

Nesta quinzena voce vai aprender a responder a uma pergunta que sera muito importante durante toda a sua formacao:

> "Por que determinado usuario consegue ou nao consegue acessar determinado banco de dados?"

Para isso, vamos introduzir:

- `psql`;
- instancia PostgreSQL;
- roles;
- databases;
- autenticacao;
- `pg_hba.conf`;
- privilegios basicos;
- `GRANT` e `REVOKE`.

Ao final da quinzena voce devera conseguir criar uma role, preparar um database para ela, permitir sua conexao, testar o acesso e explicar o que esta acontecendo quando uma conexao e aceita ou recusada.

---

# Como estudar

Nao e necessario memorizar os comandos.

Para cada atividade:

1. Leia o material relacionado ao assunto.
2. Pesquise utilizando as palavras-chave fornecidas.
3. Tente realizar a tarefa no ambiente de laboratorio.
4. Se encontrar um erro, procure entender o motivo antes de tentar corrigi-lo.
5. Registre no `diario.md` o que descobriu.

Uma boa parte do aprendizado desta quinzena acontecera justamente quando algo nao funcionar.

---

# Material de apoio

## Documentacao principal

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

Para conteudos em portugues, pesquise no YouTube por:

- PostgreSQL para iniciantes
- PostgreSQL administracao
- PostgreSQL usuarios e roles
- PostgreSQL pg_hba.conf
- PostgreSQL autenticacao
- PostgreSQL permissoes
- PostgreSQL GRANT REVOKE

Nao e necessario assistir varios videos sobre o mesmo assunto.

Escolha um material que voce consiga acompanhar e utilize a documentacao oficial para complementar o estudo.

---

# Diario de bordo

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

Quando encontrar um erro interessante, registre tambem a mensagem de erro.

---

# Atividade 1 - Retomando o PostgreSQL

**Nivel:** Basico

**Dificuldade:** Baixa

---

## Enunciado

Voce ficou algumas semanas sem trabalhar com o servidor PostgreSQL.

Antes de comecar uma nova configuracao, voce precisa verificar se ainda consegue administrar a instalacao criada anteriormente.

Entre no servidor e tente realizar as tarefas sem consultar imediatamente o material da quinzena anterior.

Voce devera:

- verificar se o PostgreSQL esta instalado;
- identificar sua versao;
- verificar o estado do servico;
- acessar o PostgreSQL utilizando o `psql`;
- listar os databases existentes;
- localizar o database `laboratorio`;
- conectar-se a ele;
- descobrir qual usuario/role esta sendo utilizado na sessao;
- sair do `psql`.

Se nao lembrar como realizar alguma tarefa, pesquise.

O objetivo nao e lembrar todos os comandos utilizados anteriormente. O objetivo e verificar se voce consegue recuperar a informacao necessaria.

---

## Palavras-chave para pesquisa

- PostgreSQL psql
- PostgreSQL list databases
- PostgreSQL current user
- PostgreSQL service systemctl
- psql meta commands

---

## Ao concluir esta atividade voce devera ser capaz de:

- verificar se uma instancia PostgreSQL esta funcionando;
- acessar o PostgreSQL utilizando o `psql`;
- identificar o database utilizado;
- identificar a role da sessao;
- utilizar a documentacao para recuperar informacoes esquecidas.

---

## Checklist

- [ ] Servico verificado.
- [ ] Versao identificada.
- [ ] `psql` utilizado.
- [ ] Databases listados.
- [ ] `laboratorio` localizado.
- [ ] Conexao com `laboratorio` realizada.
- [ ] Usuario/role da sessao identificado.
- [ ] Diario de bordo atualizado.

---

# Atividade 2 - Roles nao sao usuarios Linux

**Nivel:** Basico

**Dificuldade:** Baixa

---

## Enunciado

Na administracao Linux voce ja trabalhou com usuarios e grupos.

Agora vamos descobrir como esse conceito aparece no PostgreSQL.

Investigue as seguintes questoes:

- O que e uma role no PostgreSQL?
- Por que o PostgreSQL utiliza o termo "role"?
- Existe uma role chamada `postgres`?
- Existe um usuario Linux chamado `postgres`?
- Qual e a relacao entre eles?
- Uma role PostgreSQL precisa obrigatoriamente ter um usuario Linux correspondente?
- Uma pessoa pode utilizar uma role PostgreSQL para acessar o banco sem possuir uma conta Linux no servidor?

Depois da pesquisa, utilize seu ambiente para identificar:

- as roles existentes;
- quais delas podem realizar login;
- quais possuem privilegios administrativos.

Nao crie nenhuma role ainda.

O objetivo desta atividade e construir o modelo mental antes de comecar a criar usuarios.

---

## Palavras-chave para pesquisa

- PostgreSQL role
- PostgreSQL database roles
- PostgreSQL role LOGIN
- PostgreSQL superuser
- PostgreSQL list roles
- Linux user PostgreSQL role

---

## Ao concluir esta atividade voce devera ser capaz de:

- explicar o conceito de role;
- diferenciar uma role PostgreSQL de um usuario Linux;
- listar as roles existentes;
- identificar caracteristicas basicas de uma role;
- reconhecer uma role com privilegios administrativos.

---

## Checklist

- [ ] Roles existentes identificadas.
- [ ] Role `postgres` investigada.
- [ ] Usuario Linux `postgres` investigado.
- [ ] Diferenca entre os dois compreendida.
- [ ] Roles com login identificadas.
- [ ] Roles administrativas identificadas.
- [ ] Diario de bordo atualizado.

---

# Atividade 3 - Criando uma role e um database

**Nivel:** Intermediario

**Dificuldade:** Moderada

---

## Enunciado

Agora voce recebeu uma solicitacao para preparar um ambiente simples para uma aplicacao.

A aplicacao utilizara:

- Role: `appuser`
- Database: `appdb`

Sua tarefa sera criar esses dois recursos.

A role `appuser` devera:

- poder realizar login;
- possuir uma senha;
- nao possuir privilegios de superusuario.

O database `appdb` devera ser criado de forma que `appuser` possa utiliza-lo.

Antes de executar os comandos, pesquise:

- como criar uma role;
- como habilitar login;
- como definir uma senha;
- como criar um database;
- como definir o proprietario de um database;
- como verificar as propriedades de uma role;
- como verificar as propriedades de um database.

Depois de criar os recursos, faca uma primeira tentativa de conexao utilizando `appuser`.

Registre o resultado.

Se a conexao funcionar, explique por que.

Se nao funcionar, nao tente simplesmente contornar o problema. Registre o erro e investigue o motivo.

---

## Palavras-chave para pesquisa

- PostgreSQL CREATE ROLE
- PostgreSQL CREATE DATABASE
- PostgreSQL role LOGIN
- PostgreSQL database owner
- PostgreSQL ALTER ROLE
- PostgreSQL psql connect database

---

## Ao concluir esta atividade voce devera ser capaz de:

- criar uma role;
- configurar uma role para realizar login;
- definir uma senha;
- criar um database;
- definir o proprietario de um database;
- consultar as propriedades desses recursos;
- realizar uma primeira investigacao de problemas de conexao.

---

## Checklist

- [ ] `appuser` criada.
- [ ] Login habilitado.
- [ ] Senha configurada.
- [ ] Superusuario nao habilitado.
- [ ] `appdb` criado.
- [ ] Proprietario identificado.
- [ ] Propriedades verificadas.
- [ ] Tentativa de conexao realizada.
- [ ] Resultado documentado.
- [ ] Diario de bordo atualizado.

---

# Atividade 4 - Quem pode entrar? Conhecendo o pg_hba.conf

**Nivel:** Intermediario

**Dificuldade:** Moderada

---

## Enunciado

Voce criou `appuser` e `appdb`.

Agora chegou o momento de entender como o PostgreSQL decide se uma tentativa de conexao deve ser aceita ou recusada.

O PostgreSQL utiliza o arquivo:

`pg_hba.conf`

Esse arquivo contem regras de autenticacao para as conexoes realizadas contra a instancia.

Sua primeira tarefa sera localizar o arquivo utilizado pela sua instalacao.

Depois, pesquise e responda:

- Para que serve o `pg_hba.conf`?
- Onde ele esta localizado?
- Como descobrir qual arquivo `pg_hba.conf` esta sendo utilizado?
- O que significa uma regra `local`?
- O que significa uma regra `host`?
- O que representa o campo de database?
- O que representa o campo de user?
- O que representa o endereco de origem?
- O que representa o metodo de autenticacao?
- O que significa `peer`?
- O que significa `scram-sha-256`?
- O que significa `trust`?
- O PostgreSQL analisa todas as regras ate encontrar uma que funcione ou existe outra logica?

Nao altere o arquivo imediatamente.

Primeiro leia as regras existentes e tente entender o que elas significam.

Depois documente sua interpretacao.

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

## Ao concluir esta atividade voce devera ser capaz de:

- localizar o `pg_hba.conf`;
- explicar para que ele serve;
- interpretar uma regra basica;
- diferenciar `local` e `host`;
- compreender database, user e address em uma regra;
- explicar o conceito basico dos metodos `peer`, `scram-sha-256` e `trust`;
- compreender que a ordem das regras e importante.

---

## Checklist

- [ ] `pg_hba.conf` localizado.
- [ ] Localizacao confirmada pelo PostgreSQL.
- [ ] Regras existentes analisadas.
- [ ] `local` compreendido.
- [ ] `host` compreendido.
- [ ] Database/user/address compreendidos.
- [ ] Metodos de autenticacao pesquisados.
- [ ] Ordem das regras compreendida.
- [ ] Diario de bordo atualizado.

---

# Atividade 5 - Fazendo uma conexao funcionar

**Nivel:** Intermediario

**Dificuldade:** Desafiadora

---

## Enunciado

Agora voce devera juntar os conhecimentos adquiridos.

Voce possui:

- uma instancia PostgreSQL;
- uma role `appuser`;
- um database `appdb`;
- um arquivo `pg_hba.conf`.

Seu objetivo e permitir que `appuser` consiga realizar uma conexao autenticada com `appdb`.

Primeiro tente realizar a conexao utilizando `appuser`.

Se ela for recusada, investigue o motivo.

Analise:

- a role;
- o database;
- as regras do `pg_hba.conf`;
- o metodo de autenticacao utilizado;
- as permissoes existentes.

Depois faca a alteracao necessaria no `pg_hba.conf`.

O acesso devera utilizar autenticacao por senha.

Apos a alteracao:

1. teste novamente;
2. confirme que `appuser` consegue acessar `appdb`;
3. registre qual regra do `pg_hba.conf` permitiu a conexao;
4. explique por que a conexao anterior foi recusada;
5. registre como a alteracao foi aplicada ao PostgreSQL.

Nao utilize `trust` para resolver o problema.

O objetivo e compreender o processo de autenticacao, e nao apenas fazer a conexao funcionar.

---

## Palavras-chave para pesquisa

- PostgreSQL pg_hba.conf password authentication
- PostgreSQL scram-sha-256
- PostgreSQL reload configuration
- PostgreSQL pg_hba.conf connection refused
- PostgreSQL authentication error

---

## Ao concluir esta atividade voce devera ser capaz de:

- diagnosticar uma falha simples de autenticacao;
- relacionar uma tentativa de conexao com uma regra do `pg_hba.conf`;
- configurar uma regra basica de autenticacao por senha;
- aplicar uma alteracao de configuracao;
- testar uma conexao novamente;
- explicar por que uma conexao foi aceita ou recusada.

---

## Checklist

- [ ] `appuser` testada.
- [ ] `appdb` testado.
- [ ] Falha de conexao investigada.
- [ ] Regra adequada identificada.
- [ ] Autenticacao por senha configurada.
- [ ] Alteracao aplicada.
- [ ] Conexao realizada com sucesso.
- [ ] Motivo da falha anterior explicado.
- [ ] Diario de bordo atualizado.

---

# Atividade 6 - Autenticacao nao é permissão

**Nivel:** Intermediario

**Dificuldade:** Moderada

---

## Enunciado

Agora que `appuser` consegue se conectar ao `appdb`, uma nova duvida aparece:

> Se `appuser` conseguiu entrar no database, significa que pode fazer qualquer coisa dentro dele?

A resposta devera ser investigada na pratica.

Utilizando `appuser`, verifique o que ela consegue fazer.

Depois pesquise:

- o que e autenticacao;
- o que e autorizacao;
- o que sao privilegios;
- o que e `GRANT`;
- o que e `REVOKE`;
- qual a diferenca entre ser proprietario e possuir um privilegio concedido;
- quais privilegios existem para databases.

Documente a diferenca entre:

```text
Autenticacao
     |
     +-- posso entrar?

Autorizacao
     |
     +-- o que posso fazer depois que entrei?
```

Nao e necessario criar tabelas nesta atividade.

O objetivo e entender a diferenca entre conseguir estabelecer uma conexao e possuir autorizacao para realizar determinadas operacoes.

---

## Palavras-chave para pesquisa

- PostgreSQL authentication authorization
- PostgreSQL privileges
- PostgreSQL GRANT
- PostgreSQL REVOKE
- PostgreSQL database privileges
- PostgreSQL CONNECT privilege

---

## Ao concluir esta atividade voce devera ser capaz de:

- diferenciar autenticacao de autorizacao;
- compreender o conceito de privilegio;
- explicar a funcao de `GRANT`;
- explicar a funcao de `REVOKE`;
- compreender que conseguir conectar-se nao significa possuir todos os privilegios.

---

## Checklist

- [ ] Autenticacao compreendida.
- [ ] Autorizacao compreendida.
- [ ] Privilegios pesquisados.
- [ ] `GRANT` pesquisado.
- [ ] `REVOKE` pesquisado.
- [ ] Privilegios de database pesquisados.
- [ ] Diferenca entre autenticacao e autorizacao documentada.
- [ ] Diario de bordo atualizado.

---

# Entregaveis

Ao final da quinzena voce devera apresentar:

- [ ] `diario.md` atualizado.
- [ ] Role `appuser`.
- [ ] Database `appdb`.
- [ ] Configuracao de autenticacao documentada.
- [ ] Teste de conexao utilizando `appuser`.
- [ ] Explicacao sobre a regra do `pg_hba.conf`.
- [ ] Explicacao sobre autenticacao e autorizacao.
- [ ] Resumo da quinzena.

---

# Resumo obrigatorio

Escreva um texto com no maximo 20 linhas respondendo:

1. O que voce aprendeu nesta quinzena?
2. O que foi mais importante na diferenca entre usuarios Linux e roles PostgreSQL?
3. O que e uma role?
4. O que e um database?
5. O que e o `pg_hba.conf`?
6. Como o PostgreSQL decide se uma conexao sera aceita?
7. O que significa autenticacao?
8. O que significa autorizacao?
9. Qual a diferenca entre conseguir conectar e possuir permissao?
10. Qual foi a maior dificuldade encontrada?
11. O que voce ainda precisa estudar melhor?

---

# Perguntas para avaliacao

Durante nosso proximo encontro, esteja preparado para responder:

1. O que e uma role no PostgreSQL?
2. Qual a diferenca entre uma role PostgreSQL e um usuario Linux?
3. O que e o usuario `postgres` no Linux?
4. O que e o `psql`?
5. O que e um database?
6. O que e o `pg_hba.conf`?
7. Para que serve o `pg_hba.conf`?
8. Qual a diferenca entre uma conexao `local` e uma conexao `host`?
9. O que representam database, user e address em uma regra?
10. O que significa o metodo `peer`?
11. O que significa `scram-sha-256`?
12. Por que a ordem das regras do `pg_hba.conf` e importante?
13. O que acontece quando uma conexao nao corresponde a uma regra adequada?
14. Qual a diferenca entre autenticacao e autorizacao?
15. Qual a funcao do `GRANT`?
16. Qual a funcao do `REVOKE`?
17. Se uma role consegue conectar ao database, ela necessariamente pode criar tabelas?
18. Se `appuser` nao consegue conectar ao `appdb`, quais coisas voce investigaria primeiro?
19. Qual foi o problema de conexao que voce encontrou e como resolveu?
20. Explique, com suas proprias palavras, o caminho percorrido por uma conexao ate ela ser aceita pelo PostgreSQL.

---

# Criterio de aprovacao/reprovacao

## PostgreSQL e psql

- [ ] Consegue verificar o estado da instancia.
- [ ] Consegue acessar utilizando `psql`.
- [ ] Consegue identificar database e role da sessao.

**Aprovacao:** consegue realizar as tarefas sem depender de orientacao passo a passo.

**Reprovacao:** nao consegue acessar ou identificar os elementos basicos da instancia mesmo apos pesquisa.

---

## Roles

- [ ] Compreende o conceito de role.
- [ ] Diferencia role PostgreSQL de usuario Linux.
- [ ] Consegue criar uma role.
- [ ] Consegue verificar suas propriedades.
- [ ] Compreende a diferenca entre uma role comum e uma role administrativa.

**Aprovacao:** consegue explicar a diferenca e criar uma role comum corretamente.

**Reprovacao:** trata usuario Linux e role PostgreSQL como sendo a mesma entidade ou concede privilegios administrativos sem compreender sua finalidade.

---

## Databases

- [ ] Consegue criar um database.
- [ ] Compreende o conceito de proprietario.
- [ ] Consegue identificar o proprietario de um database.

**Aprovacao:** consegue criar e identificar corretamente um database e seu proprietario.

**Reprovacao:** nao consegue explicar a relacao entre role, database e proprietario.

---

## Autenticacao

- [ ] Sabe localizar o `pg_hba.conf`.
- [ ] Compreende sua finalidade.
- [ ] Consegue interpretar uma regra basica.
- [ ] Diferencia `local` e `host`.
- [ ] Compreende o conceito basico de `peer` e `scram-sha-256`.
- [ ] Compreende a importancia da ordem das regras.
- [ ] Consegue realizar uma alteracao simples e aplica-la.

**Aprovacao:** consegue explicar por que uma conexao foi aceita ou recusada e identificar a regra envolvida.

**Reprovacao:** altera regras sem compreender seu efeito ou nao consegue relacionar a regra do `pg_hba.conf` com o resultado da conexao.

---

## Permissoes

- [ ] Diferencia autenticacao de autorizacao.
- [ ] Compreende o conceito de privilegio.
- [ ] Compreende a funcao de `GRANT`.
- [ ] Compreende a funcao de `REVOKE`.

**Aprovacao:** consegue explicar por que uma role pode ou nao realizar determinada operacao.

**Reprovacao:** confunde autenticacao com autorizacao ou nao consegue explicar o efeito de uma permissao.

---

## Autonomia

- [ ] Pesquisa antes de solicitar ajuda.
- [ ] Consulta a documentacao.
- [ ] Consegue interpretar mensagens de erro.
- [ ] Registra as dificuldades no diario.
- [ ] Consegue explicar o que fez e por que fez.
- [ ] Nao depende apenas da memorizacao de comandos.

**Aprovacao:** demonstra capacidade de pesquisar e chegar a uma solucao.

**Reprovacao:** depende constantemente de instrucoes passo a passo para tarefas que ja foram trabalhadas.

---

# Resultado esperado da quinzena

Ao terminar esta quinzena, voce devera conseguir explicar o seguinte cenario:

```text
Usuario/cliente
       |
       | tenta conectar
       v
PostgreSQL
       |
       | verifica regras do
       | pg_hba.conf
       v
Autenticacao
       |
       | conexao aceita
       v
Role
       |
       | possui determinados
       | privilegios
       v
Database
       |
       v
Objetos e operacoes permitidas
```

A ideia principal desta quinzena e comecar a desenvolver o raciocinio de um administrador PostgreSQL:

> Quem esta tentando acessar?

> Para qual database?

> A conexao esta sendo autenticada?

> Qual regra do `pg_hba.conf` esta sendo utilizada?

> Qual role esta sendo utilizada?

> Quais privilegios essa role possui?

> O problema esta na autenticacao ou na autorizacao?

Esse raciocinio será utilizado nas proximas quinzenas.
```
