# 📘 dbanapratica — Plano dirigido (Linux + PostgreSQL)

Base: **Rocky Linux** + **PostgreSQL**

Projeto contínuo:

> Construir um ambiente real (filesystem + PostgreSQL) e evoluir até administração básica.

---

# 🔹 Quinzena 1 — Fundamentos de Linux (base forte)

📌 **Foco:** não se perder no sistema

---

## 🧭 Direção

> Entender como navegar, criar arquivos e organizar diretórios no Linux.

---

## 🎥 Vídeos

* YouTube
  👉 *Curso em Vídeo - Linux (parte comandos básicos)*
* YouTube
  👉 *Comandos básicos Linux (cd, ls, pwd)*

---

## 📖 Leitura

* [https://www.guiafoca.org/guiaonline/iniciante/ch03.html](https://www.guiafoca.org/guiaonline/iniciante/ch03.html)

---

## 🧪 Exercícios

```bash
mkdir -p ~/sgd/{dados,logs,scripts,backups}

echo "usuario1,email@test.com" > ~/sgd/dados/usuarios.txt
echo "log inicial" > ~/sgd/logs/app.log
```

Praticar:

```bash
cd
ls
pwd
```

---

## ✅ Resultado esperado

* navega sem travar
* entende onde está

---

# 🎯 ENCONTRO 1 — Navegação

### ❓ Perguntas

1. Onde você está quando abre o terminal?
2. Qual a diferença entre caminho absoluto e relativo?
3. O que acontece ao executar `cd /`?
4. Como você volta para sua home?
5. Como listar arquivos de outro diretório sem entrar nele?
6. O que o `.` e `..` representam?

---

---

# 🔹 Quinzena 2 — Permissões + processos

📌 **Foco:** controle + funcionamento do sistema

---

## 🧭 Direção

> Entender quem pode acessar arquivos e como processos funcionam.

---

## 🎥 Vídeos

* YouTube
  👉 *Permissões no Linux explicação simples*
* YouTube
  👉 *Processos Linux (ps, top, kill)*

---

## 📖 Leitura

* [https://www.vivaolinux.com.br/artigo/Permissoes-de-arquivos-no-Linux](https://www.vivaolinux.com.br/artigo/Permissoes-de-arquivos-no-Linux)

---

## 🧪 Exercícios

```bash
sudo useradd analista
chmod 700 ~/sgd/backups
chmod 755 ~/sgd/dados
```

Processos:

* criar script de loop
* rodar em background
* matar processo

---

## 🔗 Evolução

Permissões → depois banco
Processos → logs e serviços

---

# 🎯 ENCONTRO 2 — Permissões

### ❓ Perguntas

1. O que significa 755?
2. Quem é o dono de um arquivo?
3. Qual a diferença entre leitura, escrita e execução?
4. O que acontece se remover permissão de execução?
5. Como mudar o dono de um arquivo?

---

# 🎯 ENCONTRO 3 — Processos

### ❓ Perguntas

1. O que é um processo?
2. O que é PID?
3. Como listar processos?
4. O que significa rodar em background?
5. O que acontece se não matar um loop infinito?
6. Como identificar o processo correto?

---

---

# 🔹 Quinzena 3 — Instalação + serviço PostgreSQL

📌 **Foco:** PostgreSQL como serviço Linux

---

## 🧭 Direção

> Entender como instalar e gerenciar serviços no Linux.

---

## 🎥 Vídeos

* YouTube
  👉 *systemctl explicado*
* YouTube
  👉 *Instalar PostgreSQL no Linux*

---

## 📖 Leitura

* [https://www.postgresql.org/docs/current/tutorial-install.html](https://www.postgresql.org/docs/current/tutorial-install.html)

---

## 🧪 Exercícios

* instalar PostgreSQL
* inicializar cluster
* start + enable

---

## 🔗 Evolução

Base do ambiente

---

# 🎯 ENCONTRO 4 — Serviços

### ❓ Perguntas

1. O que é um serviço no Linux?
2. Diferença entre start e enable?
3. Como verificar status de um serviço?
4. O que acontece se o PostgreSQL parar?
5. Onde ficam os dados do PostgreSQL?

---

---

# 🔹 Quinzena 4 — Acesso + autenticação

📌 **Foco:** acesso ao banco

---

## 🧭 Direção

> Entender como o PostgreSQL controla acesso.

---

## 🎥 Vídeos

* YouTube
  👉 *pg_hba.conf explicado*

---

## 📖 Leitura

* [https://www.postgresql.org/docs/current/auth-pg-hba-conf.html](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html)

---

## 🧪 Exercícios

* acessar `psql`
* criar DB `sgd`
* alterar `pg_hba.conf`
* testar login

---

## 🔗 Evolução

Segurança

---

# 🎯 ENCONTRO 5 — Autenticação

### ❓ Perguntas

1. O que é pg_hba.conf?
2. Diferença entre peer e md5?
3. O que acontece se errar esse arquivo?
4. Por que reiniciar o serviço?
5. Como testar autenticação?

---

---

# 🔹 Quinzena 5 — Roles + estrutura

📌 **Foco:** controle interno do banco

---

## 🧭 Direção

> Entender usuários e organização no PostgreSQL.

---

## 🎥 Vídeos

* YouTube
  👉 *Roles no PostgreSQL*

---

## 📖 Leitura

* [https://www.postgresql.org/docs/current/user-manag.html](https://www.postgresql.org/docs/current/user-manag.html)

---

## 🧪 Exercícios

* criar role
* dar permissão
* criar schema
* alterar owner

---

## 🔗 Evolução

Multiusuário

---

# 🎯 ENCONTRO 6 — Roles

### ❓ Perguntas

1. O que é uma role?
2. Diferença entre role e usuário?
3. O que acontece sem GRANT?
4. Como testar permissões?
5. Como restringir acesso?

---

---

# 🔹 Quinzena 6 — Logs + backup + automação

📌 **Foco:** operação real

---

## 🧭 Direção

> Diagnosticar, automatizar e proteger dados.

---

## 🎥 Vídeos

* YouTube
  👉 *Logs PostgreSQL explicação*
* YouTube
  👉 *pg_dump tutorial*

---

## 📖 Leitura

* [https://www.postgresql.org/docs/current/backup-dump.html](https://www.postgresql.org/docs/current/backup-dump.html)

---

## 🧪 Exercícios

* analisar logs com grep
* criar script de carga
* backup + restore

---

## 🔗 Evolução

Administração real

---

# 🎯 ENCONTRO 7 — Logs

### ❓ Perguntas

1. Onde ficam os logs?
2. Como identificar erro?
3. Como usar grep?
4. Qual erro você encontrou?
5. Como investigar problema?

---

# 🎯 ENCONTRO 8 — Backup + automação

### ❓ Perguntas

1. O que é pg_dump?
2. Como restaurar banco?
3. O que acontece se o banco existir?
4. O que acontece se o script falhar?
5. Como automatizar backup?
6. Qual risco de não ter backup?

---

---

# 🎯 Resultado final

* Linux sólido (base real)
* PostgreSQL como serviço (não só SQL)
* prática evolutiva (SGD)
* encontros estruturados
* perguntas suficientes pra validar aprendizado
