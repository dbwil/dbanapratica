# dbanapratica — Quinzena 5 (Semana 1)
Período: 23/05 → 30/05

Tema: Editores de texto e introdução à automação com Shell Script

Base: Rocky Linux

---

## Objetivo da semana

Ao final desta semana você deverá ser capaz de:

- Utilizar o nano para edição de arquivos
- Realizar operações básicas no vi
- Criar e executar scripts bash simples
- Entender permissões de execução
- Registrar informações em arquivos de log
- Começar a automatizar tarefas simples

---

## Atividade 1 — Dominando o Nano

Nível: Fundamental

### Tempo esperado

| Etapa | Tempo |
|---------|---------:|
| Leitura | 20 min |
| Vídeos | 30 min |
| Prática | 1h10min |
| **Total** | **2h** |

### Enunciado

Você recebeu a responsabilidade de documentar o ambiente SGD criado até o momento.

Crie um arquivo chamado `README.md` dentro do diretório principal do projeto.

O documento deve conter:

- nome do projeto
- objetivo do projeto
- descrição dos diretórios existentes
- descrição dos arquivos existentes

Ao longo da semana realize ao menos três atualizações nesse arquivo.

### Direção de estudo

Aprender a criar, editar, salvar e pesquisar conteúdo utilizando o editor Nano.

### Conteúdo sugerido

#### Leitura

- https://www.hostinger.com.br/tutoriais/como-usar-editor-nano-linux
- https://www.vivaolinux.com.br/artigo/Editor-de-textos-GNU-Nano

#### Vídeos

- Nano Editor Linux - Bóson Treinamentos
- Introdução ao Nano - Curso Linux

### Palavras-chave para pesquisa

- nano linux tutorial
- nano salvar arquivo
- nano localizar texto

### Comandos úteis

```bash
nano
cat
less
```

### Checklist

- [ ] README.md criado
- [ ] Conteúdo editado
- [ ] Arquivo salvo corretamente
- [ ] Arquivo atualizado mais de uma vez
- [ ] Consegue localizar informações dentro do arquivo

---

## Atividade 2 — Sobrevivendo ao VI

Nível: Fundamental

### Tempo esperado

| Etapa | Tempo |
|---------|---------:|
| Leitura | 20 min |
| Vídeos | 40 min |
| Prática | 1h |
| **Total** | **2h** |

### Enunciado

Utilizando exclusivamente o editor vi, abra o arquivo README.md criado anteriormente e realize novas alterações.

Você deve conseguir:

- abrir arquivo
- inserir texto
- apagar linhas
- pesquisar palavras
- salvar alterações
- sair sem salvar alterações

O objetivo não é dominar o vi, mas conseguir utilizá-lo sem travar.

### Direção de estudo

Entender os modos de operação do vi.

### Conteúdo sugerido

#### Leitura

- https://www.vivaolinux.com.br/artigo/Introducao-ao-VI
- https://www.hostinger.com.br/tutoriais/vim-comandos-linux

#### Vídeos

- Vim para Iniciantes - Bóson Treinamentos
- Introdução ao Vim - LinuxTips

### Palavras-chave para pesquisa

- vi básico linux
- vim comandos essenciais
- como sair do vi

### Comandos úteis

```bash
vi
vim
```

### Checklist

- [ ] Arquivo aberto
- [ ] Conteúdo alterado
- [ ] Alterações salvas
- [ ] Saiu corretamente
- [ ] Consegue sair sem salvar

---

## Atividade 3 — Primeiro Script Bash

Nível: Básico

### Tempo esperado

| Etapa | Tempo |
|---------|---------:|
| Leitura | 20 min |
| Vídeos | 40 min |
| Desenvolvimento | 1h |
| Testes | 1h |
| **Total** | **3h** |

### Enunciado

Crie um script chamado `boasvindas.sh`.

O script deve exibir:

- nome do usuário logado
- data e hora atuais
- diretório atual

Ao final, o script deve exibir uma mensagem indicando que a execução foi concluída.

### Direção de estudo

Entender o conceito de shell script e automação básica.

### Conteúdo sugerido

#### Leitura

- https://ryanstutorials.net/bash-scripting-tutorial/
- https://www.vivaolinux.com.br/artigo/Shell-script-para-iniciantes

#### Vídeos

- Shell Script para Iniciantes - Bóson Treinamentos
- Bash Script Básico - LinuxTips

### Palavras-chave para pesquisa

- shell script básico
- bash script tutorial
- chmod +x

### Comandos úteis

```bash
echo
whoami
date
pwd
chmod +x
```

### Checklist

- [ ] Script criado
- [ ] Script executado
- [ ] Permissão de execução aplicada
- [ ] Saída correta exibida
- [ ] Entende a função do chmod +x

---

## Entregáveis da semana

Você deve apresentar:

- [ ] README.md
- [ ] boasvindas.sh
- [ ] Evidências de execução
- [ ] Dificuldades encontradas
- [ ] Resumo (máximo 10 linhas) sobre:
  - Nano
  - VI
  - Shell Script

---

## Encontro de validação

### Perguntas

1. Qual a diferença entre nano e vi?
2. O que é modo de inserção no vi?
3. Como sair do vi sem salvar alterações?
4. O que é um shell script?
5. O que faz o shebang (`#!/bin/bash`)?
6. O que faz o chmod +x?
7. Qual a diferença entre executar:
   - `bash script.sh`
   - `./script.sh`
8. Qual atividade foi mais difícil?
9. O que você precisou pesquisar além do material sugerido?

---

## Critério de aprovação

- [ ] README.md concluído
- [ ] Script funcional
- [ ] Consegue editar arquivos em nano
- [ ] Consegue realizar operações básicas em vi
- [ ] Consegue explicar o funcionamento básico do script criado

Caso contrário, os tópicos deverão ser reforçados antes do avanço para a próxima semana.
