# dbanapratica
Série de atividades práticas de um DBA 
Horas acumuladas até 25/04: 4h:30m ⏰
---

### Quinzena 1 - 29/03 a 11/04 - Dedicados 1:30h

**#)** Nome da tarefa - nível - previsão de tempo

- [x] Atualizar o perfil no github - fácil - previsão 1h
- [x] Instalar o virtualbox no Windows - fácil - 1h
- [x] Criar ao menos 1 máquina virtual (VM) - fácil - 1h
- [x] Instalar o Ubuntu Linux Desktop 24.04LTS - médio - 1h
- [x] Fazer o resumo
 
**Tarefas no VirtualBox - fácil - 2h**
- [x] Como clonar uma máquina virtual (VM)
- [x] Como criar snapshots da VM
- [x] Como restaurar snapshots da VM


**Resumo:**

1 Como realizar atualização ou criação de um perfil no Github:
Primeiro passo acessar o link do site: www.github.com⁠. Ao acessar, no canto direito da página encontramos o link para criação de conta. Após isso, criamos um login e senha e atualizamos as informações do perfil.

2 Como instalar o Virtual Box no Windows:
Primeiro passo baixar o programa no site: https://www.oracle.com/br/virtualization/technologies/vm/downloads/virtualbox-downloads.html⁠, escolher a versão que estou usando no computador, no meu caso Windows, após isso instalar o programa.

3 Criando a minha primeira máquina virtual:
Primeiro passo baixar a ISO do Linux Desktop 24.04 LTS no site: https://ubuntu.com/download/alternative-downloads⁠
Após baixar a imagem ISO, no Virtual Box vamos até a aba máquinas e vamos clicar no ícone novo, damos um nome à nossa máquina virtual e após selecionamos o local onde se encontra a imagem ISO. Após isso, escolhemos a quantidade de memória, quantos núcleos e fazemos a parte de configuração.
Concluindo esta parte, criamos um nome de usuário e senha. Ao final aparece um sumário de toda configuração, clicamos em finalizar e após isso iniciamos a nossa máquina virtual. Nesse momento começa a instalação do sistema operacional e após a conclusão nossa máquina está ativa para começar a usar.

4 Clonando uma máquina Virtual:
Primeiro passo seleciono a máquina virtual que desejo clonar, neste caso usei a VM1 Ubuntu Desktop. Após isso clicamos com o botão direito sobre ela e logo em seguida abre uma aba onde selecionamos a opção clonar. Ao clicar sobre o ícone abrirá uma janela onde podemos nomear o clone, após isso finalizamos e o clone é criado.

5 Como criar um Snapshot de uma máquina Virtual:
Primeiro passo seleciono a máquina que desejo fazer o snapshot, no canto direito no alto da tela clico no menu snapshot, onde aparece estado atual. Eu vou clicar com o botão direito do meu mouse, onde dará a opção de criar snapshot. Após isso dou nome ao meu snapshot e clico em OK.

6 Como fazer a restauração do Snapshot:
Para restaurar ao estado anterior preciso que a minha máquina esteja desligada. Eu clico na instalação anterior que desejo restaurar e, a partir deste momento, acontece a restauração.

--- 

### Quinzena 2 - 11/04 a 25/04 - Dedicados 1:30h

**#)** Nome da tarefa - nível - previsão de tempo

- [x] Aprender sobre o Linux FHS https://www.youtube.com/watch?v=aq2f6Gtftyc&t=1s
- [x] Aprender sobre os comandos básicos do Linux - https://youtu.be/QG9CeHoQUc0?t=5
- [x] Aprender sobre gereciamento de pacotes DEB (Ubuntu) - https://www.youtube.com/watch?v=BCuDQQBVw00
- [x] Aprender sobre gereciamento de pacotes RPM (Rocky Linux) - https://www.youtube.com/watch?v=HOOmkx3fmcQ
- [x] Instalar o Ubuntu Server 24.04LTS (manter uma VM com o Ubuntu Desktop)
- [x] Instalar o Rocky Linux 9.x
- [x] Fazer o resumo

**Resumo:**

Resumo sobre a segunda quinzena DBA na prática:
O primeiro vídeo foi com o professor Kretcheu e fala sobre FHS, onde pude aprender um pouco sobre a origem do padrão Linux, pude ter uma ideia geral e aprender um pouco sobre os principais diretórios, exemplo: o diretório / é o diretório raiz e todos os demais diretórios são filhos deste diretório. Já o diretório /proc é onde estão os processos, entre outros. Hoje no Unix, no padrão FHS, cada coisa tem seu lugar definido.

O segundo vídeo fala sobre os comandos básicos no Linux. Nesse vídeo pude conhecer em torno de 60 comandos essenciais para utilizar na linha de comando dos terminais.

Já no terceiro vídeo pude aprender um pouco sobre como instalar aplicações .deb no Linux. É um formato de arquivo utilizado para instalar programas em distribuições baseadas em Debian.

O quarto vídeo fala sobre o gerenciamento de pacotes. Pude ter um pouco de noção sobre YUM e RPM. Por exemplo, o RPM permite instalar pacotes de distribuições baseadas em Red Hat diretamente no servidor.
Exemplo de comando após baixar um pacote:
rpm -ivh pacote.rpm, com o YUM eu consigo fazer a instalação diretamente de um repositório Linux. Neste caso, eu não preciso ir até um site e fazer um download, posso apenas dar o seguinte comando:
yum install (nomedopacote)
e ele já faz o download direto do repositório e realiza a instalação.
Repositório nada mais é que um servidor com todos os pacotes da distribuição.
Instalando o Ubuntu Server 24.04 LTS
Para isso é necessário baixar a imagem ISO no site: https://ubuntu.com/download/alternative-downloads⁠
Após isso, no Virtual Box clique em Novo para criar a máquina Ubuntu Server, dou um nome para ela, selecionamos a imagem ISO, faço as configurações padrão e após isso clicamos em finalizar.
Criada a máquina, inicializo ela para concluir a instalação. Após a instalação entro com meu login e senha para verificar o funcionamento correto da máquina.

Para a criação da máquina Rocky Linux, entro no site: https://rockylinux.org/download⁠
Faço o download da imagem ISO da versão 9.7. Após ter baixado, abro o Virtual Box na aba Novo e clico para criar uma nova VM. Dou nome à minha máquina, neste caso Rocky-9.7, seleciono a pasta com a imagem ISO que baixei no site do Rocky Linux.

--- 

### Quinzena 3 - 26/04 a 02/05 - Dedicados 1:30h

OBS: Antecipamos o próximo encontro devido a compromissos de ambos entre 04/05 e 18/05. Próximo encontro em 23/05.

**#)** Nome da tarefa - nível - previsão de tempo

- [ ] Assistir e tomar notas sobre sobre o Linux FHS https://www.youtube.com/watch?v=aq2f6Gtftyc&t=1s
- [ ] Assistir e praticar sobre os comandos básicos do Linux - https://youtu.be/QG9CeHoQUc0?t=5
- [ ] Criar o arquivo COMANDOS.md e documentar os comandos organizando-os por contexto, exemplos:
  - [ ] Administração de arquivos e diretórios
  - [ ] Administração de usuários e senhas
  - [ ] Administração de permissões
  - [ ] Gerenciamento de processos
- [ ] Fazer o resumo

**Resumo:**

Resumo sobre a terceira quinzena DBA na prática:
