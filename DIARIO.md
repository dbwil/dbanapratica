
QUINZENA 8
1 -No dia 18/07/2026 no momento da atulização dos pacotes o que eu fiz quando o terminal ficou preso:
Quando coloquei o sistema para baixar a atualização de 1.3 GB, a tela do meu terminal ficou totalmente ocupada mostrando o progresso do download, e eu não conseguia digitar mais nenhum comando ali.

A solução: Para não interromper o download e poder continuar trabalhando, eu abri uma nova aba no terminal usando o atalho Ctrl + Shift + T. Com isso, o Linux continuou baixando o Kernel e os arquivos em segundo plano de forma segura, enquanto eu ganhei uma tela livre para continuar minhas tarefas sem travar meu fluxo.


2 - Tentei instalar o htop e deu ruim (mas resolvi)
O que aconteceu:
Tentei instalar o monitor de sistema htop direto pelo terminal do Rocky Linux 9, mas o gerenciador de pacotes retornou o erro "Impossível de encontrar uma correspondência: htop". Isso aconteceu porque o htop não vem na lista de aplicativos básicos de fábrica do sistema.
O Rocky Linux de fábrica só vem com os repositórios básicos da Red Hat. Como o htop é uma ferramenta extra, o gerenciador de pacotes simplesmente não sabia onde achar o instalador.

Como resolvi o problema:
Descobri que para esse tipo de programa rodar em servidor corporativo, a gente precisa ativar uma "loja de pacotes" da comunidade chamada EPEL (Extra Packages for Enterprise Linux).

Para resolver isso, entendi que precisava ativar um repositório extra de programas no Linux. Usei o operador && para juntar dois comandos em uma única linha e disparar a solução:

sudo yum install epel-release && sudo yum install htop

Com essa instrução, eu primeiro instalei o repositório EPEL (que funciona como uma loja expandida de ferramentas para servidores corporativos) e, logo em seguida, o sistema passou a reconhecer e instalar o htop com sucesso a partir dessa nova fonte.
