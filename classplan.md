# 📘 Linux + Administração de PostgreSQL

Base: **Rocky Linux** + **PostgreSQL**

---

# 🔹 SEMANA 1 - Navegação e estrutura

## 🧭 Direção

Entender como navegar no sistema de arquivos.

## 🎥 Vídeos

* YouTube → *“Linux para iniciantes - comandos básicos (Curso em Vídeo)”*
* YouTube → *“Comandos ls, cd, pwd explicados”*

## 📖 Leitura

* [https://www.guiafoca.org/guiaonline/iniciante/ch03.html](https://www.guiafoca.org/guiaonline/iniciante/ch03.html)

---

## 🧪 Exercício

```bash
mkdir -p ~/sgd/{dados,logs,scripts,backups}
echo "usuario1,email1@test.com" > ~/sgd/dados/usuarios.txt
echo "log inicial" > ~/sgd/logs/app.log
```

---

## ❓ Perguntas

* Caminho absoluto vs relativo?

---

---

# 🔹 SEMANA 2 - Permissões

## 🧭 Direção

Controle de acesso no Linux

## 🎥 Vídeos

* YouTube → *“Permissões no Linux (chmod explicado)”*

## 📖 Leitura

* [https://www.vivaolinux.com.br/artigo/Permissoes-de-arquivos-no-Linux](https://www.vivaolinux.com.br/artigo/Permissoes-de-arquivos-no-Linux)

---

## 🧪 Exercício

```bash
sudo useradd analista
chmod 700 ~/sgd/backups
chmod 755 ~/sgd/dados
```

---

## ❓ Perguntas

* O que significa 755?

---

---

# 🔸 ENCONTRO 1

---

---

# 🔹 SEMANA 3 - Processos e logs

## 🧭 Direção

Gerenciar processos

## 🎥 Vídeos

* YouTube → *“Processos no Linux (ps, top, kill)”*

## 📖 Leitura

* [https://www.vivaolinux.com.br/artigo/Processos-no-Linux](https://www.vivaolinux.com.br/artigo/Processos-no-Linux)

---

## 🧪 Exercício

(script + kill)

---

## ❓ Perguntas

* O que é PID?

---

---

# 🔹 SEMANA 4 - PostgreSQL como serviço

## 🧭 Direção

Instalar e rodar serviço

## 🎥 Vídeos

* YouTube → *“Instalando PostgreSQL no Linux (passo a passo)”*

## 📖 Leitura

* [https://www.postgresql.org/docs/current/tutorial-install.html](https://www.postgresql.org/docs/current/tutorial-install.html)

---

## 🧪 Exercício

(instalar + start + enable)

---

## ❓ Perguntas

* O que é um serviço?

---

---

# 🔸 ENCONTRO 2

---

---

# 🔹 SEMANA 5 - Autenticação (pg_hba.conf)

## 🧭 Direção

Controle de acesso ao banco

## 🎥 Vídeos

* YouTube → *“pg_hba.conf explicado (PostgreSQL)”*

## 📖 Leitura

* [https://www.postgresql.org/docs/current/auth-pg-hba-conf.html](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html)

---

## 🧪 Exercício

* editar pg_hba.conf
* testar login

---

## ❓ Perguntas

* diferença peer vs md5

---

---

# 🔹 SEMANA 6 - Roles e permissões

## 🧭 Direção

Controle de usuários no banco

## 🎥 Vídeos

* YouTube → *“Roles e permissões no PostgreSQL”*

## 📖 Leitura

* [https://www.postgresql.org/docs/current/user-manag.html](https://www.postgresql.org/docs/current/user-manag.html)

---

## 🧪 Exercício

* criar role
* dar acesso

---

## ❓ Perguntas

* role vs usuário?

---

---

# 🔸 ENCONTRO 3

---

---

# 🔹 SEMANA 7 - Ownership e schemas

## 🧭 Direção

Organização interna do banco

## 🎥 Vídeos

* YouTube → *“Schemas no PostgreSQL explicados”*

## 📖 Leitura

* [https://www.postgresql.org/docs/current/ddl-schemas.html](https://www.postgresql.org/docs/current/ddl-schemas.html)

---

## 🧪 Exercício

* criar schema
* alterar owner

---

## ❓ Perguntas

* o que é ownership?

---

---

# 🔹 SEMANA 8 - Logs do PostgreSQL

## 🧭 Direção

Diagnóstico

## 🎥 Vídeos

* YouTube → *“Logs no PostgreSQL (como analisar)”*

## 📖 Leitura

* [https://www.postgresql.org/docs/current/runtime-config-logging.html](https://www.postgresql.org/docs/current/runtime-config-logging.html)

---

## 🧪 Exercício

* gerar erro
* analisar logs

---

## ❓ Perguntas

* como identificar erro?

---

---

# 🔸 ENCONTRO 4

---

---

# 🔹 SEMANA 9 - Configuração (postgresql.conf)

## 🧭 Direção

Parâmetros do banco

## 🎥 Vídeos

* YouTube → *“postgresql.conf explicado”*

## 📖 Leitura

* [https://www.postgresql.org/docs/current/runtime-config.html](https://www.postgresql.org/docs/current/runtime-config.html)

---

## 🧪 Exercício

* alterar parâmetro
* validar

---

## ❓ Perguntas

* precisa reiniciar?

---

---

# 🔹 SEMANA 10 - Automação

## 🧭 Direção

Integrar Linux + banco

## 🎥 Vídeos

* YouTube → *“Bash script básico (iniciantes)”*

## 📖 Leitura

* [https://ryanstutorials.net/bash-scripting-tutorial/](https://ryanstutorials.net/bash-scripting-tutorial/)

---

## 🧪 Exercício

* script de carga

---

## ❓ Perguntas

* o que acontece se falhar?

---

---

# 🔸 ENCONTRO 5

---

---

# 🔹 SEMANA 11 - Backup

## 🧭 Direção

Proteção de dados

## 🎥 Vídeos

* YouTube → *“Backup PostgreSQL com pg_dump”*

## 📖 Leitura

* [https://www.postgresql.org/docs/current/backup-dump.html](https://www.postgresql.org/docs/current/backup-dump.html)

---

## 🧪 Exercício

* backup + restore

---

## ❓ Perguntas

* por que backup é importante?

---

---

# 🔹 SEMANA 12 - Projeto final

## 🧭 Direção

Consolidar tudo

---

## 🧪 Desafio

Montar ambiente completo

---

## ❓ Perguntas finais

* onde quebra?
* como investigar?

---

# 🎯 Resultado final

Agora você tem:

* plano consistente
* prática evolutiva
* perguntas de validação
* **material direto (sem ele ter que filtrar muito)**

---

Se quiser deixar isso ainda mais forte, posso:

👉 selecionar **1 único vídeo ideal por semana (o melhor possível)**
👉 ou montar um **roteiro de avaliação tipo prova prática (bem no estilo produção)**
