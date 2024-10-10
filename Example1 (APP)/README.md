# Como acessar o Linux que esta rodando em Docker

Para entrar no ambiente Ubuntu do seu setup Docker Compose, você pode usar o seguinte comando para obter um shell interativo dentro do contêiner que está executando o serviço `app`:

```bash
docker-compose exec app /bin/bash
```

Este comando assume que o serviço `app` executa um contêiner baseado em Ubuntu. Ele abre um shell Bash interativo dentro do contêiner, permitindo que você interaja com o ambiente Ubuntu.

Aqui está uma explicação do comando:
- `docker-compose exec`: Executa um comando dentro de um serviço em execução.
- `app`: O nome do serviço definido no seu `docker-compose.yml`.
- `/bin/bash`: O comando que você deseja executar dentro do contêiner, que neste caso é o shell Bash.

Se o seu serviço for baseado em outro shell ou sistema operacional, você pode precisar ajustar o shell (`/bin/bash`) de acordo (por exemplo, você pode usar `/bin/sh` se o Bash não estiver disponível).

Além disso, certifique-se de que o serviço está em execução ao iniciar o Docker Compose com:

```bash
docker-compose up -d
```