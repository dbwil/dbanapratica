# Quinzena 12 - Arquitetura e configuração básica do PostgreSQL

Tema: Arquitetura interna, processos, arquivos de configuração, parâmetros, conexões e logs

Sistema Operacional: Rocky Linux

Versão de referência: PostgreSQL 18

Dedicação: aproximadamente 30 minutos por dia

---

# Objetivo da quinzena

Nas últimas quinzenas, você aprendeu a trabalhar com:

- instâncias PostgreSQL;
- databases;
- roles e usuários;
- autenticação;
- schemas e tabelas;
- operações básicas de SQL;
- privilégios e controle de acesso;
- organização de acessos utilizando roles de grupo.

Agora vamos avançar no entendimento de como o PostgreSQL funciona internamente e como o administrador controla seu comportamento.

Antes de começar a alterar configurações, precisamos conhecer a arquitetura do PostgreSQL.

Uma instância não é simplesmente um serviço que recebe consultas SQL. Ela é composta por processos que trabalham em conjunto, áreas de memória compartilhada e arquivos armazenados em disco.

Nesta quinzena, vamos estudar:

- arquitetura geral do PostgreSQL;
- processo principal e processos auxiliares;
- processos responsáveis pelas conexões;
- processos relacionados à gravação de dados;
- funcionamento básico do WAL;
- arquivos de configuração;
- parâmetros da instância;
- aplicação de alterações;
- gerenciamento de conexões;
- localização e consulta dos logs.

O objetivo não é aprofundar em otimização ou tuning.

Primeiro precisamos compreender como o PostgreSQL funciona e como investigar seu comportamento.

---

# Como estudar

Continue utilizando aproximadamente 30 minutos por dia.

A proposta é trabalhar em pequenas etapas.

Para cada atividade:

1. Leia o enunciado.
2. Pesquise o assunto.
3. Consulte a documentação oficial.
4. Execute os testes no ambiente de laboratório.
5. Observe os resultados.
6. Registre suas descobertas no diário de bordo.

Não altere parâmetros sem antes entender sua finalidade e seu impacto.

Sempre que for modificar um arquivo de configuração:

- identifique o arquivo correto;
- faça uma cópia de segurança;
- registre o valor original;
- altere somente o parâmetro necessário;
- verifique se a configuração foi aceita;
- confirme o comportamento esperado.

Não faça alterações no ambiente de produção.

---

# Material de apoio

## Documentação oficial do PostgreSQL 18

Documentação geral:

https://www.postgresql.org/docs/18/

Server Processes:

https://www.postgresql.org/docs/18/tutorial-arch.html

Database Physical Storage:

https://www.postgresql.org/docs/18/storage.html

WAL Internals:

https://www.postgresql.org/docs/18/wal-internals.html

Routine Vacuuming:

https://www.postgresql.org/docs/18/routine-vacuuming.html

Server Configuration:

https://www.postgresql.org/docs/18/runtime-config.html

File Locations:

https://www.postgresql.org/docs/18/runtime-config-file-locations.html

Connections and Authentication:

https://www.postgresql.org/docs/18/runtime-config-connection.html

Logging and Reporting:

https://www.postgresql.org/docs/18/runtime-config-logging.html

Viewing Configuration Settings:

https://www.postgresql.org/docs/18/viewing-settings.html

Server Administration:

https://www.postgresql.org/docs/18/admin.html

## Material complementar

Para conteúdos em português, pesquise por:

- arquitetura interna PostgreSQL;
- processos internos PostgreSQL;
- PostgreSQL background processes;
- PostgreSQL WAL;
- PostgreSQL shared buffers;
- PostgreSQL postgresql.conf;
- PostgreSQL parâmetros de configuração;
- PostgreSQL reload restart;
- PostgreSQL max_connections;
- PostgreSQL logs.

Não é necessário assistir a vários vídeos sobre o mesmo assunto. Escolha um material que consiga acompanhar e utilize a documentação oficial para complementar.

---

# Diário de bordo

Continue utilizando o arquivo:

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

Nesta quinzena, dê atenção especial ao registro de processos e alterações de configuração.

Quando investigar um processo, registre:

- nome do processo;
- sua finalidade;
- quando ele é iniciado;
- se permanece ativo ou é executado sob demanda;
- como identificá-lo no sistema operacional.

Quando realizar uma alteração de configuração, registre:

- nome do parâmetro;
- valor original;
- valor alterado;
- arquivo ou mecanismo utilizado;
- necessidade de reload ou restart;
- resultado obtido.

---

# Atividade 1 - Conhecendo a arquitetura do PostgreSQL

Nível: Básico

Dificuldade: Moderada

Tempo estimado: 45 minutos

---

## Enunciado

Até agora você utilizou o PostgreSQL principalmente por meio do `psql` e do serviço do sistema operacional.

Mas o que acontece internamente quando o PostgreSQL é iniciado?

E o que acontece quando um cliente se conecta e executa uma consulta?

Nesta atividade, você deverá pesquisar a arquitetura geral do PostgreSQL.

Investigue os seguintes conceitos:

- instância PostgreSQL;
- processo principal;
- processos auxiliares;
- processos de atendimento às conexões;
- memória compartilhada;
- memória local dos processos;
- arquivos de dados;
- arquivos de WAL.

Procure compreender como esses elementos se relacionam.

Depois responda:

1. O que é uma instância PostgreSQL?
2. Qual é a responsabilidade do processo principal?
3. O que são processos auxiliares?
4. O que significa memória compartilhada?
5. Qual é a diferença entre memória compartilhada e memória local de um processo?
6. Onde ficam armazenados os dados dos databases?
7. Qual é a finalidade do WAL?
8. O PostgreSQL utiliza um único processo para atender todas as conexões?

Faça um desenho simples da arquitetura, identificando os principais componentes e suas relações.

Não é necessário conhecer todos os detalhes internos neste momento.

O objetivo é construir uma visão geral de como o PostgreSQL funciona.

---

## Dica

Pesquise por `PostgreSQL Server Processes`.

Observe especialmente como o PostgreSQL organiza os processos e como uma conexão de cliente é atendida.

Não confunda:

- serviço do sistema operacional;
- instância PostgreSQL;
- database;
- processo de conexão.

São conceitos relacionados, mas diferentes.

---

## Palavras-chave para pesquisa

- PostgreSQL architecture
- PostgreSQL server processes
- PostgreSQL shared memory
- PostgreSQL backend process
- PostgreSQL database cluster
- PostgreSQL WAL

---

## Ao concluir esta atividade você deverá ser capaz de:

- explicar a arquitetura geral do PostgreSQL;
- diferenciar instância, database e serviço;
- explicar a função do processo principal;
- compreender a existência de processos auxiliares;
- explicar, de forma introdutória, a finalidade da memória compartilhada e do WAL.

---

## Checklist

- [ ] Arquitetura geral pesquisada.
- [ ] Processo principal identificado.
- [ ] Processos auxiliares compreendidos.
- [ ] Memória compartilhada pesquisada.
- [ ] Memória local pesquisada.
- [ ] Armazenamento de dados compreendido.
- [ ] Finalidade do WAL compreendida.
- [ ] Desenho da arquitetura elaborado.
- [ ] Diário de bordo atualizado.

---

# Atividade 2 - Conhecendo os principais processos do PostgreSQL

Nível: Básico

Dificuldade: Moderada

Tempo estimado: 45 minutos

---

## Enunciado

Agora vamos aprofundar o conhecimento sobre os processos que compõem uma instância PostgreSQL.

No PostgreSQL 18 existem diferentes processos responsáveis por tarefas específicas.

Pesquise a finalidade dos seguintes processos:

| Processo | Responsabilidade geral |
|---|---|
| `postmaster` | Processo principal que coordena a instância e gerencia conexões. |
| `client backend` | Processo que atende uma sessão de cliente. |
| `checkpointer` | Executa checkpoints. |
| `background writer` | Auxilia na gravação de páginas modificadas em disco. |
| `walwriter` | Grava registros do WAL em arquivos. |
| `autovacuum launcher` | Coordena a execução dos processos de autovacuum. |
| `autovacuum worker` | Executa operações automáticas de VACUUM e ANALYZE. |
| `logger` | Gerencia a escrita de mensagens de log, quando o mecanismo correspondente está habilitado. |
| `archiver` | Arquiva segmentos WAL, quando o arquivamento está habilitado. |
| `startup` | Participa da inicialização e recuperação da instância. |
| `walsender` | Envia WAL para consumidores de replicação. |
| `walreceiver` | Recebe WAL em uma instância que atua como standby. |
| `logical replication launcher` | Coordena processos de replicação lógica. |
| `io worker` | Executa operações de leitura assíncrona de I/O, conforme a configuração e o uso do recurso. |

A lista não significa que todos esses processos estarão necessariamente ativos em uma instalação.

Alguns dependem de funcionalidades habilitadas, como arquivamento e replicação.

Para cada processo, pesquise:

1. Qual é sua responsabilidade?
2. Quando ele é iniciado?
3. Permanece ativo ou é executado sob demanda?
4. Qual sua relação com os demais processos?
5. Como ele pode ser identificado no sistema operacional?

Depois organize os processos em grupos:

- processo principal;
- processos de conexão;
- processos de manutenção;
- processos de gravação;
- processos relacionados a WAL e replicação;
- outros processos auxiliares.

Não é necessário memorizar todos os nomes.

O importante é compreender que cada processo possui uma responsabilidade específica.

---

## Dica

Não confunda `background writer` com `walwriter`.

Eles possuem responsabilidades diferentes.

Também não confunda `checkpointer` com `background writer`.

Os três estão relacionados à gravação de informações, mas não executam a mesma tarefa.

Atenção: versões antigas do PostgreSQL possuíam um processo independente chamado `stats collector`. Ele não deve ser considerado um processo independente na arquitetura do PostgreSQL 18.

---

## Palavras-chave para pesquisa

- PostgreSQL background processes
- PostgreSQL postmaster process
- PostgreSQL checkpointer
- PostgreSQL background writer
- PostgreSQL WAL writer
- PostgreSQL autovacuum launcher
- PostgreSQL client backend
- PostgreSQL 18 processes

---

## Ao concluir esta atividade você deverá ser capaz de:

- identificar os principais processos do PostgreSQL;
- explicar suas responsabilidades;
- diferenciar processos de conexão e processos auxiliares;
- compreender que alguns processos dependem de funcionalidades específicas;
- relacionar os processos às atividades executadas pela instância.

---

## Checklist

- [ ] Processo principal pesquisado.
- [ ] Processo de conexão pesquisado.
- [ ] Processos de gravação pesquisados.
- [ ] Processos de manutenção pesquisados.
- [ ] Processos relacionados ao WAL pesquisados.
- [ ] Processos de replicação pesquisados.
- [ ] Diferenças entre checkpointer, background writer e walwriter compreendidas.
- [ ] Processos organizados por responsabilidade.
- [ ] Diário de bordo atualizado.

---

# Atividade 3 - Observando os processos no Rocky Linux

Nível: Básico

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

Agora que você conhece os principais processos, vamos observar uma instância em funcionamento.

Utilize o Rocky Linux para investigar os processos relacionados ao PostgreSQL.

Identifique:

- processo principal;
- processos de conexão;
- processos auxiliares;
- processos relacionados à manutenção;
- processos relacionados à gravação.

Observe as relações entre os processos.

Investigue também:

- qual é o PID do processo principal;
- quais são os processos filhos;
- como identificar o usuário do sistema operacional responsável pelos processos;
- como identificar o consumo de memória e CPU;
- como visualizar a árvore de processos.

Depois abra uma sessão com o `psql` e observe o que acontece com os processos do PostgreSQL.

Mantenha a sessão aberta e compare a lista de processos antes e depois da conexão.

Feche a sessão e observe novamente.

Registre suas observações.

---

## Dica

Utilize ferramentas de administração do Linux para investigar processos.

Pesquise como visualizar:

- PID;
- PPID;
- usuário;
- consumo de CPU;
- consumo de memória;
- árvore de processos.

O objetivo é descobrir como relacionar os processos observados no Linux com os conceitos estudados na atividade anterior.

---

## Palavras-chave para pesquisa

- Linux ps process tree
- Linux pstree
- Linux process PID PPID
- PostgreSQL process list
- PostgreSQL client backend
- PostgreSQL process monitoring

---

## Ao concluir esta atividade você deverá ser capaz de:

- identificar processos PostgreSQL no Linux;
- localizar o processo principal;
- observar processos de conexão;
- compreender a relação entre processo pai e processos filhos;
- relacionar os processos do sistema operacional à arquitetura do PostgreSQL.

---

## Checklist

- [ ] Processo principal identificado.
- [ ] PID e PPID investigados.
- [ ] Árvore de processos observada.
- [ ] Processos auxiliares identificados.
- [ ] Processo de conexão observado.
- [ ] Diferença antes e depois da conexão registrada.
- [ ] Consumo de CPU e memória investigado.
- [ ] Diário de bordo atualizado.

---

# Atividade 4 - Entendendo o WAL e os checkpoints

Nível: Básico

Dificuldade: Moderada

Tempo estimado: 45 minutos

---

## Enunciado

Agora vamos estudar dois conceitos fundamentais para compreender como o PostgreSQL protege e grava os dados:

- WAL (Write-Ahead Logging);
- checkpoints.

Pesquise o funcionamento básico do WAL.

Procure responder:

1. O que significa Write-Ahead Logging?
2. Qual é a finalidade do WAL?
3. Onde os arquivos WAL ficam armazenados?
4. Por que o PostgreSQL utiliza WAL?
5. Qual é a relação entre WAL e recuperação após uma falha?
6. O que é um checkpoint?
7. Qual é a responsabilidade do processo `checkpointer`?
8. Qual é a diferença entre WAL e arquivos de dados?

Depois investigue a relação entre:

- shared buffers;
- páginas modificadas;
- WAL;
- arquivos de dados;
- checkpoint.

Monte um desenho simples mostrando o caminho conceitual de uma alteração realizada no banco.

Não é necessário realizar operações de recuperação ou modificar arquivos WAL.

O objetivo é compreender por que o PostgreSQL não depende somente da gravação imediata das páginas de dados em disco.

---

## Dica

Pesquise o conceito de Write-Ahead Logging.

O nome já apresenta uma ideia importante: antes de determinadas alterações serem consideradas persistentes, os registros correspondentes precisam ser gravados no WAL.

Não confunda o WAL com um backup do banco de dados.

---

## Palavras-chave para pesquisa

- PostgreSQL WAL
- PostgreSQL write ahead logging
- PostgreSQL checkpoint
- PostgreSQL shared buffers
- PostgreSQL crash recovery
- PostgreSQL WAL writer

---

## Ao concluir esta atividade você deverá ser capaz de:

- explicar a finalidade do WAL;
- explicar o conceito de checkpoint;
- identificar a responsabilidade do checkpointer;
- compreender a relação entre WAL e arquivos de dados;
- explicar, de forma introdutória, como o PostgreSQL utiliza WAL na recuperação após falhas.

---

## Checklist

- [ ] WAL pesquisado.
- [ ] Localização dos arquivos WAL identificada.
- [ ] Checkpoint pesquisado.
- [ ] Responsabilidade do checkpointer compreendida.
- [ ] Relação entre WAL e arquivos de dados compreendida.
- [ ] Relação com shared buffers pesquisada.
- [ ] Desenho do fluxo elaborado.
- [ ] Diário de bordo atualizado.

---

# Atividade 5 - Conhecendo os arquivos de configuração

Nível: Básico

Dificuldade: Baixa

Tempo estimado: 30 minutos

---

## Enunciado

Agora que você conhece a arquitetura e os principais processos, vamos investigar como a instância é configurada.

Pesquise e identifique os principais arquivos utilizados pelo PostgreSQL.

Você deverá localizar:

- `postgresql.conf`;
- `pg_hba.conf`;
- `pg_ident.conf`.

Identifique também:

- o diretório de dados da instância;
- o diretório onde ficam os arquivos de configuração;
- o arquivo de configuração efetivamente utilizado pelo servidor;
- o arquivo de autenticação efetivamente utilizado.

Não assuma que os arquivos estão necessariamente no mesmo diretório.

Utilize o próprio PostgreSQL para descobrir essas informações e compare os resultados com a organização dos diretórios no Rocky Linux.

Depois responda:

1. Para que serve o `postgresql.conf`?
2. Para que serve o `pg_hba.conf`?
3. Para que serve o `pg_ident.conf`?
4. Qual a diferença entre diretório de dados e diretório de configuração?
5. Por que é importante descobrir qual arquivo está sendo utilizado pela instância?

---

## Dica

O PostgreSQL disponibiliza informações sobre seus próprios arquivos de configuração.

Pesquise como consultar essas informações utilizando o `psql`.

Não confunda o arquivo de configuração com o arquivo de autenticação.

---

## Palavras-chave para pesquisa

- PostgreSQL configuration files
- PostgreSQL SHOW config_file
- PostgreSQL SHOW data_directory
- PostgreSQL hba_file
- PostgreSQL ident_file
- Rocky Linux PostgreSQL configuration directory

---

## Ao concluir esta atividade você deverá ser capaz de:

- identificar os principais arquivos de configuração;
- localizar os arquivos utilizados pela instância;
- diferenciar diretório de dados e diretório de configuração;
- explicar a finalidade de cada arquivo.

---

## Checklist

- [ ] `postgresql.conf` localizado.
- [ ] `pg_hba.conf` localizado.
- [ ] `pg_ident.conf` localizado.
- [ ] Diretório de dados identificado.
- [ ] Arquivos efetivamente utilizados identificados.
- [ ] Diferenças entre os arquivos compreendidas.
- [ ] Diário de bordo atualizado.

---

# Atividade 6 - Consultando os parâmetros da instância

Nível: Básico

Dificuldade: Baixa

Tempo estimado: 30 minutos

---

## Enunciado

O PostgreSQL possui muitos parâmetros de configuração.

Antes de alterar qualquer coisa, precisamos aprender a consultar os valores atuais.

Pesquise como visualizar:

- um parâmetro específico;
- todos os parâmetros disponíveis;
- os parâmetros relacionados a conexões;
- os parâmetros relacionados a memória;
- os parâmetros relacionados a logs.

Investigue os seguintes parâmetros:

- `listen_addresses`;
- `port`;
- `max_connections`;
- `shared_buffers`;
- `work_mem`;
- `logging_collector`;
- `log_min_duration_statement`.

Para cada um, registre:

1. Qual é sua finalidade?
2. Qual é o valor atual no seu ambiente?
3. O que pode acontecer se esse valor for alterado?
4. A alteração exige reload ou restart?
5. O parâmetro pode ser alterado por uma sessão individual ou depende de configuração da instância?

Não altere os valores nesta atividade.

O objetivo é conhecer o estado atual e entender a finalidade dos parâmetros.

---

## Dica

O PostgreSQL possui uma view chamada `pg_settings`.

Pesquise como utilizá-la para consultar informações sobre os parâmetros.

Observe que ela fornece mais informações do que somente o valor atual.

---

## Palavras-chave para pesquisa

- PostgreSQL pg_settings
- PostgreSQL SHOW ALL
- PostgreSQL configuration parameters
- PostgreSQL parameter context
- PostgreSQL configuration source
- PostgreSQL pending_restart

---

## Ao concluir esta atividade você deverá ser capaz de:

- consultar parâmetros de configuração;
- identificar valores atuais;
- compreender a finalidade de alguns parâmetros;
- identificar se um parâmetro exige reload ou restart;
- utilizar `pg_settings` para investigar a configuração da instância.

---

## Checklist

- [ ] `pg_settings` pesquisada.
- [ ] Parâmetros consultados.
- [ ] Finalidade dos parâmetros compreendida.
- [ ] Valores atuais registrados.
- [ ] Necessidade de reload ou restart identificada.
- [ ] Diário de bordo atualizado.

---

# Atividade 7 - Entendendo o postgresql.conf

Nível: Básico

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

Agora que você já conhece os arquivos de configuração e sabe consultar os parâmetros, vamos investigar o `postgresql.conf`.

Abra o arquivo utilizado pela sua instância e observe sua organização.

Identifique:

- como os parâmetros são declarados;
- como funcionam os comentários;
- como são representados valores numéricos;
- como são representados valores booleanos;
- como são representadas strings;
- como identificar parâmetros comentados;
- como identificar os valores padrão.

Escolha três parâmetros que estejam comentados no arquivo.

Para cada um, descubra:

1. Qual é seu valor padrão?
2. O que significa deixar o parâmetro comentado?
3. Como o PostgreSQL determina o valor utilizado?
4. Qual seria o efeito de definir explicitamente um valor?

Depois investigue a relação entre:

- `postgresql.conf`;
- `postgresql.auto.conf`;
- `pg_settings`.

Explique como esses elementos se relacionam.

Não altere configurações nesta atividade.

---

## Dica

Nem todo parâmetro precisa aparecer explicitamente no arquivo de configuração.

O PostgreSQL possui valores padrão.

Também existe um mecanismo para registrar configurações por meio do próprio servidor. Pesquise como ele funciona e qual arquivo utiliza.

---

## Palavras-chave para pesquisa

- PostgreSQL postgresql.conf syntax
- PostgreSQL configuration defaults
- PostgreSQL postgresql.auto.conf
- PostgreSQL ALTER SYSTEM
- PostgreSQL configuration precedence

---

## Ao concluir esta atividade você deverá ser capaz de:

- compreender a estrutura do `postgresql.conf`;
- identificar parâmetros comentados;
- diferenciar valor padrão de valor configurado;
- explicar a finalidade do `postgresql.auto.conf`;
- compreender que diferentes fontes podem definir o valor de um parâmetro.

---

## Checklist

- [ ] Estrutura do `postgresql.conf` investigada.
- [ ] Comentários compreendidos.
- [ ] Três parâmetros comentados investigados.
- [ ] Valores padrão identificados.
- [ ] `postgresql.auto.conf` pesquisado.
- [ ] Relação com `pg_settings` compreendida.
- [ ] Diário de bordo atualizado.

---

# Atividade 8 - Alterando um parâmetro e aplicando a configuração

Nível: Intermediário

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

Agora vamos realizar uma alteração controlada.

Escolha um parâmetro que possa ser aplicado por meio de reload, sem necessidade de reiniciar o PostgreSQL.

Antes de realizar a alteração:

1. Consulte o valor atual.
2. Identifique o arquivo de configuração utilizado.
3. Pesquise se o parâmetro exige reload ou restart.
4. Faça uma cópia de segurança do arquivo.
5. Registre o valor original.

Altere o parâmetro escolhido para um valor diferente, mas seguro para o ambiente de laboratório.

Depois aplique a configuração e verifique se o novo valor foi utilizado.

Registre:

- valor original;
- valor novo;
- arquivo alterado;
- procedimento utilizado para aplicar a configuração;
- resultado da consulta após a alteração.

Ao terminar, restaure o valor original e confirme que ele voltou a ser utilizado.

---

## Dica

O PostgreSQL permite recarregar suas configurações sem necessariamente interromper o serviço.

Pesquise a diferença entre:

- reload;
- restart.

Também investigue como identificar se houve erro durante a leitura de um arquivo de configuração.

---

## Palavras-chave para pesquisa

- PostgreSQL reload configuration
- PostgreSQL pg_reload_conf
- PostgreSQL systemctl reload
- PostgreSQL configuration reload
- PostgreSQL configuration error
- PostgreSQL pg_file_settings

---

## Ao concluir esta atividade você deverá ser capaz de:

- alterar um parâmetro de configuração;
- aplicar uma alteração por reload;
- verificar se a alteração foi aceita;
- identificar problemas de configuração;
- restaurar o valor original.

---

## Checklist

- [ ] Parâmetro adequado escolhido.
- [ ] Valor original registrado.
- [ ] Cópia de segurança realizada.
- [ ] Arquivo alterado.
- [ ] Reload executado.
- [ ] Novo valor confirmado.
- [ ] Possíveis erros investigados.
- [ ] Valor original restaurado.
- [ ] Diário de bordo atualizado.

---

# Atividade 9 - Reload ou restart? Entendendo a aplicação das configurações

Nível: Intermediário

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

Nem todas as alterações de configuração podem ser aplicadas da mesma maneira.

Algumas podem ser utilizadas após um reload.

Outras exigem que o processo do PostgreSQL seja reiniciado.

Pesquise a diferença entre os contextos de configuração:

- `postmaster`;
- `sighup`;
- `backend`;
- `superuser`;
- `user`.

Investigue os parâmetros:

- `listen_addresses`;
- `port`;
- `max_connections`;
- `shared_buffers`;
- `log_min_duration_statement`.

Para cada um, identifique:

1. Qual é o contexto de configuração?
2. A alteração exige reload ou restart?
3. Por que essa alteração possui esse requisito?
4. Como verificar se existe uma alteração pendente de restart?

Monte uma tabela no diário de bordo:

| Parâmetro | Contexto | Aplicação |
|---|---|---|
| listen_addresses | | |
| port | | |
| max_connections | | |
| shared_buffers | | |
| log_min_duration_statement | | |

Não altere os parâmetros que exigem restart nesta atividade.

O objetivo é compreender o comportamento antes de executar qualquer mudança.

---

## Dica

O campo `context` da view `pg_settings` é importante para essa investigação.

Observe também o campo `pending_restart`.

Não confunda a necessidade de reiniciar o serviço com a necessidade de reiniciar apenas uma sessão.

---

## Palavras-chave para pesquisa

- PostgreSQL configuration context
- PostgreSQL postmaster parameter
- PostgreSQL sighup parameter
- PostgreSQL pending_restart
- PostgreSQL reload versus restart

---

## Ao concluir esta atividade você deverá ser capaz de:

- diferenciar reload de restart;
- identificar o contexto de um parâmetro;
- compreender quais alterações exigem reinicialização;
- identificar alterações pendentes de restart.

---

## Checklist

- [ ] Contextos de configuração pesquisados.
- [ ] Cinco parâmetros investigados.
- [ ] Tabela preenchida.
- [ ] Diferença entre reload e restart compreendida.
- [ ] `pending_restart` pesquisado.
- [ ] Diário de bordo atualizado.

---

# Atividade 10 - Conhecendo os parâmetros de conexão

Nível: Intermediário

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

Agora vamos estudar alguns parâmetros relacionados às conexões com o PostgreSQL.

Pesquise a finalidade dos seguintes parâmetros:

- `listen_addresses`;
- `port`;
- `max_connections`;
- `superuser_reserved_connections`.

Para cada um, explique:

1. Qual é sua finalidade?
2. Qual é o valor atual?
3. Qual é o impacto de alterá-lo?
4. A alteração exige reload ou restart?
5. Existe algum risco em definir um valor muito alto ou muito baixo?

Depois investigue o comportamento do PostgreSQL quando o limite de conexões é atingido.

Pesquise também:

- como consultar o número de conexões atuais;
- como identificar as conexões por database;
- como identificar as conexões por usuário;
- como verificar quais conexões estão ativas ou ociosas.

Utilize a view `pg_stat_activity` para realizar a investigação.

Não aumente o limite de conexões nesta atividade.

O objetivo é compreender como o PostgreSQL controla as conexões e como o administrador pode acompanhar sua utilização.

---

## Dica

Não confunda o número máximo de conexões configurado com o número de conexões atualmente abertas.

São informações diferentes.

Também não assuma que aumentar `max_connections` é sempre a solução para problemas de conexão.

---

## Palavras-chave para pesquisa

- PostgreSQL connection settings
- PostgreSQL max_connections
- PostgreSQL listen_addresses
- PostgreSQL pg_stat_activity
- PostgreSQL database connections
- PostgreSQL idle connections
- PostgreSQL superuser_reserved_connections

---

## Ao concluir esta atividade você deverá ser capaz de:

- explicar os principais parâmetros de conexão;
- consultar conexões existentes;
- identificar conexões por database e usuário;
- diferenciar conexões ativas e ociosas;
- compreender o comportamento do limite de conexões.

---

## Checklist

- [ ] Parâmetros de conexão pesquisados.
- [ ] Valores atuais registrados.
- [ ] `pg_stat_activity` pesquisada.
- [ ] Conexões atuais consultadas.
- [ ] Conexões por database identificadas.
- [ ] Conexões por usuário identificadas.
- [ ] Estados das conexões investigados.
- [ ] Diário de bordo atualizado.

---

# Atividade 11 - Conhecendo os parâmetros de log

Nível: Intermediário

Dificuldade: Moderada

Tempo estimado: 30 minutos

---

## Enunciado

O PostgreSQL possui mecanismos de registro de eventos e mensagens que ajudam o administrador a investigar problemas.

Nesta atividade, vamos conhecer alguns parâmetros relacionados aos logs.

Pesquise a finalidade de:

- `log_destination`;
- `logging_collector`;
- `log_directory`;
- `log_filename`;
- `log_min_duration_statement`;
- `log_connections`;
- `log_disconnections`.

Para cada parâmetro, identifique:

- sua finalidade;
- o valor atual;
- se exige reload ou restart;
- onde as mensagens podem ser encontradas.

Depois investigue como identificar os logs do PostgreSQL no Rocky Linux.

Verifique se o ambiente utiliza:

- arquivos de log do próprio PostgreSQL;
- journald;
- outro mecanismo de gerenciamento de logs.

Não é necessário modificar os parâmetros nesta atividade.

O objetivo é descobrir como o PostgreSQL registra informações e como o administrador pode consultar esses registros.

---

## Dica

A localização dos logs depende da configuração da instância e da forma como o serviço foi instalado.

Não presuma que todos os ambientes armazenam os logs no mesmo diretório.

---

## Palavras-chave para pesquisa

- PostgreSQL logging configuration
- PostgreSQL log_destination
- PostgreSQL logging_collector
- PostgreSQL log_directory
- PostgreSQL log_min_duration_statement
- Rocky Linux journalctl PostgreSQL
- PostgreSQL server logs

---

## Ao concluir esta atividade você deverá ser capaz de:

- identificar os principais parâmetros de log;
- descobrir onde os logs são armazenados;
- consultar logs do PostgreSQL;
- compreender a finalidade dos registros para investigação de problemas.

---

## Checklist

- [ ] Parâmetros de log pesquisados.
- [ ] Valores atuais registrados.
- [ ] Localização dos logs identificada.
- [ ] Mecanismo de gerenciamento de logs identificado.
- [ ] Logs consultados.
- [ ] Diário de bordo atualizado.

---

# Distribuição sugerida dos 15 dias

A distribuição abaixo é uma referência. Se uma atividade exigir mais pesquisa, avance no seu próprio ritmo, mantendo a dedicação aproximada de 30 minutos por dia.

| Dia | Atividade |
|---|---|
| 1 | Atividade 1 - Arquitetura geral |
| 2 | Atividade 1 - Desenho e registro |
| 3 | Atividade 2 - Principais processos |
| 4 | Atividade 2 - Organização dos processos |
| 5 | Atividade 3 - Observação no Rocky Linux |
| 6 | Atividade 3 - Investigação da árvore de processos |
| 7 | Atividade 4 - WAL e checkpoints |
| 8 | Atividade 4 - Desenho do fluxo |
| 9 | Atividade 5 - Arquivos de configuração |
| 10 | Atividade 6 - Consulta de parâmetros |
| 11 | Atividade 7 - Estrutura do postgresql.conf |
| 12 | Atividade 8 e 9 - Alterações e aplicação |
| 13 | Atividade 10 - Conexões |
| 14 | Atividade 11 - Logs |
| 15 | Revisão, resumo e preparação para avaliação |

---

# Entregável final

Ao final da quinzena, você deverá entregar um documento contendo:

## 1. Arquitetura

- explicação da arquitetura geral;
- desenho dos principais componentes;
- relação entre processos, memória e armazenamento;
- explicação básica do funcionamento de uma conexão.

## 2. Processos

Uma tabela contendo os processos estudados, suas responsabilidades e como foram identificados no Rocky Linux.

## 3. WAL e checkpoints

- explicação do WAL;
- explicação de checkpoint;
- relação entre WAL e arquivos de dados;
- desenho do fluxo de gravação.

## 4. Arquivos de configuração

- localização do `postgresql.conf`;
- localização do `pg_hba.conf`;
- localização do `pg_ident.conf`;
- diretório de dados;
- explicação da finalidade de cada arquivo.

## 5. Parâmetros

Uma tabela contendo os parâmetros estudados, seus valores atuais e suas finalidades.

## 6. Alteração de configuração

O registro de uma alteração realizada com reload, incluindo:

- valor original;
- valor alterado;
- procedimento utilizado;
- resultado;
- restauração do valor original.

## 7. Conexões

Um pequeno relatório contendo:

- número de conexões atuais;
- conexões por database;
- conexões por usuário;
- conexões ativas e ociosas.

## 8. Logs

- localização dos logs;
- mecanismo utilizado para armazená-los;
- exemplos de mensagens encontradas;
- explicação sobre a utilidade desses registros.

---

# Resumo obrigatório

Escreva um texto com no máximo 25 linhas respondendo:

1. O que é uma instância PostgreSQL?
2. Qual é a responsabilidade do processo principal?
3. O que é um processo backend?
4. Qual é a diferença entre processo principal e processo auxiliar?
5. Para que servem o checkpointer, background writer e walwriter?
6. Qual é a responsabilidade do autovacuum launcher?
7. O que é memória compartilhada?
8. Para que serve o WAL?
9. O que é um checkpoint?
10. Qual é a diferença entre WAL e arquivos de dados?
11. Quais são os principais arquivos de configuração?
12. Qual é a finalidade do `postgresql.conf`?
13. Qual é a diferença entre `postgresql.conf` e `pg_hba.conf`?
14. O que é o `postgresql.auto.conf`?
15. Como descobrir qual arquivo de configuração está sendo utilizado?
16. Para que serve a view `pg_settings`?
17. Qual é a diferença entre reload e restart?
18. Para que serve o parâmetro `listen_addresses`?
19. Para que serve o parâmetro `max_connections`?
20. Qual é a diferença entre conexões ativas e ociosas?
21. Para que serve a view `pg_stat_activity`?
22. Para que servem os parâmetros relacionados a logs?
23. Onde os logs do PostgreSQL podem ser encontrados?
24. Qual foi a maior dificuldade encontrada?
25. O que você ainda precisa estudar melhor?

---

# Perguntas para avaliação

Durante nosso próximo encontro, esteja preparado para responder:

## Arquitetura e processos

1. O que é uma instância PostgreSQL?

2. Qual é a responsabilidade do processo principal?

3. O que acontece quando um cliente estabelece uma conexão com o PostgreSQL?

4. O PostgreSQL utiliza um único processo para atender todas as conexões?

5. Qual é a diferença entre processo principal e processo backend?

6. Para que serve o checkpointer?

7. Qual é a diferença entre checkpointer e background writer?

8. Qual é a responsabilidade do walwriter?

9. Para que serve o autovacuum launcher?

10. Todos os processos do PostgreSQL estão necessariamente ativos o tempo todo?

11. Como identificar os processos do PostgreSQL no Rocky Linux?

12. Como identificar o processo pai e os processos filhos?

## WAL e checkpoints

13. O que significa Write-Ahead Logging?

14. Para que serve o WAL?

15. O que é um checkpoint?

16. Qual é a relação entre WAL e arquivos de dados?

17. O WAL é um backup do banco de dados?

18. Qual é a importância do WAL na recuperação após uma falha?

## Configuração

19. Quais são os principais arquivos de configuração do PostgreSQL?

20. Qual é a finalidade do `postgresql.conf`?

21. Como descobrir o caminho do arquivo de configuração utilizado pela instância?

22. O que é o `postgresql.auto.conf`?

23. Para que serve a view `pg_settings`?

24. Qual é a diferença entre reload e restart?

25. Como identificar se um parâmetro exige restart?

26. O que significa `pending_restart`?

## Conexões e logs

27. Para que serve `listen_addresses`?

28. Para que serve `max_connections`?

29. Como consultar as conexões existentes no PostgreSQL?

30. Para que serve `pg_stat_activity`?

31. Qual é a diferença entre uma conexão ativa e uma conexão ociosa?

32. Onde os logs do PostgreSQL podem ser armazenados?

33. Por que os logs são importantes para investigar problemas?

---
