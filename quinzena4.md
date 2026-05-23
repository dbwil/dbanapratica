

# dbanapratica — Quinzena 4 (02/05 → 23/05)
Tema: Fundamentos de Linux (base sólida)

Base: Rocky Linux + PostgreSQL  
Projeto contínuo: Construção de um ambiente SGD (filesystem + banco futuramente)

---

## Objetivo da quinzena

Ao final, você deve ser capaz de:

- Se localizar no sistema sem se perder  
- Criar e organizar diretórios e arquivos  
- Entender permissões básicas  
- Executar e controlar processos simples  

Se esses pontos não estiverem sólidos, não avançar.

---

## Atividades

### Atividade 1 — Estrutura do ambiente

**Duração sugerida:** 1–2h

#### Enunciado

Você precisa preparar um ambiente básico para armazenar dados e scripts de um projeto.

Crie, dentro da sua pasta pessoal, uma estrutura chamada `sgd` com subdiretórios para:

- dados  
- logs  
- scripts  
- backups  

Dentro dessa estrutura:

- crie um arquivo representando dados de usuários  
- crie um arquivo representando log da aplicação  

#### Direção de estudo

- Estrutura de diretórios no Linux  
- Criação de arquivos e pastas  

#### Buscar

- "linux mkdir criar diretório"  
- "linux echo criar arquivo"  

#### Links

- https://www.guiafoca.org/guiaonline/iniciante/ch03.html  

#### Comandos úteis

```bash
mkdir
mkdir -p
cd
ls
pwd
echo
touch
cat
````

#### Checklist

* [X] Estrutura `sgd` criada corretamente
* [X] Subdiretórios existem
* [X] Arquivos foram criados
* [X] Você consegue navegar até qualquer diretório

---

### Atividade 2 — Navegação consciente

**Duração:** 1–2h

#### Enunciado

Utilizando apenas o terminal, navegue entre os diretórios criados.

Você deve ser capaz de:

* acessar qualquer diretório a partir de outro
* utilizar caminhos absolutos e relativos
* listar arquivos sem entrar no diretório

#### Direção de estudo

* Caminhos absolutos vs relativos

#### Buscar

* "cd linux exemplos"
* "caminho absoluto relativo linux"

#### Links

* [https://www.guiafoca.org/guiaonline/iniciante/ch02.html](https://www.guiafoca.org/guiaonline/iniciante/ch02.html)

#### Comandos

```bash
cd
ls
pwd
ls -l
ls -a
```

#### Checklist

* [X] Você navega entre diretórios sem erro
* [X] Usa caminho absoluto corretamente
* [X] Usa caminho relativo corretamente
* [X] Lista arquivos sem entrar no diretório

---

### Atividade 3 — Permissões básicas

**Duração:** 2–3h

#### Enunciado

Simule um cenário onde existe um usuário chamado `analista` que precisa acessar os dados do projeto, mas não pode acessar os backups.

Para isso:

* crie o usuário
* ajuste permissões dos diretórios
* valide acessando como esse usuário

#### Direção de estudo

* Permissões rwx
* Dono vs grupo vs outros

#### Buscar

* "chmod linux explicado"
* "useradd linux"

#### Links

* [https://www.vivaolinux.com.br/artigo/Permissoes-de-arquivos-no-Linux](https://www.vivaolinux.com.br/artigo/Permissoes-de-arquivos-no-Linux)

#### Comandos

```bash
useradd
passwd
chmod
chown
ls -l
su -
```

#### Checklist

* [X] Usuário criado
* [X] Permissões configuradas
* [X] Usuário acessa `dados`
* [X] Usuário não acessa `backups`

---

### Atividade 4 — Processos e execução

**Duração:** 2–3h

#### Enunciado

Crie um script que simule uma aplicação escrevendo continuamente em um arquivo de log.

Após isso:

* execute o script em segundo plano
* identifique o processo
* finalize o processo manualmente

#### Direção de estudo

* Processos no Linux
* Execução em background

#### Buscar

* "ps aux linux explicado"
* "kill processo linux"
* "bash while loop exemplo"

#### Links

* [https://www.vivaolinux.com.br/artigo/Processos-no-Linux](https://www.vivaolinux.com.br/artigo/Processos-no-Linux)

#### Comandos

```bash
nano
chmod +x
./script.sh
&
ps aux
grep
kill
top
```

#### Checklist

* [X] Script executando continuamente
* [X] Log sendo atualizado
* [X] Processo identificado corretamente
* [X] Processo finalizado corretamente

---

### Atividade 5 — Consolidação (obrigatória)

**Duração:** 1–2h

#### Enunciado

Partindo do zero (sem copiar comandos anteriores), recrie todo o ambiente:

* estrutura de diretórios
* arquivos
* permissões
* script e execução

#### Direção de estudo

* Revisão geral

#### Comandos

* Todos os anteriores

#### Checklist

* [ ] Estrutura recriada
* [ ] Permissões corretas
* [ ] Script funcional
* [ ] Execução sem ajuda

---

## Entregável da quinzena

Você deve apresentar:

* [X] Lista de comandos utilizados
* [X] Evidências (prints ou saídas)
* [X] Dificuldades encontradas
* [X] Explicação curta (resumo):

  * [X] Navegação
  * [X] Permissões
  * [X] Processos

---
Aprendizado 4ª Quinzena:

Na primeira tarefa eu pude aprender como criar diretórios e subdiretórios usando comandos básicos como "mkdir". Também consegui navegar pelos diretórios com alguns comandos como "cd", "ls", "pwd", dentre outros. Cada um com uma função. Exemplo: "pwd" mostra onde eu estou, "ls" lista o conteúdo de um diretório e "cd" serve para eu ir para onde quero.

Também aprendi a criar conteúdo dentro dos diretórios e usar o comando "echo" para colocar um texto dentro de um arquivo. Aprendi também que o "cat" serve para ler um arquivo e que o "touch" cria arquivos vazios.

Aprendi o que é caminho absoluto e caminho relativo. Caminho absoluto é o endereço completo de um diretório, e o caminho relativo é aquele que define de onde eu estou no sistema.

Já nesta atividade aprendi a criar um usuário e ajustar suas permissões. Nesse exercício tive algumas dificuldades na hora de dar acesso ao usuário na pasta "sgd", porém consegui resolver quando utilizei o comando "chmod 755 /home/wsantos", e então consegui destravar essa parte que não estava conseguindo avançar.

Também consegui gerenciar o acesso do usuário "analista" ao diretório "backup".

Os comandos aprendidos nesta aula foram: "useradd", que adiciona usuários; "passwd", que cria senha para usuário; "chmod", que edita as permissões; "chown", que muda o dono; e "su -", que troca de usuário.

Aprendi a criar scripts, executar processos em segundo plano com o comando "ps aux" e também finalizar processos usando o comando "kill".

Aprendi a usar o "nano", que é um editor de texto no terminal. Nesta parte fiquei um pouco perdido na hora de salvar o script e também tive um pouco de dificuldade para fazer o script e entender como ele funciona.

<img width="1280" height="761" alt="image" src="https://github.com/user-attachments/assets/1f77b94c-1dda-41db-aca4-d76a648e187d" />
