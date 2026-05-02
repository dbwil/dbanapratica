
Administração de arquivos e diretórios

- ls =Lista arquivos e diretórios
- ls -l = Lista detalhada
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
