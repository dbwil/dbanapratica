

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


Criando ambiente básico para armazenagem se Dados
<img width="1280" height="761" alt="image" src="https://github.com/user-attachments/assets/1f77b94c-1dda-41db-aca4-d76a648e187d" />

Navegando pelos diretórios
<img width="1280" height="752" alt="image" src="https://github.com/user-attachments/assets/b69d98f1-791e-45bc-ad48-1ab73fb2d218" />

Criando arquivo .txt
<img width="1280" height="760" alt="image" src="https://github.com/user-attachments/assets/10bff50c-35db-4221-ad36-0d6ad76c7135" />

Colocando conteúdo dentro do arquivo
<img width="1280" height="847" alt="image" src="https://github.com/user-attachments/assets/1e954062-dacd-4363-9910-78bb6f18af1a" />

Colocando conteúdo dentro do arquivo log.txt
<img width="1280" height="718" alt="image" src="https://github.com/user-attachments/assets/0104b16a-4be5-403c-b976-48855b9c26e8" />

Criando usuário e senha
<img width="1280" height="822" alt="image" src="https://github.com/user-attachments/assets/e8bd6ba0-9310-401e-bf35-c9670cabb7bd" />

Fazendo comando para bloquear grupo e usuários e permitir que somente o dono faça alterações na pasta backups
<img width="1280" height="790" alt="image" src="https://github.com/user-attachments/assets/36a4e948-1ba0-49a1-b705-3e79dc240be8" />

Conferindo se deu certo
<img width="1280" height="820" alt="image" src="https://github.com/user-attachments/assets/950bf71a-5525-4e35-84f2-bceda621c3f2" />

Fiquei travado aqui nesta hora da conferência de permissões
<img width="1280" height="745" alt="image" src="https://github.com/user-attachments/assets/9eda1a08-00dc-4b8f-bba3-3a8801a7df35" />

Consegui acessar a pasta com usuário analista quando dei o comando chmod 755/home/wsantos
<img width="1280" height="781" alt="image" src="https://github.com/user-attachments/assets/038ad348-2120-4a41-b432-c69403d46ade" />

Destravou aqui
<img width="1280" height="804" alt="image" src="https://github.com/user-attachments/assets/1eb55bcd-60e2-4a4c-b6a9-0a26873a24fb" />

Exercício concluído pois não tive acesso a pasta backups
<img width="1280" height="808" alt="image" src="https://github.com/user-attachments/assets/3fb3b960-9065-4974-ac26-068bac0ccfd0" />

Na parte de processos e criação de script fiquei um pouco perdido principalmente na hora de salvar script
<img width="1280" height="816" alt="image" src="https://github.com/user-attachments/assets/adb7d19d-2573-48d6-8acf-14fc6f6301e9" />

Arquivo rodando
<img width="1280" height="816" alt="image" src="https://github.com/user-attachments/assets/47402068-2ea8-4944-8bd0-d08cb1bf7594" />

Como ficou o terminal
<img width="1280" height="757" alt="image" src="https://github.com/user-attachments/assets/b6a4a1de-f09a-4682-a08f-9549f83f3da7" />

Resultado após filtrar o processo
<img width="1280" height="764" alt="image" src="https://github.com/user-attachments/assets/c9b58e5d-f567-47e6-a98a-31c16d98e1d1" />

Terminado o processo 4572
<img width="1280" height="839" alt="image" src="https://github.com/user-attachments/assets/c7009768-682f-4dd8-9be0-4942fa434f7e" />

Confirmado processo encerrado
<img width="1280" height="845" alt="image" src="https://github.com/user-attachments/assets/da0a1461-5a19-4d5c-9701-54345d961989" />

Comandos aprendidos e executados ao longo da quinzena:

mkdir = cria pasta

mkdir -p = cria estrutura de pastas

cd = entra em pasta

ls = lista arquivos

pwd = mostra onde está

echo = escreve texto

touch = cria arquivo

cat = mostra conteúdo

ls -l = lista detalhada

ls -a = mostra ocultos

useradd = cria usuário

passwd = cria senha

chmod = muda permissões

chown = muda dono

su - = troca usuário

nano = editor de texto

chmod +x = permite execução

./script.sh = executa script

& = executa em segundo plano

ps aux = mostra processos

grep = procura texto

kill = encerra processo

top = monitora sistema














