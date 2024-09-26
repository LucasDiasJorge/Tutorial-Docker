## Tutorial de Docker

Para este tutorial, vamos precisar de: o Docker instalado na sua máquina, e um terminal de sua preferência (neste caso, utilizaremos o VS Code com terminal integrado).

#### Primeiro passo: Instalação do Docker

Instalação:

<pre><code>sudo apt install docker # Ubuntu
sudo dnf install docker # Fedora
</code></pre>

Verifique o status do serviço do docker:

<pre><code>sudo systemctl status docker</code></pre>

O output deve ser algo como:

<pre><code>
    Output● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2020-05-19 17:00:41 UTC; 17s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 24321 (dockerd)
      Tasks: 8
     Memory: 46.4M
     CGroup: /system.slice/docker.service
             └─24321 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
</code></pre>

##### O que é o Docker?
Docker é uma ferramenta de contêineres que nos permite empacotar nossas aplicações e seus ambientes em contêineres, garantindo que a aplicação funcionará da mesma forma em qualquer máquina. Com Docker, conseguimos uma forma eficiente de executar, testar e distribuir software de maneira isolada e controlada.

#### Configurações iniciais do Docker
Assim que você instalar o Docker, abra o terminal e verifique se ele está funcionando corretamente com o comando:

<pre><code>docker --version</code></pre>

Se você receber a versão instalada como resposta, parabéns! O Docker está funcionando. Caso contrário, verifique se a instalação foi feita corretamente e se você já reiniciou sua máquina, pois pode ser necessário.

<pre><code>Docker version 27.2.0, build 3ab4256</code></pre>

#### Criando o primeiro container Docker
Para começar nosso primeiro contêiner Docker, vamos baixar uma imagem oficial da Docker Hub, que é o repositório de imagens do Docker.

No terminal, digite o seguinte comando para puxar a imagem do Ubuntu:

<pre><code>docker pull ubuntu</code></pre>

Isso irá baixar a imagem do Ubuntu mais recente. Agora, vamos rodar essa imagem e criar um contêiner interativo com ela:

<pre><code>docker run -it ubuntu</code></pre>

O comando `docker run` cria e roda o contêiner, e as flags `-it` permitem que você interaja com o contêiner no modo terminal.

Dentro do contêiner, você estará no ambiente Ubuntu, como se estivesse usando um sistema operacional novo e limpo!

#### Executando comandos dentro do container
Agora que estamos dentro do nosso contêiner, podemos executar comandos como se estivéssemos em qualquer terminal Linux. Por exemplo, tente atualizar o sistema:

<pre><code>apt-get update</code></pre>

E se quiser sair do contêiner, basta digitar `exit` no terminal.

#### Listando containers e imagens Docker
Podemos listar todos os contêineres que estão rodando ou que já rodaram em nossa máquina com o comando:

<pre><code>docker ps -a</code></pre>

Este comando mostra todos os contêineres, incluindo os que já foram finalizados. Para listar todas as imagens que baixamos, podemos usar:

<pre><code>docker images</code></pre>

Esses comandos são úteis para gerenciar nossos contêineres e imagens.

#### Criando um Dockerfile e buildando uma imagem
Agora, vamos criar nossa própria imagem Docker. Para isso, vamos criar um **Dockerfile**, que é um arquivo de configuração usado para construir uma imagem.

Crie um arquivo chamado `Dockerfile` e adicione o seguinte conteúdo:

```Dockerfile
# Usando uma imagem base do Ubuntu
FROM ubuntu:latest

# Instalação de dependências
RUN apt-get update && apt-get install -y curl

# Definindo o comando que será executado quando o contêiner iniciar
CMD ["echo", "Hello, Docker!"]
```

Agora, vamos construir nossa imagem com o seguinte comando:

<pre><code>docker build -t meu-primeiro-container .</code></pre>

Aqui, estamos dizendo ao Docker para construir uma imagem usando o `Dockerfile` presente no diretório atual (`.`) e nomeando a imagem como "meu-primeiro-container".

#### Rodando nossa imagem customizada
Depois de construirmos nossa imagem, podemos rodá-la com o comando:

<pre><code>docker run meu-primeiro-container</code></pre>

Se tudo correu bem, você verá a mensagem "Hello, Docker!" no terminal. Parabéns, você acabou de criar e rodar sua primeira imagem Docker!

#### Enviando sua imagem para o Docker Hub
Para compartilhar sua imagem com outras pessoas, você pode fazer o upload dela para o Docker Hub. Primeiro, faça login no Docker Hub:

<pre><code>docker login</code></pre>

Após logar, basta dar um `tag` na sua imagem e enviá-la para o repositório Docker Hub:

<pre><code>docker tag meu-primeiro-container seu-usuario/meu-primeiro-container</code></pre>

E em seguida, enviar com:

<pre><code>docker push seu-usuario/meu-primeiro-container</code></pre>

Agora, qualquer pessoa pode baixar sua imagem com o comando `docker pull`.

#### Conclusão
Neste tutorial, aprendemos como instalar o Docker, rodar contêineres, criar nossas próprias imagens e compartilhar no Docker Hub. Docker é uma ferramenta poderosa que facilita muito o desenvolvimento e a distribuição de software.

_[Reference](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)_
