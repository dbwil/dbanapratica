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

* [ ] Diretórios verificados
* [ ] Condicional funcionando
* [ ] Log gerado
* [ ] Consegue explicar a lógica utilizada

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

* [ ] Loop funcionando
* [ ] Relatório criado
* [ ] Informações corretas
* [ ] Consegue explicar a lógica utilizada

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

* [ ] Cenário implementado
* [ ] Permissões funcionando
* [ ] Documentação criada

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

* [ ] Scripts recriados
* [ ] Scripts executados
* [ ] Resultados corretos
* [ ] Consegue explicar o funcionamento de cada script

---

## Entregáveis da quinzena

Você deve apresentar:

* [ ] relatorio.sh
* [ ] verificador.sh
* [ ] auditoria.sh
* [ ] documentação do exercício de permissões
* [ ] evidências de execução
* [ ] resumo da quinzena

### Resumo obrigatório

Escreva um texto curto (máximo 15 linhas) explicando:

* o que são variáveis
* o que são condicionais
* o que são laços de repetição
* qual atividade foi mais difícil
* quais dúvidas ainda permanecem


ATIVIDADE 1

1 CRIANDO SCRIPT 1° PARTE
<img width="1280" height="772" alt="image" src="https://github.com/user-attachments/assets/241d5dcc-46ab-43ed-86a7-fe1fbcf9e49d" />

2 APÓS A CRIÇÃO DO SCRIPT, A EXECUÇÃO!
<img width="1280" height="792" alt="image" src="https://github.com/user-attachments/assets/96e43127-9661-4cd7-8029-7005f19874a3" />

3 CRIANDO O ARQUIVO QUE SALVA O RELATÓRIO
<img width="1280" height="786" alt="image" src="https://github.com/user-attachments/assets/ffacb2dd-b06b-4ded-92dc-616b18b25197" />

4 VERIFICANDO SE O ARQUIVO FOI CRIADO
<img width="1301" height="838" alt="image" src="https://github.com/user-attachments/assets/27778361-cbc4-4ce1-84be-d1e7fa1e16bf" />



