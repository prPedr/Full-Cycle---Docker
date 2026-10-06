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