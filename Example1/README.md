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

# Docker Compose

Este projeto demonstra como usar o Docker Compose para gerenciar e provisionar contêineres de forma eficiente. Abaixo está uma explicação dos principais comandos do Docker Compose, estrutura e exemplos de configuração.

## Comandos do Docker Compose
Aqui estão alguns dos principais comandos do Docker Compose usados para gerenciar contêineres:

- `docker-compose up`: Cria e inicia os contêineres.
- `docker-compose build`: Realiza o build das imagens que serão usadas.
- `docker-compose logs`: Exibe os logs dos contêineres.
- `docker-compose restart`: Reinicia os contêineres.
- `docker-compose ps`: Lista os contêineres em execução.
- `docker-compose scale`: Aumenta o número de réplicas de um contêiner.
- `docker-compose start`: Inicia os contêineres.
- `docker-compose stop`: Para a execução dos contêineres.
- `docker-compose down`: Para e remove todos os contêineres e seus componentes (rede, imagem, volume).

## Estrutura do Arquivo `docker-compose.yml`
O arquivo `docker-compose.yml` é fundamental para definir as configurações dos contêineres. O Compose busca, por padrão, um arquivo chamado `docker-compose.yml` no diretório corrente, mas é possível usar o parâmetro `-f` para especificar outro arquivo ou localização.

Exemplo de estrutura de um arquivo `docker-compose.yml`:

```yaml
version: '3.8'

networks:
  web_network:
    driver: overlay

volumes:
  site_conf:

services:
  web-server:
    image: httpd:latest
    ports: "80:80"
    deploy:
      placement:
        constraints:
          - "node.role==worker"
      mode: replicated
      replicas: 2
      resources:
        cpus: "0.50"
        memory: 256M
    restart: always
    networks:
      - web_network
    volumes:
      - /dir/site/html:/var/www/html
      - site_conf:/etc/httpd
```

### Conceitos Importantes:

1. **Serviços**: Cada bloco dentro de `services` define um serviço, ou seja, um conjunto de contêineres com a mesma configuração. Exemplo: o parâmetro `replicas` define quantas cópias idênticas do serviço serão criadas.

2. **Build e Image**: A opção `image` especifica qual imagem Docker será usada. Alternativamente, a opção `build` pode ser utilizada para criar a imagem a partir de um `Dockerfile`.

3. **Ports e Expose**: A opção `ports` mapeia uma porta do host para o contêiner, enquanto `expose` define quais portas estarão disponíveis internamente na rede dos contêineres.

4. **Deploy**: A seção `deploy` define como o serviço será executado, incluindo o número de réplicas e as limitações de recursos (CPU, memória).

5. **Restart e Restart Policy**: Com essas opções, é possível definir o comportamento de reinício dos contêineres. O parâmetro `restart` aceita valores como `always`, `on-failure`, e `unless-stopped`. Já o `restart_policy` (dentro de `deploy`) oferece mais controle sobre tentativas de reinício.

6. **Networks**: Define redes que podem ser usadas pelos serviços para se comunicarem.

7. **Volumes**: Define volumes para persistência de dados entre reinicializações dos contêineres.

8. **Environment e env_file**: Utilizados para definir variáveis de ambiente diretamente no arquivo ou em arquivos separados.

9. **Depends_on**: Cria dependências entre serviços, garantindo que um serviço só seja iniciado após outro estar ativo.

10. **Command e Entrypoint**: Substituem os comandos padrões definidos no Dockerfile da imagem usada, permitindo customizar o processo principal do contêiner.

## Instalação do Docker Compose
Algumas distribuições possuem o Docker Compose em seus repositórios, facilitando a instalação via gerenciador de pacotes. Contudo, a versão pode estar desatualizada. Para garantir a versão mais recente, é possível instalar manualmente seguindo as instruções no repositório oficial.

Certifique-se de ter o Docker instalado e permissão para executá-lo. Caso não tenha, consulte o post anterior sobre a instalação do Docker.