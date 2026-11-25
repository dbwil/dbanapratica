# Diário de bordo — Quinzena 12

Atividade 1: Conhecendo a arquitetura do PostgreSQL

Ambiente utilizado: Rocky Linux 9.7\
Versão do PostgreSQL: 17.11\
Banco de dados: appdb

## 1. Objetivo da atividade

Compreender a arquitetura interna do PostgreSQL, identificando o funcionamento da instância, do processo principal, dos processos auxiliares, dos processos responsáveis pelas conexões, da memória compartilhada e local, dos arquivos de dados e dos registros WAL.

## 2. Atividades realizadas

Durante a atividade, executei comandos no terminal Linux e consultas SQL no PostgreSQL para observar o funcionamento do servidor.

1\. Verificação do serviço PostgreSQL

Executei:

```
sudo systemctl status postgresql-17
```

Verifiquei que o serviço estava ativo e funcionando (`active (running)`). Identifiquei o processo principal com PID 999.

2\. Identificação dos processos PostgreSQL

Executei:

```
ps -ef | grep '[p]ostgres'
```

Identifiquei o processo principal e os processos auxiliares: logger, checkpointer, background writer, walwriter, autovacuum launcher e logical replication launcher.

3\. Análise da relação entre processos

Executei:

```
ps -o pid,ppid,stat,comm,args -C postgres
```

Observei a diferença entre PID e PPID e compreendi que os processos auxiliares possuem o processo principal como processo pai.

4\. Conexão com o banco de dados

Executei:

```
sudo -u postgres psql -d appdb
```

Acessei o banco `appdb` utilizando o cliente psql e realizei consultas para identificar informações da conexão.

5\. Identificação do processo backend

Executei:

```
SELECT
    current_user,
    current_database(),
    pg_backend_pid();
```

Identifiquei o usuário `postgres`, o banco `appdb` e o PID 6872, correspondente ao processo responsável por atender aquela conexão.

6\. Identificação dos arquivos de configuração

Executei:

```
SHOW data_directory;
SHOW config_file;
SHOW hba_file;
```

Identifiquei o diretório de dados e os arquivos `postgresql.conf` e `pg_hba.conf`.

7\. Verificação da memória compartilhada

Executei:

```
SHOW shared_buffers;
```

Identifiquei que o parâmetro estava configurado em 128 MB e compreendi sua utilização como área de cache de páginas de dados.

8\. Verificação do horário de inicialização

Executei:

```
SELECT pg_postmaster_start_time();
```

Identifiquei o horário de inicialização do servidor PostgreSQL.

9\. Consulta das atividades do servidor

Executei:

```
SELECT
    pid,
    usename,
    datname,
    application_name,
    state
FROM pg_stat_activity;
```

Observei as conexões e os processos internos, identificando informações como PID, usuário, banco de dados, aplicação e estado da sessão.


## 3. O que aprendi

Durante a atividade, compreendi que o PostgreSQL não funciona utilizando apenas um processo. Ele possui um processo principal responsável pela coordenação do servidor e diversos processos auxiliares especializados.

Aprendi a diferença entre PID e PPID, identifiquei como funciona o processo backend e entendi que cada conexão de cliente é atendida por um processo próprio no modelo observado.

Também compreendi a diferença entre memória compartilhada e memória local, a localização dos arquivos de dados e a importância do WAL para a recuperação e a segurança das transações.

Outro aprendizado importante foi diferenciar os comandos executados diretamente no terminal Linux daqueles executados dentro do ambiente do PostgreSQL (psql).

## 4. Dificuldades encontradas

Inicialmente, tive dúvidas para compreender a função de cada processo do PostgreSQL e a diferença entre o processo principal e os processos auxiliares.

Também precisei entender melhor a relação entre PID e PPID, além de interpretar as informações apresentadas pelos comandos de monitoramento.

## 5. Como resolvi as dificuldades

Resolvi as dúvidas analisando os resultados dos comandos executados, estudando o significado de cada parâmetro e relacionando as informações exibidas no Linux com as consultas realizadas dentro do PostgreSQL.

A explicação passo a passo permitiu compreender o funcionamento de cada comando e a finalidade das informações retornadas.

## 6. Conclusão

Concluo que a atividade foi importante para ampliar minha compreensão sobre a arquitetura interna do PostgreSQL.

Consegui identificar o processo principal, os processos auxiliares, o processo backend, o diretório de dados e os principais arquivos de configuração. Também compreendi conceitos fundamentais relacionados à memória e ao WAL.

A prática permitiu visualizar o PostgreSQL em funcionamento no Rocky Linux, tornando o aprendizado mais concreto e facilitando a compreensão de como o servidor gerencia seus recursos e conexões.



# DIÁRIO DE BORDO — QUINZENA 12


## Atividade 2 — Conhecendo os principais processos do PostgreSQL

### 1. Objetivo da atividade

Conhecer os principais processos que compõem uma instância PostgreSQL, compreender suas responsabilidades, identificar processos ativos no sistema operacional e diferenciar processos de conexão dos processos auxiliares.

### 2. Atividades realizadas

Inicialmente, consultei o estado do serviço PostgreSQL por meio do comando:

`sudo systemctl status postgresql-17`

Verifiquei que o serviço estava ativo e em execução. Identifiquei o processo principal com PID 999, além dos processos auxiliares apresentados pelo sistema.

Em seguida, executei:

`ps -ef | grep '[p]ostgres'`

Com esse comando, identifiquei os processos em execução e observei a relação entre o processo principal e seus processos filhos.

Depois, executei:

`ps -o pid,ppid,stat,comm,args -C postgres`

Essa consulta permitiu visualizar os identificadores PID e PPID, o estado dos processos e seus respectivos comandos.

### 3. Processos identificados

Foram encontrados os seguintes processos:

- PID 999: processo principal postgres.
- PID 1068: logger.
- PID 1090: checkpointer.
- PID 1091: background writer.
- PID 2091: walwriter.
- PID 2092: autovacuum launcher.
- PID 2093: logical replication launcher.

### 4. Experimento com client backend

Realizei um teste prático para identificar o processo responsável por atender uma conexão com o banco de dados.

Utilizei o comando:

`sudo -u postgres psql -d appdb`

Dentro do PostgreSQL, executei:

`SELECT pg_backend_pid();`

Posteriormente, consultei os processos no segundo terminal utilizando:

`ps -ef | grep '[p]ostgres'`

Identifiquei o processo com PID 12783, correspondente à conexão com o banco appdb.

Observei que seu PPID era 999, demonstrando que o processo de conexão era filho do processo principal do PostgreSQL.

Também observei que o backend apresentava o estado idle, indicando que a conexão estava aberta, mas aguardando novos comandos.

Na consulta posterior, após o encerramento da sessão, o PID 12783 não apareceu mais na listagem.

### 5. Conhecimentos adquiridos

Durante a atividade, compreendi que o PostgreSQL utiliza diferentes processos para executar tarefas específicas.

Aprendi a diferenciar:

- Processo principal: coordena a instância.
- Client backend: atende conexões de clientes.
- Checkpointer: executa pontos de verificação.
- Background writer: auxilia na gravação de páginas modificadas.
- Walwriter: grava registros WAL.
- Autovacuum launcher: coordena a manutenção automática.
- Autovacuum worker: executa operações de manutenção.
- Logger: gerencia mensagens de log.

Também compreendi a importância do PID e PPID para identificar os processos e suas relações no sistema operacional.

### 6. Dificuldades encontradas

Durante o experimento, digitei o comando `\q` diretamente no terminal Linux, recebendo a mensagem de que o comando não foi encontrado.

Compreendi que `\q` é um comando interno do PostgreSQL utilizado para sair do ambiente psql, enquanto o terminal Linux utiliza comandos próprios.

Outra dificuldade foi compreender a diferença entre o processo cliente psql e o processo servidor client backend.

Após analisar os resultados, entendi que o psql é o programa cliente e que o PostgreSQL cria um processo backend para atender à conexão.

### 7. Conclusão

A atividade permitiu compreender melhor a arquitetura interna do PostgreSQL e a divisão de responsabilidades entre seus processos.

Consegui identificar os principais processos ativos na minha instalação, analisar a relação entre processo principal e processos auxiliares e comprovar, por meio de um experimento prático, a criação de um client backend para atender uma conexão.

Também compreendi que nem todos os processos apresentados na documentação precisam estar ativos simultaneamente, pois alguns dependem de configurações específicas ou são iniciados sob demanda.

Concluo que o PostgreSQL utiliza uma arquitetura baseada em processos especializados, trabalhando em conjunto para garantir o funcionamento, a manutenção, a gravação e a recuperação dos dados.




# DIÁRIO DE BORDO — QUINZENA 12

## Atividade 3 — Observando os processos no Rocky Linux

### 1. Ambiente utilizado

- Sistema operacional: Rocky Linux 9.7
- Banco de dados: PostgreSQL 17.11
- Serviço: postgresql-17
- Banco utilizado: appdb
- Usuário Linux do PostgreSQL: postgres
- Usuário Linux do terminal cliente: wsantos
- Diretório de dados: `/var/lib/pgsql/17/data/`

### 2. Objetivo da atividade

Investigar os processos relacionados ao PostgreSQL no sistema operacional Rocky Linux, identificando o processo principal, os processos auxiliares e os processos responsáveis pelas conexões com o banco de dados.

Também foram utilizados comandos de administração do Linux para investigar PID, PPID, usuário responsável pela execução, consumo de CPU, memória e hierarquia dos processos.

Outro objetivo foi comparar o comportamento dos processos antes, durante e depois de estabelecer uma conexão com o PostgreSQL.

### 3. Primeira investigação — Identificando o processo principal

Foi utilizado o comando:

```bash
ps -ef | grep '[p]ostgres'
```

O comando permitiu visualizar os processos relacionados ao PostgreSQL, apresentando informações como usuário, PID, PPID e comando executado.

Resultado encontrado:

| Usuário | PID | PPID | Processo |
|---|---:|---:|---|
| postgres | 999 | 1 | Processo principal |
| postgres | 1068 | 999 | Logger |
| postgres | 1090 | 999 | Checkpointer |
| postgres | 1091 | 999 | Background writer |
| postgres | 2091 | 999 | WAL writer |
| postgres | 2092 | 999 | Autovacuum launcher |
| postgres | 2093 | 999 | Logical replication launcher |

Foi identificado que o processo principal do PostgreSQL possui PID 999 e PPID 1.

Também foi observado que os processos auxiliares apresentam PPID 999, demonstrando que foram iniciados pelo processo principal.

### 4. Segunda investigação — Identificando usuário, PID e PPID

Foi utilizado o comando:

```bash
ps -o user,pid,ppid,stat,%cpu,%mem,comm,args -C postgres
```

Esse comando permitiu selecionar as informações que seriam apresentadas na consulta.

Foram investigados os seguintes campos:

- USER: usuário do sistema operacional responsável pelo processo.
- PID: identificação individual do processo.
- PPID: identificação do processo pai.
- STAT: estado atual do processo.
- %CPU: percentual de utilização da CPU.
- %MEM: percentual de utilização da memória física.
- COMM: nome do comando.
- ARGS: argumentos utilizados na execução.

Foi confirmado que os processos do PostgreSQL estavam sendo executados pelo usuário Linux `postgres`.

Também foi observado que os processos auxiliares apresentavam PPID 999.

### 5. Terceira investigação — Visualizando a árvore de processos

Foi executado o comando:

```bash
pstree -p postgres
```

Resultado encontrado:

```text
postgres(999)─┬─postgres(1068)
              ├─postgres(1090)
              ├─postgres(1091)
              ├─postgres(2091)
              ├─postgres(2092)
              └─postgres(2093)
```

A ferramenta `pstree` permitiu visualizar graficamente a hierarquia dos processos.

Foi possível identificar o processo principal e seus seis processos auxiliares.

Com essa investigação, compreendi que o PostgreSQL utiliza uma arquitetura baseada em múltiplos processos, cada um responsável por determinadas atividades do servidor.

### 6. Quarta investigação — Consumo de CPU e memória

Foi utilizado o comando:

```bash
top
```

O comando apresentou informações dinâmicas sobre o funcionamento geral do sistema operacional.

Resultados observados:

| Indicador | Resultado |
|---|---:|
| Memória total | 1.707,3 MiB |
| Memória livre | 194,2 MiB |
| Memória utilizada | 1.478,5 MiB |
| Memória em cache/buffers | 201,1 MiB |
| Swap total | 2.048,0 MiB |
| Swap livre | 972,3 MiB |
| Swap utilizada | 1.075,7 MiB |
| CPU ociosa | 94,7% |

Também foram observadas colunas como PID, USER, VIRT, RES, SHR, %CPU e %MEM.

Foi compreendido que:

- VIRT representa a memória virtual.
- RES representa a memória física residente.
- SHR representa a memória compartilhável.
- %CPU representa a utilização da CPU.
- %MEM representa a porcentagem de memória física utilizada pelo processo.

A consulta apresentada mostrou o monitoramento geral do sistema. Não foi possível registrar, nessa captura, valores individuais dos processos PostgreSQL, pois eles não apareceram entre as linhas exibidas.

### 7. Quinta investigação — Comparando antes, durante e depois da conexão

Esta etapa teve como objetivo observar o comportamento dos processos PostgreSQL durante uma conexão real com o banco de dados.

#### 7.1 Momento A — Antes da conexão

Foi executado:

```bash
ps -o user,pid,ppid,%cpu,%mem,stat,args -C postgres
```

Foram identificados o processo principal e os processos auxiliares.

O processo principal continuava com PID 999, enquanto os processos auxiliares permaneciam vinculados a ele.

Não havia processos backend das novas sessões que seriam abertas no experimento.

#### 7.2 Momento B — Durante a conexão

Foi aberta uma segunda aba do terminal e executado:

```bash
psql -h localhost -U postgres -d appdb
```

Foi realizada a autenticação com a senha do usuário PostgreSQL.

Após entrar no banco, foi executada a consulta:

```sql
SELECT pg_backend_pid();
```

Resultado encontrado:

```text
 pg_backend_pid
----------------
          20424
(1 linha)
```

O resultado identificou o PID 20424 como o processo backend responsável pela sessão.

Em seguida, no primeiro terminal, foi executado novamente:

```bash
ps -ef | grep '[p]ostgres'
```

Foi encontrada a seguinte linha:

```text
postgres 20424 999 0 15:46 ? 00:00:00 postgres: postgres appdb ::1(36064) idle
```

Interpretação:

- PID: 20424.
- PPID: 999.
- Usuário Linux: postgres.
- Banco de dados: appdb.
- Endereço de conexão: ::1, correspondente ao endereço IPv6 de loopback.
- Porta de origem: 36064.
- Estado: idle.

O estado `idle` indica que a conexão estava estabelecida, mas aguardando a execução de novos comandos.

Também foram observados os seguintes processos:

| Usuário | PID | PPID | Processo |
|---|---:|---:|---|
| wsantos | 20422 | 12871 | Cliente psql |
| postgres | 20424 | 999 | Backend PostgreSQL |
| wsantos | 20240 | 12871 | Outro cliente psql |
| postgres | 20242 | 999 | Outro backend PostgreSQL |

Essa observação permitiu identificar duas sessões de conexão simultâneas.

Foi possível compreender a diferença entre o programa cliente `psql`, executado pelo usuário Linux `wsantos`, e os processos backend do servidor, executados pelo usuário Linux `postgres`.

#### 7.3 Momento C — Depois de fechar a conexão

Após encerrar as sessões do PostgreSQL, foi executado novamente:

```bash
ps -ef | grep '[p]ostgres'
```

Os processos de conexão identificados anteriormente, incluindo os PIDs 20240, 20242, 20422 e 20424, não apareceram mais na listagem.

Entretanto, o processo principal e os processos auxiliares continuaram em execução.

Isso demonstrou que encerrar uma conexão não significa desligar o servidor PostgreSQL.

### 8. Comparação dos resultados

| Momento | Resultado observado |
|---|---|
| Antes da conexão | Processo principal e processos auxiliares ativos. |
| Durante a conexão | Aparecimento dos processos cliente e backend das sessões. |
| Depois de fechar a conexão | Desaparecimento dos processos das sessões encerradas, mantendo o servidor ativo. |

O experimento demonstrou que o PostgreSQL utiliza processos backend independentes para atender às conexões dos clientes.

### 9. Conhecimentos adquiridos

Durante a atividade, aprendi a utilizar ferramentas do Rocky Linux para investigar processos relacionados ao PostgreSQL.

Compreendi:

- A diferença entre PID e PPID.
- Como identificar o processo principal.
- Como identificar os processos filhos.
- Como reconhecer o usuário Linux responsável pela execução.
- Como visualizar a árvore de processos utilizando `pstree`.
- Como monitorar os recursos do sistema utilizando `top`.
- Como identificar um processo backend de uma conexão PostgreSQL.
- A diferença entre o processo cliente `psql` e o processo servidor backend.
- O significado do estado `idle`.
- O comportamento dos processos antes, durante e depois de uma conexão.

### 10. Dificuldades e aprendizados

Um dos principais aprendizados foi compreender a relação entre o processo principal do PostgreSQL e seus processos auxiliares.

Também foi importante entender que o cliente `psql` e o backend PostgreSQL são processos diferentes, executados por usuários distintos do sistema operacional.

Durante a investigação, foram identificadas duas sessões de conexão simultâneas, permitindo observar que cada sessão possuía seu próprio processo backend.

Outra descoberta importante foi perceber que o encerramento de uma sessão não interrompe o funcionamento do servidor nem dos processos auxiliares.

Na investigação de memória e CPU, consegui consultar os indicadores gerais do sistema, mas ainda preciso aprofundar a observação individual dos processos PostgreSQL.

### 11. Conclusão

A atividade permitiu compreender melhor como o PostgreSQL organiza seus processos dentro do sistema operacional Rocky Linux.

Foi possível identificar o processo principal, reconhecer os processos auxiliares, observar a relação entre PID e PPID e visualizar a hierarquia utilizando a ferramenta `pstree`.

O experimento de conexão foi especialmente importante, pois demonstrou na prática que o PostgreSQL cria processos backend para atender às sessões dos clientes.

Também compreendi que o programa `psql` funciona como cliente, enquanto o backend é responsável por atender à conexão no servidor.

Concluo que conhecer e monitorar os processos do PostgreSQL é fundamental para a administração de bancos de dados, pois permite compreender o funcionamento interno do servidor e acompanhar a utilização dos recursos do sistema operacional.



Excelente! Parabéns! 👏 Você concluiu o questionário com 5 de 5 acertos.

100% de aproveitamento

Isso demonstra que você compreendeu os principais conceitos da atividade:

- WAL: registro antecipado das alterações.
- `pg_wal`: diretório de armazenamento dos arquivos WAL.
- Dirty pages: páginas modificadas na memória.
- `checkpointer`: responsável por coordenar os checkpoints.
- REDO: reaplicação de alterações durante a recuperação.




# Diário de Bordo — Atividade 4

## Entendendo o WAL e os Checkpoints

### 1. Objetivo da atividade

Compreender o funcionamento do WAL (Write-Ahead Logging) e dos checkpoints no PostgreSQL, identificando como esses mecanismos contribuem para a segurança, persistência e recuperação dos dados após falhas.

### 2. Conteúdos estudados

Durante a atividade, estudei os seguintes conceitos:

- **WAL (Write-Ahead Logging):** mecanismo que registra antecipadamente as alterações realizadas no banco de dados, permitindo a recuperação após falhas.
- **Localização do WAL:** os arquivos ficam armazenados, por padrão, no diretório `pg_wal`, dentro do diretório de dados do PostgreSQL.
- **Checkpoint:** ponto de verificação no qual o PostgreSQL garante a gravação das páginas modificadas nos arquivos de dados.
- **Checkpointer:** processo responsável por coordenar a execução dos checkpoints.
- **WAL writer:** processo que auxilia na gravação dos registros WAL em disco.
- **Shared buffers:** área de memória compartilhada utilizada para armazenar temporariamente páginas dos dados.
- **Dirty pages:** páginas modificadas na memória que ainda precisam ser gravadas nos arquivos de dados.
- **REDO:** mecanismo utilizado na recuperação para reaplicar alterações registradas no WAL.

### 3. Localização dos arquivos WAL

Considerando a instalação PostgreSQL 17 utilizada no laboratório Rocky Linux, identifiquei o seguinte caminho:

```
/var/lib/pgsql/17/data/pg_wal
```

Esse diretório armazena os arquivos responsáveis por registrar as alterações realizadas no banco de dados.

### 4. Relação entre os componentes

Compreendi que o PostgreSQL utiliza os shared buffers para manter páginas temporariamente na memória.

Quando uma página é modificada, ela passa a ser considerada uma dirty page. As alterações correspondentes são registradas no WAL, que precisa ser persistido antes da gravação da página modificada nos arquivos de dados.

Posteriormente, durante os checkpoints, o PostgreSQL coordena a gravação das páginas modificadas nos arquivos de dados.

### 5. Representação do fluxo

```
Comando SQL
     |
     v
Shared Buffers
     |
     v
Página modificada
(Dirty Page)
     |
     v
Registro no WAL
     |
     v
Persistência do WAL
     |
     v
Checkpoint
     |
     v
Arquivos de dados
```

### 6. Importância do WAL

O WAL é fundamental para a proteção dos dados, pois permite que o PostgreSQL recupere alterações confirmadas que ainda não haviam sido gravadas nos arquivos de dados quando ocorreu uma falha.

Também compreendi que o WAL não é um backup, mas um mecanismo de registro e recuperação das alterações.

### 7. Conclusão

A atividade permitiu compreender como o PostgreSQL trabalha com memória, registros de alterações e armazenamento em disco.

Aprendi que o banco não depende exclusivamente da gravação imediata das páginas nos arquivos de dados. Ele utiliza o WAL para registrar as alterações antecipadamente e os checkpoints para coordenar a persistência das páginas modificadas.

Dessa forma, o PostgreSQL consegue oferecer maior segurança, desempenho e capacidade de recuperação após falhas.





# Diário de Bordo — Atividade 5
## Conhecendo os arquivos de configuração do PostgreSQL

### 1. Objetivo da atividade

Identificar os principais arquivos de configuração do PostgreSQL, localizar o diretório de dados da instância e compreender a finalidade de cada arquivo, utilizando comandos SQL e ferramentas do Rocky Linux.

### 2. Acesso ao PostgreSQL

Inicialmente, acessei o PostgreSQL utilizando o usuário do sistema operacional `postgres`:

```bash
sudo -u postgres psql
```

O acesso foi realizado com sucesso, sendo apresentada a versão PostgreSQL 17.11.

### 3. Identificação do diretório de dados

Executei o comando:

```sql
SHOW data_directory;
```

Resultado:

```text
/var/lib/pgsql/17/data
```

Com isso, identifiquei o diretório de dados da instância PostgreSQL, conhecido como PGDATA.

### 4. Identificação dos arquivos de configuração

Utilizei os comandos SQL disponibilizados pelo próprio PostgreSQL.

**Arquivo principal de configuração:**

```sql
SHOW config_file;
```

Resultado:

```text
/var/lib/pgsql/17/data/postgresql.conf
```

**Arquivo de autenticação:**

```sql
SHOW hba_file;
```

Resultado:

```text
/var/lib/pgsql/17/data/pg_hba.conf
```

**Arquivo de mapeamento de identidades:**

```sql
SHOW ident_file;
```

Resultado:

```text
/var/lib/pgsql/17/data/pg_ident.conf
```

### 5. Conferência dos arquivos no Rocky Linux

Após sair do PostgreSQL utilizando:

```sql
\q
```

Executei o comando:

```bash
ls -l /var/lib/pgsql/17/data/
```

A listagem confirmou a existência dos três arquivos de configuração no diretório de dados.

Também realizei a verificação individual:

```bash
ls -l /var/lib/pgsql/17/data/postgresql.conf

ls -l /var/lib/pgsql/17/data/pg_hba.conf

ls -l /var/lib/pgsql/17/data/pg_ident.conf
```

Todos os arquivos foram encontrados.

### 6. Análise das permissões

Durante a investigação, observei que os três arquivos apresentaram permissões:

```text
-rw-------
```

Identifiquei que o proprietário e o grupo dos arquivos são `postgres`.

Compreendi que essas permissões permitem leitura e escrita pelo proprietário, restringindo o acesso dos demais usuários.

### 7. Finalidade dos arquivos estudados

**postgresql.conf**

Arquivo principal de configuração do servidor. Define parâmetros como porta de comunicação, memória compartilhada, conexões e checkpoints.

**pg_hba.conf**

Arquivo responsável pelas regras de autenticação e controle de acesso às conexões realizadas no PostgreSQL.

**pg_ident.conf**

Arquivo utilizado para relacionar identidades externas, como usuários do sistema operacional, aos usuários internos do PostgreSQL, quando esse mapeamento é necessário.

### 8. Diferença entre diretório de dados e diretório de configuração

O diretório de dados armazena as informações internas necessárias ao funcionamento do PostgreSQL, incluindo arquivos das tabelas, índices, WAL e configurações.

O diretório de configuração é o local onde ficam armazenados os arquivos responsáveis por definir o comportamento e as regras da instância.

Na instalação investigada, os arquivos principais de configuração estavam dentro do próprio diretório de dados:

```text
/var/lib/pgsql/17/data
```

Compreendi que esses diretórios podem estar separados, dependendo da configuração da instalação.

### 9. Importância de identificar os arquivos utilizados

Aprendi que é importante consultar o próprio PostgreSQL para descobrir quais arquivos estão sendo utilizados pela instância.

Essa prática evita alterações em arquivos incorretos e facilita a administração, manutenção e investigação de problemas no servidor.

Também identifiquei a existência do arquivo `postgresql.auto.conf`, utilizado para armazenar configurações realizadas pelo comando `ALTER SYSTEM`.

### 10. Conclusão

A atividade permitiu compreender como o PostgreSQL organiza seus principais arquivos de configuração e como podemos identificar seus caminhos utilizando comandos SQL.

Aprendi a diferenciar o arquivo principal de configuração, o arquivo de autenticação e o arquivo de mapeamento de identidades.

Também pratiquei a comparação entre as informações retornadas pelo PostgreSQL e a estrutura de diretórios do Rocky Linux.

Dessa forma, desenvolvi uma compreensão inicial sobre a organização e a administração dos arquivos de configuração de uma instância PostgreSQL.



# Diário de Bordo — Atividade 6: Consultando os parâmetros da instância

Data 13/11/2026

## 1. Objetivo

Aprender a consultar os parâmetros de configuração do PostgreSQL, identificar seus valores atuais e compreender como funcionam as possibilidades de alteração, sem modificar nenhuma configuração da instância.

## 2. Acesso ao PostgreSQL

Comando executado:

```bash
sudo -u postgres psql
```

O acesso foi realizado com sucesso, utilizando o usuário `postgres`.

## 3. Consulta de parâmetros específicos

Foram executados os comandos:

```sql
SHOW port;
SHOW listen_addresses;
SHOW max_connections;
```

Resultados:

- `port`: 5432.
- `listen_addresses`: localhost.
- `max_connections`: 100.

Também foi realizada uma consulta utilizando a view `pg_settings`:

```sql
SELECT name, setting
FROM pg_settings
WHERE name = 'port';
```

Resultado: parâmetro `port`, valor `5432`.

## 4. Consulta de todos os parâmetros

Foram utilizados os comandos:

```sql
SHOW ALL;
```

E:

```sql
SELECT name, setting
FROM pg_settings
ORDER BY name;
```

As consultas foram iniciadas, mas a exibição completa foi interrompida durante a execução devido à grande quantidade de resultados.

## 5. Consulta por categoria

### Conexões

```sql
SELECT name, setting, unit
FROM pg_settings
WHERE name IN (
    'listen_addresses',
    'port',
    'max_connections'
);
```

Resultados:
- `listen_addresses`: localhost.
- `max_connections`: 100.
- `port`: 5432.

### Memória

```sql
SELECT name, setting, unit
FROM pg_settings
WHERE name IN (
    'shared_buffers',
    'work_mem'
);
```

Resultados:
- `shared_buffers`: 16384 blocos de 8 kB, equivalentes a 128 MB.
- `work_mem`: 4096 kB, equivalentes a 4 MB.

### Logs

```sql
SELECT name, setting, unit
FROM pg_settings
WHERE name IN (
    'logging_collector',
    'log_min_duration_statement'
);
```

Resultados:
- `logging_collector`: on.
- `log_min_duration_statement`: -1 ms.

O valor `-1` indica que o registro de consultas baseado na duração está desativado.

## 6. Investigação detalhada dos sete parâmetros

Consulta executada:

```sql
SELECT
    name,
    setting,
    unit,
    context,
    source,
    pending_restart
FROM pg_settings
WHERE name IN (
    'listen_addresses',
    'port',
    'max_connections',
    'shared_buffers',
    'work_mem',
    'logging_collector',
    'log_min_duration_statement'
)
ORDER BY name;
```

### Resultados encontrados

| Parâmetro | Valor atual | Contexto | Origem | Reinicialização pendente |
|---|---|---|---|---|
| listen_addresses | localhost | postmaster | default | false |
| logging_collector | on | postmaster | configuration file | false |
| log_min_duration_statement | -1 ms | superuser | default | false |
| max_connections | 100 | postmaster | configuration file | false |
| port | 5432 | postmaster | default | false |
| shared_buffers | 128 MB | postmaster | configuration file | false |
| work_mem | 4 MB | user | default | false |

## 7. Finalidade dos parâmetros e consequências de alterações

**listen_addresses:** define os endereços de rede nos quais o PostgreSQL aceita conexões. Uma alteração pode permitir ou restringir conexões externas. Exige restart e não pode ser alterado por sessão.

**port:** define a porta utilizada pelo PostgreSQL. Uma alteração exige que os clientes utilizem a nova porta. Exige restart e não pode ser alterado por sessão.

**max_connections:** define o limite de conexões simultâneas. Aumentar esse limite pode elevar o consumo de memória; diminuir pode impedir novas conexões. Exige restart e não pode ser alterado por sessão.

**shared_buffers:** define a memória compartilhada utilizada pelo PostgreSQL para armazenar blocos de dados em cache. Uma alteração pode afetar desempenho e consumo de memória. Exige restart e não pode ser alterado por sessão.

**work_mem:** define a memória disponível para determinadas operações de consulta, como ordenações e tabelas de hash. Aumentar pode reduzir o uso de arquivos temporários, mas também elevar o consumo de memória. Pode ser alterado por sessão e não exige restart ou reload para essa alteração.

**logging_collector:** controla a coleta das mensagens de log em arquivos pelo processo coletor. Uma alteração modifica o mecanismo de coleta de logs. Exige restart e não pode ser alterado por sessão.

**log_min_duration_statement:** define o tempo mínimo de execução para registrar uma instrução SQL no log. Alterações podem aumentar ou reduzir a quantidade de registros gerados. Pode ser alterado por sessão por um superusuário, sem necessidade de restart ou reload.

## 8. Aprendizados

Durante a atividade, aprendi:

- Utilizar `SHOW` para consultar um parâmetro específico.
- Utilizar `SHOW ALL` para visualizar os parâmetros disponíveis.
- Consultar a view `pg_settings`.
- Utilizar `SELECT`, `WHERE`, `IN` e `ORDER BY` para filtrar e organizar resultados.
- Interpretar as colunas `name`, `setting`, `unit`, `context`, `source` e `pending_restart`.
- Diferenciar parâmetros que exigem restart daqueles que podem ser alterados em tempo de execução.
- Identificar a origem dos valores de configuração.

Também compreendi que os valores de memória precisam ser interpretados junto com suas unidades de medida.

## 9. Conclusão

A atividade permitiu conhecer melhor a configuração da instância PostgreSQL 17.11 e entender como investigar os parâmetros sem alterar o funcionamento do servidor.

Foi possível identificar os valores atuais dos sete parâmetros solicitados, suas finalidades, suas origens e os contextos de alteração.

Nenhum parâmetro foi modificado durante a atividade.



Perfeito! Agora temos os resultados dos três parâmetros e podemos concluir a investigação da Atividade 7. Você conseguiu identificar uma diferença muito importante: **um parâmetro pode estar comentado no arquivo de configuração e, mesmo assim, possuir um valor ativo no PostgreSQL.**

## **1. Analisando os resultados**

## **Resultado da consulta**

| **Parâmetro**            | **Valor atual** | **Valor padrão** |
| ------------------------ | --------------- | ---------------- |
| `listen_addresses`       | `localhost`     | `localhost`      |
| `port`                   | `5432`          | `5432`           |
| `authentication_timeout` | `60`            | `60`             |

Nenhum reinício pendente

Nos três parâmetros, `pending_restart = false`.

Vamos entender cada um.

**1.&#x20;**`listen_addresses = localhost`

Define em quais endereços IP o PostgreSQL aceita conexões.

- `localhost`: aceita conexões locais, pela própria máquina.
- Se configurado para `*`, pode escutar em todas as interfaces de rede, sujeito às demais regras de acesso.

**2.&#x20;**`port = 5432`

Define a porta de comunicação do PostgreSQL.

- `5432` é a porta padrão.
- O comentário no arquivo indica que mudar esse parâmetro exige reiniciar o serviço.

**3.&#x20;**`authentication_timeout = 60`

Define o tempo máximo permitido para concluir a autenticação de um cliente.

- O valor `60` representa 60 segundos.
- O comentário `1min` no arquivo corresponde exatamente a esse valor.

## **2. O que aprendemos sobre a origem dos valores?**

Observe que os três resultados apresentam:

```
source = default
sourcefile = vazio
sourceline = vazio
```

Isso demonstra que os valores atuais vêm dos padrões internos do PostgreSQL, e não de declarações ativas em arquivos de configuração.

Compare com o resultado anterior de `max_connections`:

```
source = configuration file
sourceline = 65
```

Nesse caso, o valor vem de uma configuração encontrada na linha 65 do `postgresql.conf`.

Essa diferença é fundamental para quem administra bancos de dados: não basta conhecer o valor de um parâmetro; também precisamos saber de onde ele veio.





# Atividade 7 — Entendendo o postgresql.conf

## 1. Objetivo

Compreender a organização do arquivo `postgresql.conf`, identificar parâmetros comentados e ativos, interpretar valores padrão e entender a relação entre `postgresql.conf`, `postgresql.auto.conf` e `pg_settings`.

## 2. Investigação do arquivo postgresql.conf

Acessei o arquivo de configuração utilizando:

```
sudo -u postgres less /var/lib/pgsql/17/data/postgresql.conf
```

O arquivo apresenta parâmetros organizados por seções, comentários explicativos e declarações de configuração.

Identifiquei que:

- O caractere `#` transforma o restante da linha em comentário.
- Os parâmetros seguem a estrutura `nome = valor`.
- Existem valores numéricos, booleanos, textos e valores acompanhados de unidades.
- Parâmetros comentados não representam necessariamente parâmetros sem valor: o PostgreSQL pode utilizar seus valores padrão.

Para localizar parâmetros comentados, executei:

```
grep -nE '^[[:space:]]*#[[:space:]]*[a-zA-Z_][a-zA-Z0-9_]*[[:space:]]*=' /var/lib/pgsql/17/data/postgresql.conf | head -n 30
```

Entre os parâmetros encontrados, selecionei:

- `listen_addresses`, com comentário indicando `localhost`.
- `port`, com comentário indicando `5432`.
- `authentication_timeout`, com comentário indicando `1min`.

## 3. Investigação dos valores no pg_settings

Utilizei consultas SQL para investigar os valores atuais, os valores padrão e a origem das configurações.

### max_connections

- Valor atual: 100.
- Valor padrão: 100.
- Origem: configuration file.
- Arquivo: `/var/lib/pgsql/17/data/postgresql.conf`.
- Linha: 65.

### port

- Valor atual: 5432.
- Valor padrão: 5432.
- Origem: default.

### shared_buffers

- Valor atual: 16384.
- Valor padrão: 16384.
- Origem: configuration file.
- Linha: 129.
- Equivalência: 128 MB.

### listen_addresses

- Valor atual: localhost.
- Valor padrão: localhost.
- Origem: default.

### authentication_timeout

- Valor atual: 60 segundos.
- Valor padrão: 60 segundos.
- Origem: default.

Os três últimos parâmetros consultados (`listen_addresses`, `port` e `authentication_timeout`) apresentaram `pending_restart = false`.

## 4. Investigação do postgresql.auto.conf

Executei:

```
sudo -u postgres cat /var/lib/pgsql/17/data/postgresql.auto.conf
```

O arquivo apresentou apenas os comentários informativos:

```
# Do not edit this file manually!
# It will be overwritten by the ALTER SYSTEM command.
```

Concluí que não havia declarações de parâmetros nesse arquivo durante a investigação.

O `postgresql.auto.conf` é utilizado pelo comando `ALTER SYSTEM` para armazenar configurações realizadas por esse mecanismo.

## 5. Relação entre os arquivos e pg_settings

- `postgresql.conf`: arquivo principal de configuração do PostgreSQL.
- `postgresql.auto.conf`: armazena configurações feitas pelo comando `ALTER SYSTEM` e prevalece sobre declarações conflitantes no arquivo principal.
- `pg_settings`: permite consultar os parâmetros, seus valores efetivos e suas origens.

## 6. Aprendizados

Aprendi que um parâmetro comentado pode continuar possuindo um valor ativo porque o PostgreSQL utiliza valores padrão internos.

Também compreendi a diferença entre:

- `setting`: valor atualmente utilizado.
- `boot_val`: valor padrão de inicialização.
- `reset_val`: valor para o qual o parâmetro retornaria com `RESET` na sessão.
- `source`: origem do valor.
- `sourcefile`: arquivo de origem, quando aplicável.
- `sourceline`: linha do arquivo de origem.
- `pending_restart`: indica se uma alteração aguarda reinicialização.

**Conclusão:** consegui identificar a estrutura do arquivo de configuração, investigar parâmetros comentados, consultar valores padrão e compreender como o PostgreSQL determina os valores efetivamente utilizados.

Não realizei alterações nos arquivos de configuração durante esta atividade.



Preparei o diário de bordo da **Atividade 8**, com a data de hoje (17/11/2026), registrando os comandos executados, os resultados obtidos, as dificuldades encontradas e o aprendizado.

Um detalhe importante: você conseguiu realizar a alteração de `-1` para `1000 ms`, aplicar por reload, verificar que foi aceita e restaurar o valor original. Não foi necessário reiniciar o PostgreSQL.

# Diário de Bordo – Atividade 8: Alterando um parâmetro e aplicando a configuração

**Data:** 17/11/2026\
**Sistema operacional:** Rocky Linux\
**Banco de dados:** PostgreSQL 17.11\
**Nível:** Intermediário

## 1. Objetivo da atividade

Realizar uma alteração controlada em um parâmetro de configuração do PostgreSQL, aplicar a mudança por meio de reload, verificar se a configuração foi aceita e, posteriormente, restaurar o valor original sem reiniciar o serviço.

## 2. Primeira investigação: consultando o valor atual

Inicialmente, entrei no PostgreSQL utilizando:

```
sudo -u postgres psql
```

Tentei consultar o parâmetro, mas digitei incorretamente seu nome, utilizando o sinal `+`:

```
SHOW log_min+duration_statement;
```

O PostgreSQL retornou um erro de sintaxe. Corrigi o comando e executei:

```
SHOW log_min_duration_statement;
```

Resultado:

```
log_min_duration_statement
----------------------------
-1
```

Em seguida, consultei as informações detalhadas do parâmetro:

```
SELECT
    name,
    setting,
    unit,
    context,
    source,
    sourcefile,
    pending_restart
FROM pg_settings
WHERE name = 'log_min_duration_statement';
```

Resultados identificados:

- Parâmetro: `log_min_duration_statement`;
- Valor original: `-1`;
- Unidade: milissegundos;
- Contexto: `superuser`;
- Origem: `default`;
- Reinicialização pendente: `false`.

O contexto `superuser` indica que a alteração exige privilégios de superusuário. Para verificar a possibilidade de aplicação por reload, também considerei a classificação do parâmetro: ele aceita recarregamento de configuração.

## 3. Identificando o arquivo de configuração

Executei:

```
SHOW config_file;
```

Resultado:

```
/var/lib/pgsql/17/data/postgresql.conf
```

Esse é o arquivo utilizado para armazenar as configurações do PostgreSQL.

## 4. Criando a cópia de segurança

Saí do PostgreSQL e executei:

```
cp /var/lib/pgsql/17/data/postgresql.conf \
/var/lib/pgsql/17/data/postgresql.conf.bkp_atividade8
```

Para confirmar a criação do backup, utilizei:

```
ls -lh /var/lib/pgsql/17/data/postgresql.conf*
```

O resultado confirmou a existência dos dois arquivos:

- `postgresql.conf`: arquivo original;
- `postgresql.conf.bkp_atividade8`: cópia de segurança.

## 5. Alterando o parâmetro

Abri o arquivo utilizando o editor de texto `vi`:

```
vi /var/lib/pgsql/17/data/postgresql.conf
```

Localizei o parâmetro e alterei seu valor original de `-1` para `1000`:

```
log_min_duration_statement = 1000
```

O novo valor significa que consultas com duração igual ou superior a 1.000 milissegundos (1 segundo) passam a ser registradas no log do PostgreSQL.

Durante essa etapa, tive algumas dificuldades com o editor `vi`, incluindo uma sessão suspensa e a presença de um arquivo temporário `.swp`. Realizei a limpeza do arquivo temporário e consegui concluir a edição.

## 6. Aplicando a configuração por reload

Após salvar o arquivo, entrei novamente no PostgreSQL e executei:

```
SELECT pg_reload_conf();
```

Resultado:

```
pg_reload_conf
---------------
t
```

O resultado `t` representa verdadeiro (`true`) e indica que a solicitação de recarregamento foi realizada.

Em seguida, consultei o parâmetro:

```
SHOW log_min_duration_statement;
```

Resultado:

```
1s
```

O PostgreSQL apresentou o valor em formato de tempo, equivalente a 1.000 milissegundos.

Para confirmar o valor numérico e sua origem, executei:

```
SELECT
    name,
    setting,
    unit,
    source,
    sourcefile,
    pending_restart
FROM pg_settings
WHERE name = 'log_min_duration_statement';
```

Resultados:

- Valor: `1000`;
- Unidade: `ms`;
- Origem: `configuration file`;
- Arquivo: `postgresql.conf`;
- Reinicialização pendente: `false`.

## 7. Investigando possíveis erros de configuração

Para verificar se o PostgreSQL identificou algum problema na leitura do arquivo, consultei a visão `pg_file_settings`:

```
SELECT
    name,
    setting,
    applied,
    error,
    sourcefile,
    sourceline
FROM pg_file_settings
WHERE name = 'log_min_duration_statement';
```

Resultados:

- Parâmetro encontrado: `log_min_duration_statement`;
- Valor configurado: `1000`;
- Aplicado: `true`;
- Erro: nenhum;
- Arquivo: `postgresql.conf`;
- Linha: 547.

Essa consulta confirmou que a configuração foi encontrada e aceita, sem apresentar erros.

## 8. Restaurando o valor original

Após confirmar o funcionamento da alteração, abri novamente o arquivo:

```
vi /var/lib/pgsql/17/data/postgresql.conf
```

Restaurei o valor original:

```
log_min_duration_statement = -1
```

Salvei o arquivo e solicitei novamente o recarregamento:

```
SELECT pg_reload_conf();
```

Resultado:

```
pg_reload_conf
---------------
t
```

Em seguida, confirmei a restauração:

```
SHOW log_min_duration_statement;
```

Resultado:

```
-1
```

Por fim, consultei novamente as informações do parâmetro:

```
SELECT
    name,
    setting,
    source,
    pending_restart
FROM pg_settings
WHERE name = 'log_min_duration_statement';
```

Resultado:

- Valor restaurado: `-1`;
- Origem: `configuration file`;
- Reinicialização pendente: `false`.

## 9. Aprendizados da atividade

Durante a atividade, aprendi:

- Consultar os parâmetros de configuração utilizando `SHOW` e `pg_settings`.
- Identificar o arquivo de configuração com `SHOW config_file`.
- Criar uma cópia de segurança utilizando `cp`.
- Editar configurações utilizando o editor `vi`.
- Aplicar alterações utilizando `pg_reload_conf()`.
- Verificar se uma configuração foi aceita por meio de `pg_settings`.
- Investigar erros utilizando `pg_file_settings`.
- Restaurar uma configuração sem reiniciar o serviço PostgreSQL.

Também compreendi a diferença entre `reload` e `restart`: o reload permite reler as configurações sem interromper o serviço, enquanto o restart reinicia o PostgreSQL.

## 10. Resultado final

**Atividade concluída com sucesso!**

Consegui alterar o parâmetro `log_min_duration_statement` de `-1` para `1000` milissegundos, aplicar a configuração por meio de reload, confirmar sua aceitação e investigar possíveis erros.

Posteriormente, restaurei o valor original para `-1` e confirmei que a configuração voltou a ser utilizada.

Todo o procedimento foi realizado sem necessidade de reiniciar o PostgreSQL.



1. `pg_settings`**:** mostra o que o PostgreSQL está efetivamente utilizando.
2. `pg_file_settings`**:** permite investigar o que está definido nos arquivos e se as configurações foram aplicadas.
3. `pg_reload_conf()`**:** solicita que o PostgreSQL releia suas configurações.



Vamos realizar a **Atividade 9 – Reload ou Restart?** como uma aula prática, seguindo o mesmo método das atividades anteriores: primeiro entender o conceito, depois executar os comandos no PostgreSQL, interpretar os resultados e, por último, montar o diário de bordo.

Você está trabalhando com PostgreSQL 17.11 no Rocky Linux. Portanto, vamos utilizar comandos compatíveis com essa versão.

# **1. Entendendo a diferença entre reload e restart**

Antes de executar qualquer comando, precisamos entender que o PostgreSQL é formado por um processo principal (*postmaster*) e vários processos auxiliares.

Quando alteramos uma configuração, o PostgreSQL precisa saber quando e como aplicar essa mudança.

**RELOAD – Recarregar configurações**

Sem reiniciar o serviço

- O PostgreSQL relê o arquivo `postgresql.conf`.
- As configurações compatíveis com reload são aplicadas.
- O processo principal continua funcionando.
- As conexões existentes normalmente são preservadas.

**RESTART – Reiniciar o servidor**

Exige reinicialização

- O PostgreSQL encerra sua execução e inicia novamente.
- As configurações que dependem da inicialização são aplicadas.
- As conexões existentes são interrompidas.
- O serviço pode ficar indisponível durante a operação.

**Uma analogia:**

Imagine que o PostgreSQL seja uma empresa.

- Reload: atualizar uma orientação interna sem fechar a empresa.
- Restart: fechar e abrir a empresa novamente para que uma mudança estrutural entre em funcionamento.

Essa é a diferença fundamental.

# **2. Entendendo os contextos de configuração**

O campo `context` informa em que condições determinado parâmetro pode ser alterado.

Vamos estudar os cinco contextos solicitados.

| Contexto     | Significado                                                                                       |
| ------------ | ------------------------------------------------------------------------------------------------- |
| `postmaster` | Só é aplicado quando o servidor inicia. Exige restart.                                            |
| `sighup`     | Pode ser aplicado após recarregar as configurações.                                               |
| `backend`    | Pode ser definido para uma sessão no momento da conexão.                                          |
| `superuser`  | Pode ser alterado durante a sessão, mas exige privilégio de superusuário ou permissão específica. |
| `user`       | Pode ser alterado durante a sessão pelo próprio usuário.                                          |

**Atenção a uma diferença importante:**

`backend` não significa necessariamente que precisa reiniciar o servidor.

Significa que o valor pode ser definido quando uma conexão é iniciada, mas não pode ser modificado naquela sessão depois que ela começou. Uma alteração no arquivo de configuração pode ser recarregada e afetar novas conexões.

Já `superuser` e `user` permitem alterações durante a sessão, respeitando as permissões correspondentes.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+1



# **3. Investigando os cinco parâmetros**

Agora vamos entender o que cada parâmetro controla e por que alguns exigem restart.

[Университет Синергия | Факультет программирования](https://images.openai.com/static-rsc-4/ANPFXBAeipA1M8GPQa13wu17ozsGn1uBNsk18X_QarDOaL4kql_ctdES8b3vLB_UOonIZbuAF1sxCMnslIAQ22lE2LNvCqzaNRnt7zlHFyUjzE2WeSZ_7laiaT7nUzuEPc7KMvUFgcsTYIMhbJVC0aGtp89kheRzBkDIrDyPk1E?purpose=inline)

**1.&#x20;**`listen_addresses`

Define em quais endereços IP o PostgreSQL aceita conexões.

Contexto: `postmaster`.

Exige restart

**Por quê?** Os endereços de escuta são definidos durante a inicialização do servidor, quando ele configura os pontos de comunicação de rede.

**2.&#x20;**`port`

Define a porta TCP utilizada pelo PostgreSQL para receber conexões.

Contexto: `postmaster`.

Exige restart

**Por quê?** O servidor precisa abrir a porta durante sua inicialização. Não basta reler o arquivo de configuração.

**3.&#x20;**`max_connections`

Define o número máximo de conexões simultâneas permitidas no servidor.

Contexto: `postmaster`.

Exige restart

**Por quê?** O PostgreSQL precisa dimensionar estruturas internas de memória e recursos para administrar as conexões durante a inicialização.

**4.&#x20;**`shared_buffers`

Define a quantidade de memória compartilhada que o PostgreSQL utiliza para armazenar páginas de dados em cache.

Contexto: `postmaster`.

Exige restart

**Por quê?** Essa memória compartilhada é reservada e configurada quando o servidor inicia. Não pode ser redimensionada simplesmente com reload.

**5.&#x20;**`log_min_duration_statement`

Define o tempo mínimo de execução de uma consulta para que sua duração seja registrada no log.

Contexto: `superuser`.

Não exige restart

**Por quê?** É uma configuração que pode ser modificada por um superusuário durante a sessão, respeitando as permissões. Também pode ser definida no arquivo de configuração e aplicada com reload.

Exemplo: configurando `250ms`, consultas que demorarem 250 milissegundos ou mais terão sua duração registrada.

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+2

# **4. Parte prática – investigando no PostgreSQL**

Agora vamos para o terminal do Rocky Linux.

**Passo 1 – Acessar o PostgreSQL**

No terminal, execute:

```
sudo -u postgres psql
```

Você deverá visualizar algo semelhante a:

```
psql (17.11)
Digite "help" para obter ajuda.

postgres=#
```

**Passo 2 – Investigar os cinco parâmetros**

Copie e execute esta consulta:

```
SELECT
    name,
    setting,
    unit,
    context,
    pending_restart
FROM pg_settings
WHERE name IN (
    'listen_addresses',
    'port',
    'max_connections',
    'shared_buffers',
    'log_min_duration_statement'
)
ORDER BY name;
```

Vamos entender o que cada coluna significa:

| Coluna            | O que informa                                           |
| ----------------- | ------------------------------------------------------- |
| `name`            | Nome do parâmetro.                                      |
| `setting`         | Valor atualmente utilizado.                             |
| `unit`            | Unidade de medida, quando aplicável.                    |
| `context`         | Contexto de configuração.                               |
| `pending_restart` | Informa se existe alteração aguardando reinicialização. |

**O resultado esperado quanto aos contextos será:**

| Parâmetro                    | Contexto esperado |
| ---------------------------- | ----------------- |
| `listen_addresses`           | postmaster        |
| `port`                       | postmaster        |
| `max_connections`            | postmaster        |
| `shared_buffers`             | postmaster        |
| `log_min_duration_statement` | superuser         |

A coluna `pending_restart` provavelmente apresentará `false` para todos, caso você não tenha alterações pendentes.

Isso é normal!

# **5. Entendendo o pending_restart**

Esse é um dos pontos mais importantes da atividade.

O `pending_restart` é um indicador que informa se uma configuração foi modificada no arquivo de configuração, mas ainda não entrou em funcionamento porque exige reinicialização.

Vamos fazer uma investigação específica:

```
SELECT
    name,
    setting,
    context,
    pending_restart
FROM pg_settings
WHERE pending_restart = true;
```

Se aparecer:

```
 name | setting | context | pending_restart
------+---------+---------+-----------------
(0 rows)
```

Significa que não existem alterações pendentes de restart.

**Atenção:** `false` não significa que o parâmetro nunca exige restart. Significa apenas que não existe uma alteração pendente naquele momento.

Outro detalhe: o `pending_restart` não indica se uma sessão precisa ser reconectada para receber uma configuração. Ele trata especificamente das alterações que exigem reinicialização do servidor.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+1



# **6. Comandos para conhecer o reload**

Não precisamos executar nenhuma alteração nesta atividade, mas é importante conhecer como funciona o procedimento.

O PostgreSQL permite solicitar a releitura das configurações usando:

```
SELECT pg_reload_conf();
```

O resultado esperado é:

```
 pg_reload_conf
----------------
 t
(1 row)
```

O `t` significa que o sinal de recarga foi enviado com sucesso. Não significa que todas as configurações foram necessariamente aplicadas: parâmetros que exigem restart continuam pendentes.

Também existe o comando pelo terminal:

```
sudo -u postgres /usr/pgsql-17/bin/pg_ctl reload \
-D /var/lib/pgsql/17/data
```

Não execute esses comandos como parte obrigatória da atividade. Estamos apenas conhecendo as ferramentas.

**E para o restart?**

No Rocky Linux, o serviço normalmente pode ser reiniciado com:

```
sudo systemctl restart postgresql-17
```

Esse comando interrompe e inicia novamente o serviço. Não vamos executá-lo nesta atividade, pois o objetivo é somente investigar.


# Atividade 9 – Reload ou restart? Entendendo a aplicação das configurações
data 18/11/2026
**Nível:** Intermediário\
**Sistema operacional:** Rocky Linux\
**Banco de dados:** PostgreSQL 17.11

## 1. Objetivo

Compreender como o PostgreSQL aplica alterações em seus parâmetros de configuração, diferenciando os procedimentos de reload e restart.

Também investigar os contextos de configuração e identificar possíveis alterações pendentes de reinicialização por meio da view `pg_settings`.

## 2. Diferença entre reload e restart

- **Reload:** solicita ao PostgreSQL que releia o arquivo `postgresql.conf`, aplicando as configurações que permitem esse procedimento, sem interromper o serviço.
- **Restart:** encerra e inicia novamente o servidor PostgreSQL, sendo necessário para aplicar parâmetros classificados como `postmaster`.

## 3. Contextos de configuração pesquisados

- **postmaster:** parâmetro aplicado somente durante a inicialização do servidor. Exige restart.
- **sighup:** permite aplicar alterações por meio de reload.
- **backend:** pode ser definido no início de uma conexão, mas não alterado durante aquela sessão.
- **superuser:** pode ser alterado durante a sessão por superusuários ou usuários com permissão específica.
- **user:** pode ser alterado durante a sessão pelo próprio usuário.

## 4. Investigação dos parâmetros

| Parâmetro                  | Contexto   | Aplicação                                   |
| -------------------------- | ---------- | ------------------------------------------- |
| listen_addresses           | postmaster | Restart                                     |
| port                       | postmaster | Restart                                     |
| max_connections            | postmaster | Restart                                     |
| shared_buffers             | postmaster | Restart                                     |
| log_min_duration_statement | superuser  | Reload ou alteração na sessão com permissão |

### Justificativa das configurações

- `listen_addresses`: define os endereços de rede nos quais o servidor aceita conexões. Precisa ser configurado durante a inicialização.
- `port`: define a porta de comunicação do PostgreSQL, que precisa ser aberta durante a inicialização.
- `max_connections`: determina o limite de conexões simultâneas e influencia a alocação de recursos internos.
- `shared_buffers`: define a memória compartilhada utilizada pelo PostgreSQL para armazenar páginas de dados em cache.
- `log_min_duration_statement`: determina o tempo mínimo de execução de uma consulta para que sua duração seja registrada nos logs. Pode ser alterado sem reiniciar o servidor.

## 5. Investigação do pending_restart

A view `pg_settings` disponibiliza a coluna `pending_restart`, que indica se uma alteração realizada no arquivo de configuração depende de uma reinicialização do servidor.

O valor `true` indica que existe uma alteração aguardando restart. O valor `false` indica que não existe uma alteração pendente de reinicialização.

Consulta utilizada:

```
SELECT
    name,
    setting,
    context,
    pending_restart
FROM pg_settings
WHERE pending_restart = true;
```

## 6. Conclusão

Nesta atividade, compreendi que nem todas as alterações realizadas nas configurações do PostgreSQL são aplicadas da mesma maneira.

Aprendi que os parâmetros classificados como `postmaster` exigem a reinicialização do servidor, enquanto parâmetros como `sighup` permitem a aplicação por reload.

Também compreendi que os contextos `backend`, `superuser` e `user` possuem comportamentos relacionados ao momento da conexão e às permissões necessárias para modificar configurações.

Por fim, conheci a importância da coluna `pending_restart`, que permite identificar se existem alterações aguardando a reinicialização do servidor.

**Importante:** nenhum parâmetro que exige restart foi alterado durante esta atividade. O objetivo foi exclusivamente investigar e compreender o comportamento das configurações.



# Diário de Bordo – Atividade 10
## Conhecendo os parâmetros de conexão do PostgreSQL

**Ambiente:** Rocky Linux  
**Banco de dados:** PostgreSQL 17.11  
**Usuário utilizado:** postgres

### 1. Objetivo

Investigar os principais parâmetros de conexão do PostgreSQL, compreender suas finalidades, identificar seus valores atuais e analisar como o servidor controla e monitora as conexões simultâneas.

### 2. Acesso ao PostgreSQL

Para iniciar a atividade, acessei o PostgreSQL utilizando o usuário do sistema operacional `postgres`:

```bash
sudo -u postgres psql
```

### 3. Investigação dos parâmetros de conexão

Utilizei a view `pg_settings` para consultar os parâmetros relacionados às conexões.

Comando executado:

```sql
SELECT
    name,
    setting,
    unit,
    context,
    pending_restart
FROM pg_settings
WHERE name IN (
    'listen_addresses',
    'port',
    'max_connections',
    'superuser_reserved_connections'
)
ORDER BY name;
```

**Resultados encontrados:**

| Parâmetro | Valor atual | Contexto |
|---|---|---|
| listen_addresses | localhost | postmaster |
| max_connections | 100 | postmaster |
| port | 5432 | postmaster |
| superuser_reserved_connections | 3 | postmaster |

Todos apresentaram `pending_restart = false`.

#### Finalidade dos parâmetros

- `listen_addresses`: define os endereços de rede nos quais o PostgreSQL aceita conexões. O valor encontrado foi `localhost`, indicando que o servidor está configurado para receber conexões locais.

- `port`: define a porta TCP utilizada pelo PostgreSQL. O valor encontrado foi `5432`, que corresponde à porta padrão.

- `max_connections`: estabelece o número máximo de conexões simultâneas permitidas. O valor encontrado foi `100`.

- `superuser_reserved_connections`: reserva conexões para usuários com privilégios de superusuário. O valor encontrado foi `3`.

Os quatro parâmetros possuem contexto `postmaster`, portanto suas alterações exigem reinicialização do servidor para entrar em vigor.

Também foi estudado que valores muito baixos podem impedir conexões necessárias, enquanto valores muito altos podem aumentar o consumo de memória e recursos do servidor.

### 4. Investigação das conexões atuais

Utilizei a view `pg_stat_activity` para consultar a quantidade de registros:

```sql
SELECT
    COUNT(*) AS conexoes_atuais
FROM pg_stat_activity;
```

**Resultado encontrado:** 6 registros.

Inicialmente, considerei que todos representavam conexões de clientes. Entretanto, após investigar a coluna `backend_type`, identifiquei que cinco registros correspondiam a processos internos do PostgreSQL.

### 5. Identificação das conexões por database

Comando executado:

```sql
SELECT
    datname AS banco_de_dados,
    COUNT(*) AS quantidade_conexoes
FROM pg_stat_activity
GROUP BY datname
ORDER BY quantidade_conexoes DESC;
```

Resultado:

- 5 registros sem database associado;
- 1 registro associado ao database `postgres`.

### 6. Identificação das conexões por usuário

Comando executado:

```sql
SELECT
    usename AS usuario,
    COUNT(*) AS quantidade_conexoes
FROM pg_stat_activity
GROUP BY usename
ORDER BY quantidade_conexoes DESC;
```

Resultado:

- 4 registros sem usuário associado;
- 2 registros associados ao usuário `postgres`.

### 7. Investigação dos estados das conexões

Comando executado:

```sql
SELECT
    state,
    COUNT(*) AS quantidade
FROM pg_stat_activity
GROUP BY state
ORDER BY quantidade DESC;
```

Resultado:

- 5 registros sem estado associado;
- 1 conexão com estado `active`.

Aprendi que `active` representa uma conexão executando uma consulta, enquanto `idle` representa uma conexão estabelecida, mas aguardando uma nova solicitação.

Também estudei o estado `idle in transaction`, que representa uma conexão dentro de uma transação aberta, sem executar uma consulta naquele momento.

### 8. Identificação dos tipos de processos

Para compreender os registros sem usuário e database, utilizei:

```sql
SELECT
    pid,
    backend_type,
    usename,
    datname,
    state
FROM pg_stat_activity
ORDER BY pid;
```

Foram identificados os seguintes processos:

| PID | Processo |
|---|---|
| 1052 | checkpointer |
| 1053 | background writer |
| 1495 | walwriter |
| 1496 | autovacuum launcher |
| 1497 | logical replication launcher |
| 50715 | client backend |

Com essa investigação, compreendi que `pg_stat_activity` apresenta tanto processos internos quanto conexões de clientes.

### 9. Contagem exclusiva das conexões de clientes

Para contar somente as conexões de clientes, utilizei:

```sql
SELECT
    COUNT(*) AS conexoes_de_clientes
FROM pg_stat_activity
WHERE backend_type = 'client backend';
```

**Resultado encontrado:** 1 conexão de cliente.

Também agrupei as conexões por usuário, database e estado:

```sql
SELECT
    usename AS usuario,
    datname AS banco,
    state AS estado,
    COUNT(*) AS quantidade
FROM pg_stat_activity
WHERE backend_type = 'client backend'
GROUP BY usename, datname, state
ORDER BY quantidade DESC;
```

Resultado:

- Usuário: `postgres`;
- Database: `postgres`;
- Estado: `active`;
- Quantidade: 1.

### 10. Comportamento do limite de conexões

Foi realizada uma consulta para comparar o limite configurado com a quantidade de registros encontrados:

```sql
SELECT
    (SELECT setting::integer
     FROM pg_settings
     WHERE name = 'max_connections') AS limite_maximo,

    COUNT(*) AS conexoes_atuais,

    (SELECT setting::integer
     FROM pg_settings
     WHERE name = 'superuser_reserved_connections') AS conexoes_reservadas

FROM pg_stat_activity;
```

Resultado:

| Informação | Valor |
|---|---:|
| Limite máximo | 100 |
| Registros na pg_stat_activity | 6 |
| Conexões reservadas | 3 |
| Conexões de clientes | 1 |

Aprendi que, quando o limite de conexões comuns é atingido, novas conexões podem ser recusadas. As conexões reservadas permitem que superusuários ainda tenham acesso administrativo ao servidor.

Também compreendi que aumentar `max_connections` nem sempre resolve problemas, pois pode elevar o consumo de recursos.

### 11. Conclusão

Com esta atividade, aprendi a consultar e interpretar os principais parâmetros de conexão do PostgreSQL.

Também compreendi a importância da view `pg_stat_activity` para monitorar as conexões, identificar usuários, bancos de dados, estados e processos internos.

O principal aprendizado foi entender que nem todos os registros apresentados em `pg_stat_activity` representam conexões de clientes. Para identificar corretamente essas conexões, é necessário utilizar o campo `backend_type` e filtrar por `client backend`.

Concluí que o monitoramento adequado das conexões é fundamental para administrar um banco de dados, identificar possíveis problemas de utilização e evitar que o limite de conexões seja atingido.

**Observação:** Nenhum parâmetro foi alterado durante a atividade.



# Diário de Bordo – Atividade 11: Conhecendo os parâmetros de log

**Data:** 19/11/2026  
**Sistema operacional:** Rocky Linux  
**Banco de dados:** PostgreSQL 17.11

## 1. Objetivo

Investigar os principais parâmetros de configuração de logs do PostgreSQL, identificar seus valores atuais, compreender os requisitos de reload e restart e descobrir onde os registros são armazenados no Rocky Linux.

## 2. Parâmetros investigados

Foi utilizada a visão `pg_settings` para consultar os sete parâmetros solicitados.

| Parâmetro | Valor encontrado | Contexto |
|---|---|---|
| log_connections | off | superuser-backend |
| log_destination | stderr | sighup |
| log_directory | log | sighup |
| log_disconnections | off | superuser-backend |
| log_filename | postgresql-%a.log | sighup |
| logging_collector | on | postmaster |
| log_min_duration_statement | -1 | superuser |

Todos apresentaram `pending_restart = false`.

Foi identificado que `log_destination`, `log_directory` e `log_filename` podem ser recarregados com reload. `logging_collector` exige restart. Os parâmetros `log_connections` e `log_disconnections` possuem contexto superuser-backend, enquanto `log_min_duration_statement` pode ser alterado em tempo de execução por superusuário.

## 3. Identificação dos diretórios

O diretório de dados identificado foi:

`/var/lib/pgsql/17/data`

Arquivo de configuração:

`/var/lib/pgsql/17/data/postgresql.conf`

Diretório de armazenamento dos logs:

`/var/lib/pgsql/17/data/log`

O comando `pg_current_logfile()` retornou:

`log/postgresql-Wed.log`

O arquivo `current_logfiles` confirmou o destino `stderr` e o caminho relativo do arquivo atual.

## 4. Investigação dos mecanismos de gerenciamento de logs

Foi verificado que o serviço `postgresql-17` estava ativo no Rocky Linux.

A consulta ao `journalctl` mostrou mensagens relacionadas à inicialização do PostgreSQL, incluindo a confirmação de que a saída futura seria direcionada ao coletor de logs próprio do banco.

Também foi identificado que o serviço `rsyslog` estava instalado e ativo, utilizando o componente `imjournal`.

A investigação confirmou o uso de arquivos próprios do PostgreSQL, além da presença de registros do serviço no journald. Não foi identificada evidência de encaminhamento direto dos logs do PostgreSQL ao rsyslog.

## 5. Consulta dos arquivos de log

Foi utilizado o comando:

`sudo tail -n 30 /var/lib/pgsql/17/data/log/postgresql-Fri.log`

A consulta apresentou registros reais do funcionamento do PostgreSQL.

Foram identificados:

- Inicialização do PostgreSQL 17.11.
- Escuta nos endereços IPv4 e IPv6 locais.
- Utilização da porta 5432.
- Recuperação automática após uma interrupção anterior não normal.
- Confirmação de que o banco estava pronto para aceitar conexões.
- Registro de um erro de sintaxe ocorrido durante uma atividade anterior.

O erro registrado demonstrou que o PostgreSQL consegue armazenar a mensagem de erro e o comando associado à ocorrência, permitindo investigar problemas posteriormente.

## 6. Conclusão

A atividade permitiu compreender a finalidade dos parâmetros de log do PostgreSQL e identificar como as configurações influenciam o armazenamento e a consulta dos registros.

Foi confirmado que o ambiente utiliza o coletor próprio de logs do PostgreSQL, com armazenamento em arquivos no diretório `/var/lib/pgsql/17/data/log`.

Também foi possível consultar os registros por meio do `journalctl` e compreender a diferença entre os logs gerenciados pelo PostgreSQL e os registros do sistema operacional.

Concluí que os logs são ferramentas fundamentais para administração de bancos de dados, pois permitem acompanhar inicializações, conexões, erros, consultas e procedimentos de recuperação.

**Atividade concluída sem necessidade de modificar parâmetros ou reiniciar o serviço.**



# Entregável final


**1. O que é uma instância PostgreSQL?**

É o conjunto de processos, memória compartilhada e arquivos de armazenamento responsáveis pelo funcionamento do servidor PostgreSQL.

**2. Qual é a responsabilidade do processo principal?**

Iniciar, gerenciar e coordenar os processos do PostgreSQL, além de administrar as solicitações de conexão.

**3. O que acontece quando um cliente estabelece uma conexão?**

O servidor recebe a solicitação, verifica as regras de autenticação e cria um processo backend para atender à conexão.

**4. O PostgreSQL utiliza um único processo para atender todas as conexões?**

Não. Ele utiliza processos separados, chamados backends, para atender as conexões dos clientes.

**5. Qual é a diferença entre processo principal e processo backend?**

O processo principal gerencia a instância. O backend executa as consultas de uma conexão específica.

**6. Para que serve o checkpointer?**

Coordena os checkpoints, garantindo que as páginas modificadas sejam gravadas nos arquivos de dados.

**7. Qual é a diferença entre checkpointer e background writer?**

O checkpointer coordena os checkpoints. O background writer grava páginas modificadas em segundo plano para reduzir a quantidade de trabalho de gravação em momentos de maior demanda.

**8. Qual é a responsabilidade do walwriter?**

Gravar registros WAL armazenados em memória no armazenamento persistente.

**9. Para que serve o autovacuum launcher?**

Coordenar os processos de manutenção automática das tabelas, incluindo VACUUM e ANALYZE.

**10. Todos os processos estão necessariamente ativos o tempo todo?**

Não. Alguns processos executam tarefas periodicamente ou são iniciados conforme a necessidade.

**11. Como identificar os processos no Rocky Linux?**

Utilizando:

```
ps -ef | grep postgres
```

**12. Como identificar o processo pai e os processos filhos?**

Utilizando:

```
ps -ef | grep postgres
pstree -p 999
```

O PID identifica o processo e o PPID identifica o processo pai.

## **WAL e checkpoints**

**13. O que significa Write-Ahead Logging?**

Significa registro antecipado de gravação.

**14. Para que serve o WAL?**

Registrar alterações antes da gravação definitiva nos arquivos de dados, permitindo recuperação após falhas.

**15. O que é um checkpoint?**

É um ponto de sincronização que garante a gravação de páginas modificadas e ajuda a controlar a recuperação.

**16. Qual é a relação entre WAL e arquivos de dados?**

O WAL registra as alterações e os arquivos de dados armazenam as informações efetivas do banco.

**17. O WAL é um backup?**

Não. Ele é um mecanismo de registro e recuperação. Não substitui um backup completo.

**18. Qual é a importância do WAL após uma falha?**

Permite reaplicar alterações necessárias durante a recuperação do banco de dados.

## **Configuração**

**19. Quais são os principais arquivos de configuração?**

`postgresql.conf`, `pg_hba.conf` e `pg_ident.conf`.

**20. Qual é a finalidade do postgresql.conf?**

Configurar parâmetros gerais do servidor, como memória, conexões, porta e logs.

**21. Como descobrir o caminho do arquivo de configuração?**

```
SHOW config_file;
```

**22. O que é postgresql.auto.conf?**

É o arquivo que armazena configurações realizadas pelo comando `ALTER SYSTEM`.

**23. Para que serve pg_settings?**

Consultar parâmetros, valores atuais, origem, contexto e necessidade de reinicialização.

**24. Qual é a diferença entre reload e restart?**

Reload recarrega configurações sem reiniciar o servidor. Restart interrompe e inicia novamente o serviço.

**25. Como identificar se um parâmetro exige restart?**

Consultando o campo `context` da view `pg_settings`. Quando o contexto é `postmaster`, exige reinicialização.

**26. O que significa pending_restart?**

Indica se existe uma alteração de configuração pendente que necessita de restart para ser aplicada.

## **Conexões e logs**

**27. Para que serve listen_addresses?**

Define os endereços de rede nos quais o PostgreSQL aceita conexões.

**28. Para que serve max_connections?**

Define o número máximo de conexões simultâneas permitidas.

**29. Como consultar as conexões existentes?**

```
SELECT * FROM pg_stat_activity;
```

**30. Para que serve pg_stat_activity?**

Permite monitorar sessões, usuários, bancos de dados, consultas e estados das conexões.

**31. Qual é a diferença entre active e idle?**

Active significa que a sessão está executando uma consulta. Idle significa que está conectada, mas aguardando uma nova solicitação.

**32. Onde os logs podem ser armazenados?**

No nosso ambiente, em:

```
/var/lib/pgsql/17/data/log
```

**33. Por que os logs são importantes?**

Porque registram acontecimentos do servidor, ajudam a identificar erros, investigar problemas e acompanhar o funcionamento do banco.





# Resumo dos conhecimentos adquiridos

1. Uma instância PostgreSQL é o conjunto de processos, memória compartilhada e arquivos que fazem o banco funcionar.
2. O processo principal, chamado postmaster, gerencia a instância e coordena os processos.
3. Um backend é um processo responsável por atender uma conexão de cliente.
4. O processo principal coordena o servidor; os auxiliares executam tarefas específicas.
5. Checkpointer coordena checkpoints, background writer grava páginas em segundo plano e walwriter grava registros WAL.
6. O autovacuum launcher coordena a manutenção automática das tabelas.
7. Memória compartilhada é uma área utilizada por diferentes processos para compartilhar informações.
8. O WAL registra alterações antecipadamente para permitir a recuperação do banco.
9. Checkpoint é um ponto de sincronização que ajuda a garantir a gravação das páginas modificadas.
10. WAL registra alterações; arquivos de dados armazenam tabelas e índices.
11. Os principais arquivos são postgresql.conf, pg_hba.conf e pg_ident.conf.
12. O postgresql.conf define os parâmetros gerais de funcionamento do servidor.
13. postgresql.conf configura o servidor; pg_hba.conf controla as regras de autenticação.
14. postgresql.auto.conf armazena configurações realizadas por meio de ALTER SYSTEM.
15. Os caminhos podem ser descobertos utilizando SHOW config_file e SHOW data_directory.
16. pg_settings permite consultar valores, origens e contextos dos parâmetros.
17. Reload recarrega configurações sem reiniciar o servidor; restart reinicia o serviço.
18. listen_addresses define em quais endereços o PostgreSQL aceita conexões.
19. max_connections define o limite de conexões simultâneas.
20. Conexões ativas executam consultas; conexões ociosas estão conectadas, mas aguardando.
21. pg_stat_activity permite consultar sessões, usuários, bancos e estados.
22. Parâmetros de log controlam o destino, armazenamento e conteúdo dos registros.
23. Os logs podem ser encontrados em /var/lib/pgsql/17/data/log.
24. Minha maior dificuldade foi compreender a relação entre os processos, a memória, o WAL e as gravações em disco.
25. Ainda preciso aprofundar meus conhecimentos em recuperação, monitoramento, conexões e administração avançada do PostgreSQL.
