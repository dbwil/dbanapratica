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

- [x] README.md criado
- [x] Conteúdo editado
- [x] Arquivo salvo corretamente
- [x] Arquivo atualizado mais de uma vez
- [x] Consegue localizar informações dentro do arquivo

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

- [X] Arquivo aberto
- [X] Conteúdo alterado
- [X] Alterações salvas
- [X] Saiu corretamente
- [X] Consegue sair sem salvar

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

- [x] Script criado
- [x] Script executado
- [x] Permissão de execução aplicada
- [x] Saída correta exibida
- [x] Entende a função do chmod +x

---

## Entregáveis da semana

Você deve apresentar:

- [x] README.md
- [x] boasvindas.sh
- [x] Evidências de execução
- [x] Dificuldades encontradas
- [x] Resumo (máximo 10 linhas) sobre:
  - Nano
  - VI
  - Shell Script

Resumo da Quinzena:

  Nano:
Aprendi a criar e editar arquivos pelo terminal usando o editor nano.

VI:
Aprendi os modos de comando e inserção, além de salvar e sair do editor.

Shell Script:
Aprendi que scripts são arquivos com comandos que podem automatizar tarefas. Aprendi a criar, dar permissão com chmod +x e executar um script Bash.
Aprendi o conceito básico de Shell Script, criando um script Bash simples e executando comandos automaticamente.

Dificuldades encontradas

Minha principal dificuldade foi entender o funcionamento dos editores no Linux, principalmente o VI, por causa dos modos de comando e inserção. Também tive dificuldade no entender este o conceito do comando (#!/bin/bash) como funciona.


EXERCÍCIO 1
<img width="1280" height="689" alt="image" src="https://github.com/user-attachments/assets/7c32db2a-5d67-4f25-8e21-ee0ea5e288b0" />
1 CRIANDO ARQUIVO README.md COM O NANO

<img width="1280" height="687" alt="image" src="https://github.com/user-attachments/assets/1e15bc3b-1cbd-45b2-b959-d7fd21a64156" />
2 DANDO O COMANDO cat README.md 
ESSE COMANDO MOSTRA O CONTEUDO DO ARQUIVO CRIADO E OU SEJA O QUE FOI INSERIDO DENTRO DELE.




