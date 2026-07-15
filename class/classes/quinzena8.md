# dbanapratica

# Missão 1 - Instalando seu primeiro servidor PostgreSQL

**Período:** 05/07 a 07/08

**Tema:** Instalação do PostgreSQL no Rocky Linux

**Sistema Operacional:** Rocky Linux

---

# Objetivo da missão

Ao final desta missão você deverá ser capaz de:

- preparar um servidor Rocky Linux para receber o PostgreSQL;
- instalar o PostgreSQL utilizando o repositório oficial (PGDG);
- administrar o serviço do PostgreSQL;
- acessar o PostgreSQL utilizando o `psql`;
- criar seu primeiro banco de dados;
- localizar os principais arquivos da instalação;
- documentar todo o procedimento realizado.

Nesta missão ainda não estudaremos SQL nem administração avançada do PostgreSQL. O objetivo é compreender como o PostgreSQL é instalado e administrado como uma aplicação Linux.

---

# Como estudar

Antes de iniciar qualquer atividade:

1. Leia a documentação oficial relacionada ao assunto.
2. Assista a pelo menos um vídeo sobre o tema.
3. Pesquise utilizando as palavras-chave sugeridas.
4. Tente resolver o problema antes de procurar ajuda.
5. Registre todas as dúvidas e descobertas no diário de bordo.

O objetivo desta mentoria é desenvolver autonomia na pesquisa, interpretação da documentação e resolução de problemas.

---

# Material de apoio

## Documentação

PostgreSQL

https://www.postgresql.org/docs/

Rocky Linux

https://docs.rockylinux.org/

## Vídeos

Pesquisar no YouTube por:

- Bóson Treinamentos PostgreSQL
- Bóson Treinamentos DNF
- LinuxTips PostgreSQL
- LinuxTips Rocky Linux

## Fontes complementares

Quando necessário, utilize também:

- PostgreSQL Documentation
- Rocky Linux Documentation
- Stack Overflow
- DBA Stack Exchange

Evite copiar comandos sem compreender sua finalidade.

---

# Diário de bordo

Durante toda a missão mantenha um arquivo chamado:

`diario.md`

Sempre que encontrar uma dificuldade registre:

- Data
- Atividade
- Problema encontrado
- Como resolveu
- Links consultados
- O que aprendeu

Ao final da missão esse diário fará parte da avaliação.

---

# Atividade 1 - Preparando o servidor

**Nível:** Básico

**Dificuldade:** Baixa

---

## Enunciado

Você recebeu um servidor Rocky Linux recém-instalado e foi solicitado a prepará-lo para receber uma nova aplicação.

Antes de instalar o PostgreSQL, é importante conhecer o ambiente e garantir que ele esteja atualizado e pronto para uso.

Ao final desta atividade responda às seguintes perguntas:

- Qual versão do Rocky Linux está instalada?
- Qual versão do kernel está sendo utilizada?
- O sistema possui atualizações disponíveis?
- Como atualizar todos os pacotes do sistema?
- Como listar os repositórios configurados?
- Como obter informações sobre um pacote antes de instalá-lo?
- Instale uma ferramenta de sua escolha (`tree` ou `htop`) e explique por que ela pode ser útil para um administrador de sistemas.

Documente todo o procedimento realizado.

---

## Palavras-chave para pesquisa

- dnf update
- dnf upgrade
- dnf repolist
- dnf info
- rocky linux version
- uname

---

## Ao concluir esta atividade você deverá ser capaz de:

- atualizar um servidor Rocky Linux;
- consultar informações do sistema operacional;
- compreender o funcionamento básico do DNF;
- consultar repositórios configurados;
- instalar aplicações utilizando o gerenciador de pacotes;
- documentar procedimentos técnicos.

---

## Checklist

- [ ] Sistema atualizado.
- [ ] Versão do Rocky Linux identificada.
- [ ] Versão do kernel identificada.
- [ ] Repositórios consultados.
- [ ] Ferramenta instalada.
- [ ] Diário de bordo atualizado.

---

# Atividade 2 - Instalando o PostgreSQL

**Nível:** Intermediário

**Dificuldade:** Moderada

---

## Enunciado

Com o servidor preparado, chegou o momento de instalar o PostgreSQL.

Nesta mentoria utilizaremos o repositório oficial do PostgreSQL (PGDG), amplamente utilizado em ambientes profissionais para disponibilizar versões atualizadas do PostgreSQL.

Pesquise como adicionar esse repositório ao Rocky Linux e realize a instalação da versão estável mais recente do PostgreSQL.

Após concluir a instalação responda:

- O PostgreSQL foi instalado corretamente?
- Quais são os principais pacotes instalados?
- Qual a finalidade dos principais pacotes?
- Qual usuário foi criado automaticamente?
- Qual grupo foi criado?
- Como verificar a versão instalada do PostgreSQL?
- Como confirmar que a instalação foi concluída com sucesso?

Documente todas as etapas realizadas.

---

## Palavras-chave para pesquisa

- PostgreSQL PGDG
- PostgreSQL Rocky Linux
- PostgreSQL install Rocky Linux
- PostgreSQL repository
- PostgreSQL packages
- PostgreSQL version

---

## Ao concluir esta atividade você deverá ser capaz de:

- compreender por que utilizamos o repositório oficial do PostgreSQL;
- adicionar um novo repositório ao Rocky Linux;
- instalar o PostgreSQL utilizando o DNF;
- identificar os principais componentes da instalação;
- verificar se a instalação foi concluída com sucesso;
- documentar todo o procedimento realizado.

---

## Checklist

- [ ] Repositório PGDG configurado.
- [ ] PostgreSQL instalado.
- [ ] Principais pacotes identificados.
- [ ] Usuário identificado.
- [ ] Grupo identificado.
- [ ] Versão identificada.
- [ ] Instalação validada.
- [ ] Diário de bordo atualizado.

---

# Atividade 3 - Colocando o PostgreSQL em funcionamento

**Nível:** Intermediário

**Dificuldade:** Moderada

---

## Enunciado

O PostgreSQL já está instalado, porém uma aplicação instalada ainda não significa que ela esteja pronta para uso.

Nesta atividade você deverá colocar o PostgreSQL em funcionamento e realizar o primeiro acesso ao banco de dados.

Pesquise como:

- iniciar o serviço do PostgreSQL;
- verificar se o serviço está em execução;
- configurar o serviço para iniciar automaticamente junto com o sistema operacional;
- acessar o PostgreSQL utilizando o usuário administrativo criado durante a instalação;
- abrir o terminal interativo (`psql`);
- listar os bancos de dados existentes;
- criar um banco de dados chamado **laboratorio**;
- confirmar que o banco foi criado corretamente;
- encerrar a sessão do `psql`.

Nesta etapa **não** crie tabelas, usuários, schemas ou qualquer outro objeto além do banco de dados solicitado.

Documente todo o procedimento realizado.

---

## Palavras-chave para pesquisa

- systemctl postgresql
- enable postgresql service
- psql
- create database
- list databases postgresql

---

## Ao concluir esta atividade você deverá ser capaz de:

- iniciar e interromper o serviço do PostgreSQL;
- verificar se o serviço está funcionando corretamente;
- configurar o serviço para iniciar automaticamente;
- acessar o PostgreSQL utilizando o `psql`;
- listar bancos de dados existentes;
- criar um novo banco de dados;
- compreender a diferença entre instalar uma aplicação e colocá-la em funcionamento.

---

## Checklist

- [ ] Serviço iniciado.
- [ ] Serviço configurado para iniciar automaticamente.
- [ ] Status do serviço verificado.
- [ ] Acesso ao `psql` realizado.
- [ ] Banco `laboratorio` criado.
- [ ] Banco confirmado.
- [ ] Diário de bordo atualizado.

---

# Atividade 4 - Conhecendo a instalação

**Nível:** Intermediário

**Dificuldade:** Moderada

---

## Enunciado

Agora que o PostgreSQL está instalado e funcionando, chegou o momento de conhecer melhor sua estrutura.

Imagine que outro administrador assumirá este servidor futuramente. Para isso, será necessário documentar onde estão localizados os principais componentes da instalação.

Pesquise e responda:

- Onde estão localizados os executáveis do PostgreSQL?
- Onde ficam armazenados os arquivos de configuração?
- Onde ficam armazenados os bancos de dados?
- Onde ficam os arquivos de log?
- Qual serviço do systemd foi criado?
- Qual usuário é responsável pela execução do PostgreSQL?
- Quais diretórios você considera mais importantes para um administrador conhecer?

Nesta atividade **não altere nenhum arquivo de configuração**.

O objetivo é apenas conhecer a estrutura da instalação.

Documente todas as respostas encontradas.

---

## Palavras-chave para pesquisa

- PostgreSQL file layout
- PostgreSQL data directory
- PostgreSQL configuration files
- PostgreSQL log location
- PostgreSQL systemd

---

## Ao concluir esta atividade você deverá ser capaz de:

- compreender a organização dos arquivos do PostgreSQL;
- localizar os principais diretórios da instalação;
- identificar os arquivos de configuração;
- localizar os arquivos de log;
- identificar o serviço responsável pelo PostgreSQL;
- compreender a estrutura básica de uma instalação PostgreSQL.

---

## Checklist

- [ ] Binários localizados.
- [ ] Arquivos de configuração localizados.
- [ ] Diretório de dados identificado.
- [ ] Diretório de logs identificado.
- [ ] Serviço identificado.
- [ ] Usuário responsável identificado.
- [ ] Diário de bordo atualizado.

---

---

# Atividade 5 - Documentando a instalação

**Nível:** Básico

**Dificuldade:** Baixa

---

## Enunciado

Imagine que outro administrador da equipe precisará instalar um novo servidor PostgreSQL utilizando o mesmo procedimento realizado por você.

Sua tarefa será produzir um documento técnico que sirva como guia de instalação e consulta rápida.

Crie um arquivo chamado:

`instalacao-postgresql.md`

Esse documento deverá responder, de forma organizada, às seguintes perguntas:

- Como instalar o PostgreSQL?
- Como verificar se a instalação foi concluída com sucesso?
- Como iniciar o serviço?
- Como interromper o serviço?
- Como reiniciar o serviço?
- Como verificar o status do serviço?
- Como acessar o PostgreSQL utilizando o `psql`?
- Como listar os bancos existentes?
- Como criar um banco de dados?
- Onde ficam:
    - os arquivos de configuração;
    - os arquivos de log;
    - o diretório de dados;
    - os executáveis do PostgreSQL.

Ao escrever o documento, procure utilizar suas próprias palavras. O objetivo não é copiar a documentação oficial, mas produzir um material que seja útil para uma consulta rápida.

---

## Palavras-chave para pesquisa

- PostgreSQL documentation
- PostgreSQL administration
- PostgreSQL quick start

---

## Ao concluir esta atividade você deverá ser capaz de:

- organizar procedimentos técnicos;
- produzir documentação clara e objetiva;
- registrar informações importantes para futuras consultas;
- documentar uma instalação de forma reproduzível.

---

## Checklist

- [ ] Documento criado.
- [ ] Procedimento organizado.
- [ ] Informações conferidas.
- [ ] Linguagem clara.
- [ ] Diário de bordo atualizado.

---

# Entregáveis

Ao final desta missão você deverá entregar:

- [ ] PostgreSQL instalado e funcionando.
- [ ] Banco de dados `laboratorio` criado.
- [ ] Arquivo `instalacao-postgresql.md`.
- [ ] Arquivo `diario.md`.
- [ ] Evidências das principais etapas (capturas de tela ou registros dos comandos executados).
- [ ] Resumo da missão.

---

# Resumo da missão

Escreva um texto com no máximo 20 linhas respondendo às seguintes perguntas:

1. O que você aprendeu durante esta missão?
2. Qual atividade foi mais desafiadora?
3. Qual foi o maior problema encontrado e como ele foi resolvido?
4. O que mais chamou sua atenção na instalação do PostgreSQL?
5. Quais assuntos você acredita que ainda precisa estudar melhor?

---

# Perguntas para avaliação

Durante nosso próximo encontro, esteja preparado para responder às seguintes perguntas:

1. Por que utilizamos o repositório oficial (PGDG)?
2. Qual a diferença entre instalar um programa e colocá-lo em funcionamento?
3. Quais foram os principais pacotes instalados?
4. Como verificar se o PostgreSQL está em execução?
5. Como iniciar e interromper o serviço?
6. Como acessar o PostgreSQL utilizando o `psql`?
7. Como listar os bancos existentes?
8. Como criar um banco de dados?
9. Onde ficam armazenados os dados do PostgreSQL?
10. Onde ficam os arquivos de configuração?
11. Onde ficam os arquivos de log?
12. O que você faria se o PostgreSQL não iniciasse após reiniciar o servidor?
13. Qual foi o aprendizado mais importante desta missão?

---

# Critérios de aprovação

Ao final desta missão espera-se que você seja capaz de:

- [ ] Preparar um servidor Rocky Linux para receber o PostgreSQL.
- [ ] Instalar o PostgreSQL utilizando o repositório oficial.
- [ ] Colocar o PostgreSQL em funcionamento.
- [ ] Acessar o PostgreSQL utilizando o `psql`.
- [ ] Criar um banco de dados.
- [ ] Localizar os principais arquivos da instalação.
- [ ] Produzir documentação técnica.
- [ ] Demonstrar autonomia na pesquisa e resolução de problemas.

Caso algum dos objetivos acima não seja atingido, os assuntos correspondentes serão revisados antes do início da próxima missão.

---

# Próxima missão

Na próxima etapa da mentoria iniciaremos a administração básica do PostgreSQL.

Os principais assuntos serão:

- arquitetura do PostgreSQL;
- bancos de dados;
- roles e usuários;
- autenticação;
- arquivos `postgresql.conf` e `pg_hba.conf`;
- criação de tabelas;
- tipos de dados;
- primeiros comandos SQL.
