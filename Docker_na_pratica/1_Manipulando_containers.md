# Comandos Básicos do Docker

### `docker run hello-world`
Baixa a imagem oficial `hello-world` (caso ainda não exista localmente) e executa um container a partir dela. Este container exibe uma mensagem de confirmação do ambiente Docker e finaliza a execução.

### `docker ps`
Lista todos os containers que estão atualmente em **execução** no sistema, exibindo informações como ID, imagem, comando de entrada, tempo de criação, status e portas.

### `docker ps -a`
Lista **todos** os containers do sistema, incluindo os que estão rodando e os que já foram finalizados (*Exited*).

### `docker run --name mycontainer hello-world`
Cria e executa um container a partir da imagem `hello-world`, atribuindo a ele o nome customizado `mycontainer` para facilitar sua identificação e gerenciamento em vez de usar um nome aleatório gerado pelo Docker.

### `docker run --help | grep name`
Exibe a documentação de ajuda do comando `docker run` e filtra a saída (usando o `grep`) para mostrar apenas as linhas que contêm a palavra "name" (geralmente usada para buscar a sintaxe da flag `--name`).

### `docker run --name mycontainer2 hello-world /xpto`
Tenta executar um container apontando para um executável `/xpto` que não existe dentro da imagem `hello-world`. O container falha no início da execução e retorna um erro de arquivo/diretório não encontrado.

### `docker run --name mycontainer3 hello-world /hello`
Executa o container nomeado `mycontainer3` sobrescrevendo o comando padrão de inicialização pelo executável `/hello` interno da imagem `hello-world`, executando a aplicação com sucesso.

### `docker rm <id_container>` / `docker rm <nome_container>`
Remove um container do sistema utilizando seu ID ou nome. Para que este comando funcione, o container **deve estar parado ou finalizado** (status *Exited*); não é possível remover um container enquanto ele estiver em execução.

### `docker run --name mynginx nginx`
Baixa a imagem `nginx` (se necessário) e inicia um container chamado `mynginx` em primeiro plano (*foreground*), prendendo o terminal no log do servidor web.

### `docker stop mynginx`
Envia um sinal (*SIGTERM*) para interromper de forma graciosa o container `mynginx`. É a maneira recomendada para parar um container, permitindo que a aplicação salve estados e encerre processos com segurança antes de desligar.

### `docker start mynginx`
Reinicia um container existente que estava parado (`mynginx`), preservando as alterações e configurações feitas no container antes da interrupção.

### `docker rm -f mynginx`
Força a remoção imediata de um container (`-f` / `--force`), mesmo que ele ainda esteja em execução. O Docker envia um sinal de interrupção abrupta (*SIGKILL*) e apaga o container imediatamente.

### `docker rm -f <inicio_id>`
Remove um container forçadamente utilizando apenas os primeiros caracteres do seu ID (ex: `docker rm -f a1b`). O Docker faz a busca e, se o trecho fornecido for único e não ambíguo, localiza e apaga o container correto sem precisar do ID completo.

### `docker run -d nginx`
Executa o container em modo *detached* (segundo plano/background). O Docker inicia o serviço `nginx`, libera o terminal imediatamente e retorna apenas o ID longo do container gerado.

### `docker attach <id_container>`
Anexa o terminal do host aos fluxos de entrada, saída e erro (*stdin*, *stdout*, *stderr*) de um container que já está em execução em background, permitindo visualizar os logs em tempo real ou interagir diretamente com ele.

### `docker exec <id_container> ls`
Executa o comando `ls` dentro de um container que já está rodando, listando os arquivos e diretórios do diretório de trabalho padrão sem precisar entrar interativamente no container.

### `docker exec <id_container> ls -la`
Executa o comando `ls -la` dentro do container em execução, listando todos os arquivos e diretórios (incluindo ocultos) com detalhes de permissões, proprietário, tamanho e data de modificação.

### `docker exec -it <id_container> bash`
Abre um terminal interativo (`bash`) dentro de um container em execução. A combinação das flags `-i` (*interactive*, mantém o stdin aberto) e `-t` (*tty*, aloca um terminal pseudo-TTY) permite navegar e executar comandos diretamente no Shell do container.

### `docker run --rm nginx`
Cria e executa um container a partir da imagem `nginx` e adiciona a flag `--rm`, que garante a remoção automática do container e do seu sistema de arquivos no momento em que ele for parado ou finalizado.

### `docker ps -aq`
Lista apenas (`-q` / *quiet*) os IDs numéricos de todos (`-a` / *all*) os containers presentes no sistema, sejam eles ativos ou parados. Retorna uma lista limpa contendo apenas as hashes dos containers, sem cabeçalhos ou colunas extras.

### `docker rm -f $(docker ps -aq)`
Remove forçadamente (`-f`) todos os containers do sistema de uma só vez. O comando utiliza a substituição de Shell `$(...)` para passar a lista de IDs retornada pelo `docker ps -aq` como argumento para o `docker rm -f`.