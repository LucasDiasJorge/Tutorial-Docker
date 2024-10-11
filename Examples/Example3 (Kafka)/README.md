# Docker Compose para Kafka e Zookeeper

Este repositório contém a configuração de Docker Compose para rodar o Apache Kafka e Zookeeper em contêineres. O Kafka é configurado para comunicação com um broker e possui um listener público configurável para acessar externamente.

## Pré-requisitos

- Docker e Docker Compose instalados
- Configurar o `<IP>` público na variável de ambiente `KAFKA_ADVERTISED_LISTENERS` do serviço Kafka

## Como Usar

1. Clone este repositório e navegue até o diretório:
   ```bash
   git clone <repositório>
   cd <diretório>
   ```

2. Substitua o `<IP>` no arquivo `docker-compose.yml` pelo IP público ou hostname do servidor.

3. Inicie o Docker Compose:
   ```bash
   docker-compose up -d
   ```

4. Verifique se os contêineres estão em execução:
   ```bash
   docker-compose ps
   ```

5. Para visualizar os logs dos serviços:
   ```bash
   docker-compose logs -f kafka
   docker-compose logs -f zookeeper
   ```

## Configurações

- **Zookeeper**:
  - Porta: `2181`
  - Limite de memória: `200m`

- **Kafka**:
  - Porta interna: `29092`
  - Porta externa: `9092`
  - Limite de memória: `1000m`
  - Comunicação com o Zookeeper na porta `2181`
  
### Variáveis de Ambiente

- `KAFKA_ADVERTISED_LISTENERS`: Define os listeners para o Kafka. Configure o IP público para expor externamente.
- `KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR`: Define o fator de replicação dos tópicos de offsets. Para um único broker, o valor recomendado é `1`.

## Como Decidir Quanto de Memória Alocar

Para ajustar a memória dos serviços de acordo com o uso, você pode utilizar o comando `docker stats` para monitorar o consumo de recursos dos contêineres em tempo real. 

### Usando `docker stats`

Execute o seguinte comando para visualizar o uso de recursos:

```bash
docker stats
```

Isso exibirá uma tabela com as seguintes informações:
- Nome do contêiner
- Uso de CPU
- Uso de Memória / Limite de Memória
- % de Memória em uso
- Rede de entrada/saída
- I/O do disco

### Análise de Uso de Memória

1. **Acompanhe o uso de memória** dos contêineres Kafka e Zookeeper. Observe se algum deles atinge frequentemente o limite definido.

2. **Ajuste o limite de memória** no arquivo `docker-compose.yml` de acordo com o uso observado:
   - Se o uso de memória estiver constantemente próximo ao limite, considere aumentá-lo.
   - Se o uso de memória estiver significativamente abaixo do limite, você pode reduzir o valor para economizar recursos.

3. **Exemplo de ajuste**:
   - Se o Kafka está usando consistentemente cerca de 800 MB, mas com picos de até 950 MB, considere aumentar o limite para algo em torno de `1200m` para acomodar esses picos.

4. **Atualize o Docker Compose** com as novas alocações de memória:
   ```bash
   docker-compose down
   docker-compose up -d
   ```

## Encerramento

Para encerrar e remover os contêineres, execute:

```bash
docker-compose down
```