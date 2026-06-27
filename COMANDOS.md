
Administração de arquivos e diretórios

- ls = Lista arquivos e diretórios
- ls -l = Lista detalhada
- ls -a = Mostra todos os arquivos e pastas, incluindo os arquivos ocultos.
- cd /caminho = Entra em um diretório
- cd .. = Volta um diretório
- pwd = Mostra o diretório atual
- mkdir nome = Cria diretório
- rm arquivo = Remove arquivo
- rm -r pasta = Remove pasta
-cp origem destino = Copia arquivos
- mv origem destino = Move ou renomeia

Administração de usuários e senhas

- adduser nome = Cria usuário
- passwd nome = Define senha
- userdel nome = Remove usuário
- whoami = Mostra usuário atual
- id = Mostra informações do usuário

Administração de permissões

- chmod 777 arquivo = Altera permissões (Libera tudo)
- chmod 644 arquivo = Permissão normal (Comum)
- chmod 000 arquivo = Ninguém consegue usar o arquivo (Tira as permissões - Bloquear).
- chmod +x arquivo = Torna executável
- chown usuario:grupo arquivo = Muda dono
 exemplo chown wsantos arquivo.txt
- ls -l = Ver permissões

Arquivos ocultos (de “sombra”)

- ls -a = Mostra arquivos ocultos
- ls -la = Lista detalhada incluindo ocultos

Outros comandos úteis

- clear = Limpa o terminal
- history = Mostra histórico de comandos
- man comando = Manual do comando
-echo "texto" = Exibe texto

ATUALIZAÇÃO:
# APOSTILA DE COMANDOS LINUX

## Curso DBA na Prática – Resumo da 1ª à 7ª Quinzena

# 1. NAVEGAÇÃO NO SISTEMA

## pwd

**Significado:** Print Working Directory.

**Função:** Mostra o diretório onde você está no momento.

**Exemplo:**

```bash
pwd
```

---

## ls

**Função:** Lista os arquivos e diretórios do local atual.

**Exemplo:**

```bash
ls
```

---

## ls -l

**Função:** Lista arquivos mostrando permissões, dono, grupo, tamanho e data.

**Exemplo:**

```bash
ls -l
```

---

## ls -la

**Função:** Lista todos os arquivos, inclusive os ocultos.

**Exemplo:**

```bash
ls -la
```

---

## cd

**Significado:** Change Directory.

**Função:** Entrar em um diretório.

**Exemplos:**

```bash
cd documentos
cd ..
cd ~
cd /
```

---

## clear

**Função:** Limpa a tela do terminal.

```bash
clear
```

---

# 2. CRIAÇÃO DE DIRETÓRIOS E ARQUIVOS

## mkdir

**Significado:** Make Directory.

**Função:** Criar diretórios.

```bash
mkdir projetos
```

Criando vários:

```bash
mkdir dados registros backups
```

---

## touch

**Função:** Criar arquivos vazios.

```bash
touch arquivo.txt
```

---

## cp

**Significado:** Copy.

**Função:** Copiar arquivos ou diretórios.

```bash
cp arquivo.txt copia.txt
```

---

## mv

**Significado:** Move.

**Função:** Mover ou renomear arquivos.

Mover:

```bash
mv arquivo.txt documentos/
```

Renomear:

```bash
mv antigo.txt novo.txt
```

---

## rm

**Função:** Remover arquivos.

```bash
rm arquivo.txt
```

---

## rm -r

**Função:** Remover diretórios com todo o conteúdo.

```bash
rm -r pasta
```

---

## rmdir

**Função:** Remove apenas diretórios vazios.

```bash
rmdir pasta
```

---

# 3. VISUALIZAÇÃO DE ARQUIVOS

## cat

**Função:** Mostrar o conteúdo de um arquivo.

```bash
cat arquivo.txt
```

---

## less

**Função:** Ler arquivos grandes página por página.

```bash
less arquivo.txt
```

Sair pressionando:

```text
q
```

---

## more

**Função:** Visualizar arquivos grandes.

```bash
more arquivo.txt
```

---

# 4. PESQUISA

## grep

**Função:** Procurar uma palavra dentro de um arquivo.

```bash
grep Port /etc/ssh/sshd_config
```

Também pode pesquisar na saída de outro comando:

```bash
journalctl -u sshd | grep Accepted
```

---

## find

**Função:** Procurar arquivos ou diretórios.

```bash
find .
```

---

# 5. USUÁRIOS

## whoami

**Função:** Mostrar o usuário logado.

```bash
whoami
```

---

## id

**Função:** Mostrar informações completas do usuário.

```bash
id
```

---

## groups

**Função:** Mostrar os grupos do usuário.

```bash
groups
```

---

## su

**Significado:** Switch User.

**Função:** Trocar de usuário.

```bash
su -
```

---

## useradd

**Função:** Criar usuários.

```bash
useradd operador
```

---

## passwd

**Função:** Criar ou alterar senha.

```bash
passwd operador
```

---

# 6. PERMISSÕES

## chmod

**Significado:** Change Mode.

**Função:** Alterar permissões.

```bash
chmod 740 dados
```

Significado dos números:

7 = leitura + escrita + execução

4 = somente leitura

0 = sem acesso

---

## chown

**Significado:** Change Owner.

**Função:** Alterar o dono de um arquivo.

```bash
chown operador dados
```

---

## chgrp

**Significado:** Change Group.

**Função:** Alterar o grupo.

```bash
chgrp leitores dados
```

---

# 7. SHELL SCRIPT

## nano

**Função:** Criar ou editar scripts.

```bash
nano relatorio.sh
```

---

## echo

**Função:** Mostrar mensagens ou gravar em arquivos.

```bash
echo "Olá"
```

Gravar em arquivo:

```bash
echo "Teste" > arquivo.txt
```

---

## if

**Função:** Executar ações dependendo de uma condição.

Exemplo:

```bash
if [ -d dados ]
then
echo "Existe"
fi
```

---

## for

**Função:** Repetir comandos várias vezes.

```bash
for arquivo in *
do
echo $arquivo
done
```

---

## Testes

Verificar diretório:

```bash
[ -d dados ]
```

Verificar arquivo:

```bash
[ -f teste.txt ]
```

---

# 8. GERENCIAMENTO DE PACOTES

## dnf install

**Função:** Instalar programas.

```bash
dnf install tree
```

---

## dnf remove

**Função:** Remover programas.

```bash
dnf remove tree
```

---

## dnf reinstall

**Função:** Reinstalar programas.

```bash
dnf reinstall tree
```

---

## dnf info

**Função:** Mostrar informações do pacote.

```bash
dnf info tree
```

---

## rpm -qc

**Função:** Mostrar os arquivos de configuração de um pacote.

```bash
rpm -qc openssh-server
```

---

## which

**Função:** Mostrar onde o programa está instalado.

```bash
which tree
```

---

# 9. SERVIÇOS (SYSTEMD)

## systemctl status

**Função:** Verificar o status de um serviço.

```bash
systemctl status sshd
```

---

## systemctl start

**Função:** Iniciar um serviço.

```bash
systemctl start sshd
```

---

## systemctl stop

**Função:** Parar um serviço.

```bash
systemctl stop sshd
```

---

## systemctl restart

**Função:** Reiniciar um serviço.

```bash
systemctl restart sshd
```

---

## systemctl enable

**Função:** Fazer o serviço iniciar automaticamente.

```bash
systemctl enable sshd
```

---

## systemctl disable

**Função:** Impedir a inicialização automática.

```bash
systemctl disable sshd
```

---

## systemctl is-enabled

**Função:** Verificar se inicia automaticamente.

```bash
systemctl is-enabled sshd
```

---

# 10. ARQUIVOS DE CONFIGURAÇÃO

Consultar arquivos de configuração:

```bash
rpm -qc openssh-server
```

Ler arquivo:

```bash
cat /etc/ssh/sshd_config
```

Pesquisar configuração:

```bash
grep Port /etc/ssh/sshd_config
```

---

# 11. LOGS

## journalctl

**Função:** Mostrar todos os logs do sistema.

```bash
journalctl
```

---

## journalctl -u

**Função:** Mostrar os logs de um serviço.

```bash
journalctl -u sshd
```

---

## journalctl -n

**Função:** Mostrar apenas as últimas linhas.

```bash
journalctl -u sshd -n 20
```

---

## journalctl -f

**Função:** Acompanhar os logs em tempo real.

```bash
journalctl -u sshd -f
```

---

# 12. REDE

## ping

**Função:** Testar conexão com outro computador.

```bash
ping google.com
```

---

# 13. DATA E HORA

## date

**Função:** Mostrar a data e hora.

```bash
date
```

---

## timedatectl

**Função:** Mostrar e configurar data, hora e fuso horário.

```bash
timedatectl
```

---

# 14. COMANDOS IMPORTANTES PARA MEMORIZAR

* pwd
* ls
* cd
* mkdir
* touch
* cp
* mv
* rm
* cat
* grep
* find
* chmod
* chown
* chgrp
* whoami
* id
* groups
* su
* useradd
* passwd
* nano
* echo
* if
* for
* dnf
* rpm
* which
* systemctl
* journalctl
* ping
* date
* timedatectl



