

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

* [ ] Estrutura `sgd` criada corretamente
* [ ] Subdiretórios existem
* [ ] Arquivos foram criados
* [ ] Você consegue navegar até qualquer diretório

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

* [ ] Você navega entre diretórios sem erro
* [ ] Usa caminho absoluto corretamente
* [ ] Usa caminho relativo corretamente
* [ ] Lista arquivos sem entrar no diretório

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

* [ ] Usuário criado
* [ ] Permissões configuradas
* [ ] Usuário acessa `dados`
* [ ] Usuário não acessa `backups`

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

* [ ] Script executando continuamente
* [ ] Log sendo atualizado
* [ ] Processo identificado corretamente
* [ ] Processo finalizado corretamente

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

* [ ] Lista de comandos utilizados
* [ ] Evidências (prints ou saídas)
* [ ] Dificuldades encontradas
* [ ] Explicação curta (resumo):

  * [ ] Navegação
  * [ ] Permissões
  * [ ] Processos

---
