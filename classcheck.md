Sobre este checklist:

* **medir objetivamente**
* resultado: **APROVADO / REPROVADO**

---

# 📘 Critérios gerais (valem para TODOS os encontros)

Antes de cada checklist específico, aplicar sempre:

### 🔴 Reprova automático se:

* não consegue executar sem copiar
* não sabe explicar o que fez
* só “decorou comando”
* não trouxe dúvidas

---

### 🟢 Aprova se:

* executa sem ajuda
* explica com as próprias palavras
* erra e consegue corrigir
* entende o porquê

---

# 🎯 ENCONTRO 1 — Navegação Linux

## ✔ Checklist

* [ ] Consegue navegar usando `cd` sem se perder
* [ ] Sabe ir da home até `~/sgd/dados` sem ajuda
* [ ] Usa `pwd` corretamente
* [ ] Lista arquivos com `ls`
* [ ] Entende `.` e `..`

---

## 🧪 Teste prático

Você diz:

> “vá até a pasta logs sem usar caminho absoluto”

---

## 🟢 Aprovação

✔ faz tudo sem travar + explica

## 🔴 Reprovação

✘ se perde na navegação
✘ não entende caminho

---

# 🎯 ENCONTRO 2 — Permissões

## ✔ Checklist

* [ ] Sabe o que significa 755
* [ ] Consegue alterar permissões com `chmod`
* [ ] Entende diferença entre dono, grupo e outros
* [ ] Sabe bloquear acesso a um diretório
* [ ] Consegue testar com outro usuário

---

## 🧪 Teste prático

> “faça o usuário analista acessar dados mas não backups”

---

## 🟢 Aprovação

✔ resolve sozinho

## 🔴 Reprovação

✘ faz tentativa aleatória
✘ não entende resultado

---

# 🎯 ENCONTRO 3 — Processos

## ✔ Checklist

* [ ] Consegue rodar script em background
* [ ] Usa `ps aux` corretamente
* [ ] Identifica PID correto
* [ ] Mata processo com `kill`
* [ ] Entende o que é processo

---

## 🧪 Teste prático

> “mate o processo que está escrevendo no log”

---

## 🟢 Aprovação

✔ identifica e mata corretamente

## 🔴 Reprovação

✘ mata processo errado
✘ não entende PID

---

# 🎯 ENCONTRO 4 — Serviços (PostgreSQL)

## ✔ Checklist

* [ ] Sabe instalar PostgreSQL
* [ ] Inicializa cluster
* [ ] Usa `systemctl` corretamente
* [ ] Verifica status
* [ ] Entende start vs enable

---

## 🧪 Teste prático

> “pare e suba o PostgreSQL novamente”

---

## 🟢 Aprovação

✔ faz sem ajuda

## 🔴 Reprovação

✘ não entende serviço

---

# 🎯 ENCONTRO 5 — Autenticação

## ✔ Checklist

* [ ] Sabe localizar `pg_hba.conf`
* [ ] Entende peer vs md5
* [ ] Consegue alterar autenticação
* [ ] Reinicia serviço corretamente
* [ ] Testa login

---

## 🧪 Teste prático

> “configure login com senha”

---

## 🟢 Aprovação

✔ login funcionando

## 🔴 Reprovação

✘ quebra acesso e não sabe recuperar

---

# 🎯 ENCONTRO 6 — Roles

## ✔ Checklist

* [ ] Cria role corretamente
* [ ] Dá permissões com GRANT
* [ ] Testa acesso
* [ ] Entende role vs usuário
* [ ] Entende restrição de acesso

---

## 🧪 Teste prático

> “crie um usuário com acesso limitado”

---

## 🟢 Aprovação

✔ acesso funciona corretamente

## 🔴 Reprovação

✘ permissões erradas
✘ não entende GRANT

---

# 🎯 ENCONTRO 7 — Logs

## ✔ Checklist

* [ ] Sabe onde ficam logs
* [ ] Usa `tail -f`
* [ ] Usa `grep`
* [ ] Identifica erro
* [ ] Explica o erro

---

## 🧪 Teste prático

> “encontre erro de login no log”

---

## 🟢 Aprovação

✔ encontra e explica

## 🔴 Reprovação

✘ não consegue interpretar

---

# 🎯 ENCONTRO 8 — Backup + automação

## ✔ Checklist

* [ ] Executa `pg_dump`
* [ ] Restaura banco
* [ ] Entende processo de backup
* [ ] Script funciona
* [ ] Entende falha de script

---

## 🧪 Teste prático

> “apague o banco e recupere”

---

## 🟢 Aprovação

✔ restaura com sucesso

## 🔴 Reprovação

✘ perde dados
✘ não entende restore

---

# 🎯 ENCONTRO FINAL — Projeto completo

## ✔ Checklist

* [ ] Estrutura Linux correta
* [ ] Permissões corretas
* [ ] PostgreSQL funcionando
* [ ] Autenticação configurada
* [ ] Roles funcionando
* [ ] Logs analisáveis
* [ ] Backup funcional
* [ ] Script funcional

---

## 🧪 Teste prático

> “monte tudo do zero”

---

## 🟢 Aprovação

✔ autonomia total

## 🔴 Reprovação

✘ depende de ajuda
✘ não conecta os conceitos

---

# 🎯 Uso prático (importante)

Faça assim:

* 70% correto → aprovado
* <70% → repete conteúdo

---

# ⚠️ Insight importante

* executa mas não explica → reprova
* explica mas não executa → reprova

👉 precisa dos dois
