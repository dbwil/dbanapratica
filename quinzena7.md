# dbanapratica — Quinzena 7

Período: 21/06 → 04/07

Tema: Instalação e Administração Básica de Aplicações Linux

Base: Rocky Linux

---

## Objetivo da quinzena

Ao final desta quinzena você deverá ser capaz de:

* Entender o conceito de pacotes e repositórios
* Instalar e remover aplicações
* Consultar informações sobre pacotes instalados
* Entender o conceito de serviços
* Gerenciar serviços utilizando o systemd
* Localizar e interpretar logs básicos
* Identificar arquivos de configuração
* Aplicar shell script em tarefas administrativas simples

---

## Atividade 1 — Instalando sua Primeira Aplicação

Nível: Básico

### Tempo esperado

| Etapa     |       Tempo |
| --------- | ----------: |
| Leitura   |      20 min |
| Vídeos    |      40 min |
| Pesquisa  |      30 min |
| Prática   |          1h |
| **Total** | **2h30min** |

### Enunciado

Escolha uma aplicação simples para terminal.

Sugestões:

* tree
* htop
* tmux
* screen

Seu objetivo é descobrir:

* como instalar a aplicação
* como verificar se ela foi instalada
* como obter informações sobre o pacote
* como removê-la
* como reinstalá-la

Documente todo o procedimento realizado.

### Direção de estudo

* Pacotes
* Repositórios
* Gerenciamento de software

### Conteúdo sugerido

#### Leitura

* [https://docs.rockylinux.org/books/admin_guide/08-softwares/](https://docs.rockylinux.org/books/admin_guide/08-softwares/)
* [https://www.redhat.com/sysadmin/dnf-command-cheat-sheet](https://www.redhat.com/sysadmin/dnf-command-cheat-sheet)

#### Vídeos

Pesquisar no YouTube:

* "Bóson Treinamentos dnf"
* "LinuxTips gerenciamento de pacotes"

### Palavras-chave para pesquisa

* dnf install package
* dnf remove package
* dnf info package
* linux package manager

### Checklist

* [X] Aplicação instalada
* [X] Aplicação executada
* [X] Informações do pacote consultadas
* [X] Aplicação removida
* [X] Aplicação reinstalada
* [X] Procedimento documentado

---

## Atividade 2 — Conhecendo os Serviços do Sistema

Nível: Básico

### Tempo esperado

| Etapa     |       Tempo |
| --------- | ----------: |
| Leitura   |      20 min |
| Vídeos    |      40 min |
| Pesquisa  |      30 min |
| Prática   |          1h |
| **Total** | **2h30min** |

### Enunciado

Escolha um serviço existente no sistema.

Seu objetivo é descobrir:

* se o serviço está ativo
* se ele inicia automaticamente
* quando foi iniciado
* qual processo está associado a ele

Documente as descobertas.

### Direção de estudo

* Serviços
* Daemons
* Systemd

### Conteúdo sugerido

#### Leitura

* [https://docs.rockylinux.org/books/admin_guide/07-services/](https://docs.rockylinux.org/books/admin_guide/07-services/)

#### Vídeos

Pesquisar no YouTube:

* "systemctl linux bóson treinamentos"
* "linux systemd introdução"

### Palavras-chave para pesquisa

* systemctl status
* systemd services
* linux daemon

### Checklist

* [ ] Serviço identificado
* [ ] Status consultado
* [ ] Processo identificado
* [ ] Inicialização automática compreendida
* [ ] Documentação criada

---

## Atividade 3 — Explorando Arquivos de Configuração

Nível: Básico

### Tempo esperado

| Etapa     |  Tempo |
| --------- | -----: |
| Leitura   | 20 min |
| Pesquisa  | 40 min |
| Prática   |     1h |
| **Total** | **2h** |

### Enunciado

Utilizando a aplicação instalada na Atividade 1 ou o serviço estudado na Atividade 2, descubra:

* onde ficam seus arquivos de configuração
* qual a finalidade desses arquivos
* quais parâmetros podem ser alterados

Realize uma alteração simples e documente o resultado.

### Direção de estudo

* Arquivos de configuração
* Estrutura do Linux

### Conteúdo sugerido

#### Leitura

* [https://www.guiafoca.org/guiaonline/intermediario/](https://www.guiafoca.org/guiaonline/intermediario/)

#### Vídeos

Pesquisar no YouTube:

* "estrutura diretórios linux"
* "arquivos configuração linux"

### Palavras-chave para pesquisa

* linux configuration files
* etc directory linux
* application configuration linux

### Checklist

* [ ] Arquivo localizado
* [ ] Finalidade compreendida
* [ ] Alteração realizada
* [ ] Alteração revertida
* [ ] Documentação criada

---

## Atividade 4 — Explorando Logs

Nível: Intermediário

### Tempo esperado

| Etapa     |       Tempo |
| --------- | ----------: |
| Leitura   |      20 min |
| Vídeos    |      30 min |
| Prática   |        1h30 |
| **Total** | **2h20min** |

### Enunciado

Investigue os logs relacionados ao serviço escolhido anteriormente.

Seu objetivo é descobrir:

* onde os logs estão armazenados
* quais informações são registradas
* como localizar eventos específicos

Documente os resultados encontrados.

### Direção de estudo

* Logs
* Troubleshooting
* Journal

### Conteúdo sugerido

#### Leitura

* [https://docs.rockylinux.org/books/admin_guide/09-logs/](https://docs.rockylinux.org/books/admin_guide/09-logs/)

#### Vídeos

Pesquisar no YouTube:

* "journalctl linux"
* "logs linux systemd"

### Palavras-chave para pesquisa

* journalctl tutorial
* linux logs
* systemd journal

### Checklist

* [ ] Logs localizados
* [ ] Eventos identificados
* [ ] Evidências registradas
* [ ] Consegue explicar a utilidade dos logs

---

## Atividade 5 — Shell Script Aplicado

Nível: Intermediário

### Tempo esperado

| Etapa           |       Tempo |
| --------------- | ----------: |
| Planejamento    |      20 min |
| Desenvolvimento |        1h30 |
| Testes          |          1h |
| **Total**       | **2h50min** |

### Enunciado

Crie um script que realize uma verificação simples de um serviço do sistema.

O script deve:

* verificar se o serviço está em execução
* registrar o resultado em um arquivo de log
* informar ao usuário o resultado da verificação

O objetivo é aplicar os conceitos de shell script aprendidos até o momento em uma tarefa administrativa real.

### Direção de estudo

* Variáveis
* Condicionais
* Logs

### Conteúdo sugerido

Revisar os materiais das quinzenas anteriores.

### Palavras-chave para pesquisa

* shell script service status
* bash if command success
* systemctl shell script

### Checklist

* [ ] Script criado
* [ ] Script executado
* [ ] Log gerado
* [ ] Resultado correto

---

## Entregáveis da quinzena

Você deve apresentar:

* [ ] Documentação da instalação da aplicação
* [ ] Relatório do serviço analisado
* [ ] Relatório dos arquivos de configuração
* [ ] Relatório dos logs
* [ ] Script de verificação
* [ ] Resumo da quinzena

### Resumo obrigatório

Escreva um texto curto (máximo 15 linhas) explicando:

* o que é um pacote
* o que é um repositório
* o que é um serviço
* o que é um arquivo de configuração
* o que é um log
* quais dúvidas ainda permanecem

---

Essa quinzena foi desenhada especificamente para preparar a próxima etapa: **instalação e operação básica do PostgreSQL no Rocky Linux**, sem introduzir conceitos de banco de dados ainda. O foco será entender PostgreSQL primeiro como uma aplicação Linux (pacote, serviço, configuração e logs) e só depois como um SGBD.


ATIVIDADE 1 — INSTALANDO MINHA PRIMEIRA APLICAÇÃO

1 PRIMEIRA TENTATIVA DE INSTALAÇÃO DEU ERRO POIS A DATA E A HORA DO SISTEMA NÃO ESTAVAM SINCRONIZADAS E CORRETAS. APÓS O AJUSTE, CONSEGUI FAZER A INSTALAÇÃO DO PRIMEIRO PACOTE.
<img width="1280" height="826" alt="image" src="https://github.com/user-attachments/assets/e2fa6984-6c83-4193-a75d-60f8fd88a2b8" />

2 PRIMEIRO PACOTE INSTALADO
<img width="1280" height="791" alt="image" src="https://github.com/user-attachments/assets/ec96471b-a7f1-49e5-a099-777ee3541f8f" />

3 VERIFICADO INFORMAÇÕES DO PROGRAMA INSTALADO
<img width="1280" height="778" alt="image" src="https://github.com/user-attachments/assets/85bae3b1-f57d-4268-8a56-7d1ddba8efdb" />

4 VERIFICANDO SE O PACOTE ESTÁ INSTALADO
<img width="1280" height="846" alt="image" src="https://github.com/user-attachments/assets/d3ab5934-85f6-46b0-bc40-124b562fc53c" />

5 REMOVENDO O PROGRAMA QUE FOI INSTALADO
<img width="1280" height="846" alt="image" src="https://github.com/user-attachments/assets/b8303637-9cf8-461c-a219-a4f94b38d797" />

6 VERIFICANDO SE O PROGRAMA FOI REMOVIDO
<img width="1280" height="809" alt="image" src="https://github.com/user-attachments/assets/ce9e4d12-0705-455f-a7c2-df731e73df64" />

7 REINSTALANDO O PACOTE
<img width="1280" height="809" alt="image" src="https://github.com/user-attachments/assets/f56ed2f3-9996-47d9-b9dc-259461a59f95" />








