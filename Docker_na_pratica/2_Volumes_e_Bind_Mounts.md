### `docker run -d -p 8080:80 -v $(pwd)/src/my_nginx_html:/usr/share/nginx/html nginx`

Cria e executa um container Nginx apontando arquivos HTML locais diretamente para o servidor web usando **Bind Mount**.

#### Quebra dos parâmetros:

* **`docker run`**: Comando responsável por criar e iniciar um novo container a partir de uma imagem.
* **`-d`** (*detached*): Executa o container em segundo plano (background), liberando o terminal imediatamente.
* **`-p 8080:80`** (*port mapping*): Mapeia a porta `8080` do computador (host) para a porta `80` interna do container (`-p host:container`).
* **`-v $(pwd)/src/my_nginx_html:/usr/share/nginx/html`** (*volume / bind mount*):
  * **`$(pwd)/src/my_nginx_html`**: Caminho absoluto do diretório no seu computador (o `$(pwd)` pega o diretório atual do terminal).
  * **`:`**: Separador entre o caminho do host e o caminho do container.
  * **`/usr/share/nginx/html`**: Caminho interno do container onde o Nginx busca os arquivos estáticos para servir na web.
* **`nginx`**: Nome da imagem oficial utilizada para criar o container.

> [!TIP]
> **Efeito prático:** Qualquer alteração feita nos arquivos da pasta `src/my_nginx_html` na sua máquina local reflete **instantaneamente** no navegador ao acessar `http://localhost:8080`, sem necessidade de reiniciar o container ou reconstruir a imagem.