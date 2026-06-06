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

* [ ] Relatório gerado
* [ ] Informações corretas
* [ ] Arquivo criado
* [ ] Variáveis utilizadas
* [ ] Consegue explicar o funcionamento das variáveis utilizadas

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

---

# Orientações para avaliação

Durante a reunião de acompanhamento, observar:

* Se consegue explicar a lógica dos scripts sem ler o código
* Se entende a diferença entre variável e comando
* Se compreende quando utilizar um IF
* Se compreende quando utilizar um FOR
* Se consegue identificar erros simples em scripts
* Se ainda demonstra insegurança com permissões

---

# Perguntas para avaliação

1. O que é uma variável?
2. Como armazenar a saída de um comando em uma variável?
3. Qual a diferença entre uma variável e um comando?
4. O que é uma condicional?
5. Quando utilizar um IF?
6. O que acontece quando uma condição não é satisfeita?
7. O que é um laço de repetição?
8. Quando utilizar um FOR?
9. Qual a diferença entre FOR e WHILE?
10. Como verificar se um diretório existe?
11. Como você investigaria um script que não está funcionando?
12. Como descobrir quem é o proprietário de um arquivo?
13. Qual a diferença entre chmod e chown?
14. Qual atividade foi mais difícil?
15. O que você precisou pesquisar além do material sugerido?

---

# Critério de aprovação

* [ ] Scripts funcionais
* [ ] Consegue explicar os conceitos utilizados
* [ ] Entregou o resumo
* [ ] Consegue modificar os scripts sem ajuda
* [ ] Entende o conceito de permissões e ownership

Caso contrário, os tópicos deverão ser reforçados antes do avanço para a próxima quinzena.
