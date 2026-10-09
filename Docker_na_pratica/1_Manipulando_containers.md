# Resumo do Capítulo: Manipulando Containers

Este documento consolida os conceitos fundamentais, comandos e boas práticas para a manipulação e gerenciamento de containers no Docker.

---

## 1. Executando o Primeiro Container

Para verificar se o ambiente Docker está instalado e configurado corretamente, utiliza-se o container de teste oficial:

```bash
docker run hello-world
```

> **Funcionamento:** Este comando verifica se a imagem `hello-world` existe localmente. Caso contrário, faz o download automático (*pull*) do Docker Hub, cria e executa o container, exibindo uma mensagem de confirmação no terminal antes de finalizar.

---

## 2. Nomeando Containers e Modos de Execução

### Executando um Container com Nome Personalizado (`--name`)
Por padrão, o Docker atribui nomes aleatórios aos containers. Para facilitar a identificação e manipulação, utilize a flag `--name`:

```bash
docker run --name mynginx nginx
```

### Executando em Segundo Plano (`-d`)
Para liberar o terminal e executar o container em background (*detached mode*), utilize a flag `-d`:

```bash
docker run -d --name mynginx nginx
```

### Mapeando Portas (`-p`)
Permite mapear uma porta da máquina hospedeira (*host*) para uma porta interna do container no formato `HOST:CONTAINER`:

```bash
docker run -d -p 8080:80 nginx
```
> **Resultado:** A aplicação Nginx rodando na porta `80` do container fica acessível externamente via `http://localhost:8080`.

---

## 3. Gerenciamento do Ciclo de Vida do Container

### Listando Containers
- **Apenas em execução:**
  ```bash
  docker ps
  ```
- **Todos os containers (ativos e parados):**
  ```bash
  docker ps -a
  ```

> [!NOTE]
> `docker ps` exibe apenas containers ativos. Para visualizar containers finalizados com status *Exited*, use `docker ps -a`.

### Parando e Iniciando Containers
- **Parar um container ativo (desligamento gracioso):**
  ```bash
  docker stop mynginx
  ```
- **Reiniciar um container parado:**
  ```bash
  docker start mynginx
  ```

### Removendo Containers
- **Remoção padrão (apenas containers parados):**
  ```bash
  docker rm mynginx
  ```
- **Remoção forçada (containers em execução):**
  ```bash
  docker rm -f mynginx
  ```

---

## 4. Conexão ao Terminal (Attach vs. Detach)

### Conectando-se ao Processo Principal (`docker attach`)
Permite conectar o terminal local ao processo em execução dentro de um container rodando em background:

```bash
docker attach mynginx
```

> [!TIP]
> **Saindo sem encerrar o container:** Para se desconectar do container sem pará-lo, pressione a sequência de teclas: **`CTRL + P`** seguido de **`CTRL + Q`**.

---

## 5. Execução de Comandos e Remoção Automática

### Executando Comandos em um Novo Container
É possível disparar um comando isolado direto na criação do container:

```bash
docker run nginx ls -la
```

### Acessando o Shell Interativo
Para abrir uma sessão interativa no terminal do container (`-i` interativo, `-t` pseudo-TTY):

```bash
docker run -it nginx bash
```

### Diferença Fundamental: `docker run` vs `docker exec`
* **`docker run`**: Cria e inicia um **novo** container.
* **`docker exec`**: Executa um novo processo dentro de um container que **já está em execução**.

```bash
# Exemplo com docker exec
docker exec -it mynginx bash
```

### Remoção Automática (`--rm`)
Remove o container e seu sistema de arquivos descartável automaticamente assim que a execução do processo for finalizada:

```bash
docker run --rm nginx ls -la
```

---

## 6. Remoção em Massa de Containers

Utilizando a substituição de comandos do Shell `$(...)` com a flag `-q` (*quiet*, retorna apenas os IDs):

* **Remover todos os containers parados:**
  ```bash
  docker rm $(docker ps -a -q)
  ```

* **Remover TODOS os containers (inclusive em execução):**
  ```bash
  docker rm -f $(docker ps -a -q)
  ```

---

## 7. Diferenças Principais de Conceitos

### `docker exec` vs `docker attach`
* **`docker exec`**: Cria um **novo processo separado** no container (ex: abrir uma nova sessão bash para inspeção).
* **`docker attach`**: Conecta seu terminal diretamente ao **processo principal (PID 1)** do container (ex: visualizar logs em tempo real).

---

## Tabela Resumo de Comandos

| Categoria | Comando | Descrição |
| :--- | :--- | :--- |
| **Criação / Execução** | `docker run <imagem>` | Cria e inicia um container. |
| **Segundo Plano** | `docker run -d <imagem>` | Executa o container em background. |
| **Mapeamento de Porta**| `docker run -p 8080:80 <imagem>` | Mapeia porta Host:Container. |
| **Listagem** | `docker ps` | Lista containers ativos. |
| **Listagem Geral** | `docker ps -a` | Lista todos os containers (ativos e parados). |
| **Controle** | `docker stop <nome\|id>` | Interrompe a execução do container. |
| **Controle** | `docker start <nome\|id>` | Inicia um container parado. |
| **Remoção Simples** | `docker rm <nome\|id>` | Remove um container parado. |
| **Remoção Forçada** | `docker rm -f <nome\|id>` | Remove um container mesmo em execução. |
| **Interatividade** | `docker exec -it <nome\|id> bash` | Abre um Shell dentro de um container ativo. |
| **Limpeza Geral** | `docker rm -f $(docker ps -a -q)` | Apaga todos os containers do sistema. |