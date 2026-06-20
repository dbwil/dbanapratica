# dbanapratica — Quinzena 6

Período: 07/06 → 21/06

Tema: Shell Script Intermediário

Base: Rocky Linux

---

## Objetivo da quinzena

Ao final desta quinzena você deverá ser capaz de:

* Utilizar variáveis em shell script
* Utilizar condicionais simples
* Utilizar laços de repetição simples
* Coletar informações do sistema
* Gerar relatórios simples
* Automatizar pequenas tarefas administrativas

---

## Atividade 1 — Evoluindo o Script da Quinzena Anterior

Nível: Básico

### Tempo esperado

| Etapa           |  Tempo |
| --------------- | -----: |
| Leitura         | 20 min |
| Vídeos          | 40 min |
| Desenvolvimento |     1h |
| Testes          |     1h |
| **Total**       | **3h** |

### Enunciado

Utilizando o script criado na quinzena anterior, transforme-o em um relatório simples do ambiente.

O relatório deve permitir identificar:

* quem executou o script
* quando ele foi executado
* em qual diretório ele foi executado
* quantos arquivos existem dentro da estrutura SGD

As informações devem ser exibidas na tela e também registradas em um arquivo.

### Direção de estudo

* Variáveis
* Substituição de comandos
* Redirecionamento de saída

### Conteúdo sugerido

#### Leitura

* https://ryanstutorials.net/bash-scripting-tutorial/
* https://ryanstutorials.net/bash-scripting-tutorial/bash-variables.php

#### Vídeos

Pesquisar no YouTube:

* "Bóson Treinamentos shell script variáveis"
* "LinuxTips shell script básico"

### Palavras-chave para pesquisa

* bash variables
* command substitution bash
* bash redirect output

### Checklist

* [X] Relatório gerado
* [X] Informações corretas
* [X] Arquivo criado
* [X] Variáveis utilizadas
* [X] Consegue explicar o funcionamento das variáveis utilizadas

---

## Atividade 2 — Verificador de Diretórios

Nível: Básico

### Tempo esperado

| Etapa           |       Tempo |
| --------------- | ----------: |
| Leitura         |      20 min |
| Vídeos          |      40 min |
| Desenvolvimento |        1h30 |
| Testes          |          1h |
| **Total**       | **3h30min** |

### Enunciado

Crie um script que verifique a existência de diretórios da estrutura SGD.

Caso o diretório exista:

* registrar a informação em um arquivo de log

Caso o diretório não exista:

* informar ao usuário

O script deve verificar ao menos os diretórios:

* dados
* logs
* scripts
* backups

### Direção de estudo

* Condicionais
* Testes em shell script

### Conteúdo sugerido

#### Leitura

* https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php

#### Vídeos

Pesquisar no YouTube:

* "Bóson Treinamentos if shell script"
* "LinuxTips condicionais bash"

### Palavras-chave para pesquisa

* bash if directory exists
* shell script conditionals
* bash test directory

### Checklist

* [X] Diretórios verificados
* [X] Condicional funcionando
* [X] Log gerado
* [X] Consegue explicar a lógica utilizada

---

## Atividade 3 — Auditoria de Arquivos

Nível: Intermediário

### Tempo esperado

| Etapa           |       Tempo |
| --------------- | ----------: |
| Leitura         |      20 min |
| Vídeos          |      40 min |
| Desenvolvimento |        1h30 |
| Testes          |          1h |
| **Total**       | **3h30min** |

### Enunciado

Crie um script que percorra todos os arquivos existentes dentro da estrutura SGD.

Para cada arquivo encontrado, gere um relatório contendo:

* nome
* tamanho
* proprietário

O relatório deve ser salvo em arquivo.

### Direção de estudo

* Laços de repetição
* Processamento de arquivos
* Variáveis

### Conteúdo sugerido

#### Leitura

* https://ryanstutorials.net/bash-scripting-tutorial/bash-loops.php

#### Vídeos

Pesquisar no YouTube:

* "Bóson Treinamentos for shell script"
* "Bóson Treinamentos while shell script"

### Palavras-chave para pesquisa

* bash for loop
* iterate files bash
* file owner linux

### Checklist

* [X] Loop funcionando
* [X] Relatório criado
* [X] Informações corretas
* [X] Consegue explicar a lógica utilizada

---

## Atividade 4 — Revisão de Permissões

Nível: Básico

### Tempo esperado

| Etapa     |  Tempo |
| --------- | -----: |
| Revisão   | 30 min |
| Exercício |     1h |
| Testes    | 30 min |
| **Total** | **2h** |

### Enunciado

Revise o ambiente SGD criado até o momento.

Implemente um cenário onde:

* um usuário possui acesso total ao diretório dados
* outro usuário possui apenas acesso de leitura
* um terceiro usuário não possui acesso

Documente:

* como a solução foi implementada
* quais permissões foram utilizadas
* quais dificuldades foram encontradas

### Direção de estudo

* Ownership
* Permissões
* Usuários

### Conteúdo sugerido

#### Leitura

* https://www.guiafoca.org/guiaonline/intermediario/ch03.html

#### Vídeos

Pesquisar no YouTube:

* "Bóson Treinamentos permissões linux"
* "LinuxTips chmod chown"

### Palavras-chave para pesquisa

* chmod linux
* chown linux
* linux file permissions

### Checklist

* [X] Cenário implementado
* [X] Permissões funcionando
* [X] Documentação criada

---

## Atividade 5 — Consolidação

Nível: Intermediário

### Tempo esperado

| Etapa           |  Tempo |
| --------------- | -----: |
| Revisão         | 30 min |
| Reimplementação |   1h30 |
| **Total**       | **2h** |

### Enunciado

Sem consultar materiais anteriores:

* recrie os scripts desenvolvidos nesta quinzena
* execute todos os scripts
* valide os resultados

O objetivo é verificar se os conceitos foram assimilados.

### Checklist

* [X] Scripts recriados
* [X] Scripts executados
* [X] Resultados corretos
* [X] Consegue explicar o funcionamento de cada script

---

## Entregáveis da quinzena

Você deve apresentar:

* [X] relatorio.sh
* [X] verificador.sh
* [X] auditoria.sh
* [X] documentação do exercício de permissões
* [X] evidências de execução
* [X] resumo da quinzena

### Resumo obrigatório

Escreva um texto curto (máximo 15 linhas) explicando:

* o que são variáveis
* o que são condicionais
* o que são laços de repetição
* qual atividade foi mais difícil
* quais dúvidas ainda permanecem


***ATIVIDADE 1***

1 CRIANDO SCRIPT 1° PARTE
<img width="1280" height="772" alt="image" src="https://github.com/user-attachments/assets/241d5dcc-46ab-43ed-86a7-fe1fbcf9e49d" />

2 APÓS A CRIÇÃO DO SCRIPT, A EXECUÇÃO!
<img width="1280" height="792" alt="image" src="https://github.com/user-attachments/assets/96e43127-9661-4cd7-8029-7005f19874a3" />

3 CRIANDO O ARQUIVO QUE SALVA O RELATÓRIO
<img width="1280" height="786" alt="image" src="https://github.com/user-attachments/assets/ffacb2dd-b06b-4ded-92dc-616b18b25197" />

4 VERIFICANDO SE O ARQUIVO FOI CRIADO
<img width="1301" height="838" alt="image" src="https://github.com/user-attachments/assets/27778361-cbc4-4ce1-84be-d1e7fa1e16bf" />


ATIVIDADE 2

5 CRIANDO O SHELL SCRIPT: VERIFICA_SGD.SH
<img width="1280" height="841" alt="image" src="https://github.com/user-attachments/assets/0892478f-b667-4fa9-bebd-be6a9b0728bb" />

6 SCRIPT CRIADO, VERIFICADO E COM PERMISSÃO DE EXECUÇÃO CONCEDIDA.
<img width="1280" height="785" alt="image" src="https://github.com/user-attachments/assets/7084ca0b-c584-45e1-8af6-88779cba0dec" />

7 NA PRIMEIRA EXECUÇÃO DEU ERRO PORQUE NÃO HAVIA ESPAÇO ENTRE O "S" DE DADOS E O COLCHETE QUE FECHA O TESTE.
<img width="1280" height="820" alt="image" src="https://github.com/user-attachments/assets/fd546c35-b708-405a-9ef6-0d634d6e4147" />

8 ADICIONANDO NO SCRIPT O COMANDO PARA CRIAR O ARQUIVO verificacao.log.
<img width="1280" height="809" alt="image" src="https://github.com/user-attachments/assets/8c29e6d2-f239-4215-ac85-f98f10f4035f" />

9 INSERINDO O RESTANTE DO SCRIPT.
<img width="1280" height="826" alt="image" src="https://github.com/user-attachments/assets/fd9ef37f-686d-4766-9b58-43723fb2c860" />

10 VERIFICANDO
<img width="1280" height="797" alt="image" src="https://github.com/user-attachments/assets/26f1441e-122c-43ae-81f1-ec76bf489966" />

11 TESTE COM PASTA INEXISTENTE.
<img width="1280" height="840" alt="image" src="https://github.com/user-attachments/assets/299dda4e-94ce-4a6c-ad06-4549f683313c" />

12 VERIFICAÇÃO COM SCRIPT DE VERIFICAÇÃO DE UMA PASTA QUE NÃO EXISTE.
<img width="1280" height="789" alt="image" src="https://github.com/user-attachments/assets/85f97023-9a9a-4102-95f2-55e5f8044acd" />

 ATIVIDADE 3

 13 CRIANDO SCRIPT DE AUDITORIA DE ARQUIVOS.
 <img width="1280" height="822" alt="image" src="https://github.com/user-attachments/assets/5818a978-7fda-474a-8113-8382191ef574" />

 14 CONCEDENDO PERMISSÃO DE EXECUÇÃO.
 <img width="1280" height="844" alt="image" src="https://github.com/user-attachments/assets/749455e7-561a-44ea-8d0e-edc26f89c723" />

 15 EXECUTANDO O SCRIPT
 <img width="1280" height="827" alt="image" src="https://github.com/user-attachments/assets/2b894dbd-943f-4867-8ceb-8cf978aa5e2a" />

 16 NESTA PARTE EU ME CONFUNDI, POIS DEI UM CAT EM AUDITORIA.SH QUANDO, NA VERDADE, DEVERIA DAR UM CAT EM AUDITORIA.TXT.
 <img width="1280" height="818" alt="image" src="https://github.com/user-attachments/assets/f7461816-dbef-45c9-aad3-c69b705550e7" />

 17 DANDO UM CAT NO ARQUIVO CERTO
 <img width="1280" height="804" alt="image" src="https://github.com/user-attachments/assets/e2936c0f-d40b-4c65-a8a9-4089bed8a8c1" />

 18 COLOCANDO UM FILTRO PARA MOSTRAR APENAS ARQUIVOS NO DOCUMENTO AUDITORIA.TXT
 <img width="1280" height="827" alt="image" src="https://github.com/user-attachments/assets/b450a12f-3720-4dd3-9d51-33c220fd020e" />

 19 RELATÓRIO
 <img width="1280" height="844" alt="image" src="https://github.com/user-attachments/assets/ef69c8cf-f2f3-4408-9259-09767b8d9055" />

 ATIVIDADE 4

 20 NESTA PARTE FIQUEI TRAVADO PORQUE ESTAVA LOGADO COMO USUÁRIO COMUM. TIVE QUE TROCAR DE USUÁRIO PARA CONSEGUIR CRIAR NOVOS USUÁRIOS.
 <img width="1280" height="791" alt="image" src="https://github.com/user-attachments/assets/018f9b18-8dfc-4bdf-a346-f6a040e5618e" />

 21 CRIANDO USUÁRIO E SENHA.
 <img width="1280" height="798" alt="image" src="https://github.com/user-attachments/assets/4c971909-a7f9-481e-8aed-f45e956ed908" />

 22 ALTERANDO O DONO DO DIRETÓRIO DADOS PARA USUÁRIO_TOTAL.
 <img width="1280" height="799" alt="image" src="https://github.com/user-attachments/assets/83f86bb7-1e0e-4573-baf7-8c515d1d8d2a" />

 23 DANDO ACESSO TOTAL PARA USUÁRIO_TOTAL.
 <img width="1280" height="771" alt="image" src="https://github.com/user-attachments/assets/202f9165-b3d3-430a-bc61-da5f7b371e9f" />

 24 CONCEDENDO PERMISSÃO AO DONO, CRIANDO GRUPO E DANDO PERMISSÃO DE LEITURA. E TESTANDO O ACESSO DOS USUÁRIOS E SUAS PERMISSÕES.
 <img width="1280" height="831" alt="image" src="https://github.com/user-attachments/assets/6e345f5f-8456-4944-a71d-d4ae7f2f4894" />

 ATIVIDADE 5

 25 CRIANDO NOVO AMBIENTE COM NOVOS DIRETÓRIOS.
 <img width="1280" height="811" alt="image" src="https://github.com/user-attachments/assets/ee94ea29-08aa-4d06-ac8c-5687bbc5f193" />

 26 CRIANDO NOVO SCRIPT.
 <img width="1280" height="820" alt="image" src="https://github.com/user-attachments/assets/292d8182-1e9e-47e0-8c4e-cd6cb27af6c5" />

 27 EXECUTANDO O NOVO SCRIPT.
 <img width="1280" height="821" alt="image" src="https://github.com/user-attachments/assets/07ecaebf-ca2c-435d-a129-72690d86d2df" />

 28 CRIANDO VERIFICADOR DE DIRETÓRIO.
 <img width="1280" height="815" alt="image" src="https://github.com/user-attachments/assets/d7db347f-abe4-4d0f-9c57-68f6ec15a8fd" />

 29 DANDO PERMISSÃO E EXECUTANDO O SCRIPT.
 <img width="1280" height="809" alt="image" src="https://github.com/user-attachments/assets/c2811e93-d569-4a6e-8551-5490089dbf4d" />

 30 CRIANDO AUDITORIA.SH, DANDO PERMISSÃO E EXECUTANDO.
 <img width="1280" height="764" alt="image" src="https://github.com/user-attachments/assets/cacc718c-61c5-480c-8a20-2f6f2ff07ec6" />

 31 VERIFICANDO AUDITORIA.TXT.
 <img width="1280" height="816" alt="image" src="https://github.com/user-attachments/assets/89c96969-f6c4-4c66-aeb3-3a66b2744086" />

 32 CRIANDO NOVOS USUÁRIOS E SENHAS.
 <img width="1280" height="823" alt="image" src="https://github.com/user-attachments/assets/a6682ae4-a182-4c55-a541-d82626959d3b" />

 33 TORNANDO OPERADOR DONO DO DIRETÓRIO DOCUMENTOS E DANDO PERMISSÕES.
 <img width="1280" height="825" alt="image" src="https://github.com/user-attachments/assets/6abd9d0e-f3e9-49b9-a435-6b2b853ba560" />

 34 CRIANDO GRUPO E DANDO PERMISSÃO.
 <img width="1280" height="764" alt="image" src="https://github.com/user-attachments/assets/1be5b894-acbf-43a4-96fc-69af67139344" />

35 TESTANDO PERMISSÕES.
<img width="1280" height="796" alt="image" src="https://github.com/user-attachments/assets/550c62dc-dcfa-4986-b819-1bb6dcba2f0e" />

36 APÓS ALGUNS ERROS, CONSEGUI COLOCAR A PERMISSÃO CORRETA PARA O GRUPO CONSEGUIR ACESSAR O DIRETÓRIO.
<img width="1280" height="850" alt="image" src="https://github.com/user-attachments/assets/e49e93a0-8d8a-4db3-9617-686e6d1186e8" />































