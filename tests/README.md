# Tests

## Environment variables

1. Set these environment variable values:

    ```console
    export GIT_ACCOUNT=senzing
    export GIT_REPOSITORY=stream-producer
    export GIT_ACCOUNT_DIR=~/${GIT_ACCOUNT}.git
    export GIT_REPOSITORY_DIR="${GIT_ACCOUNT_DIR}/${GIT_REPOSITORY}"
    ```

## Test CLI

### Test input formats to STDOUT

1. Test file-based input.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}

    ./stream-producer.py json-to-stdout    --input-url tests/simple/simple.json       2>/dev/null
    ./stream-producer.py csv-to-stdout     --input-url tests/simple/simple.csv        2>/dev/null
    ./stream-producer.py avro-to-stdout    --input-url tests/simple/twitter.avro      2>/dev/null
    ./stream-producer.py parquet-to-stdout --input-url tests/simple/userdata1.parquet 2>/dev/null
    ```

1. Test URL-based input.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}

    ./stream-producer.py json-to-stdout \
      --input-url "http://senzing.dockter.com/files/stream-producer/simple.json" \
      2>/dev/null

    ./stream-producer.py csv-to-stdout \
      --input-url "http://senzing.dockter.com/files/stream-producer/simple.csv" \
      2>/dev/null

    ./stream-producer.py avro-to-stdout \
      --input-url "http://senzing.dockter.com/files/stream-producer/twitter.avro" \
      2>/dev/null

    ./stream-producer.py parquet-to-stdout \
      --input-url "http://senzing.dockter.com/files/stream-producer/userdata1.parquet" \
       2>/dev/null
    ```

### Test limiters

1. Test file-based input.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}

    ./stream-producer.py json-to-stdout \
      --input-url tests/simple/simple.json \
       2>/dev/null

    ./stream-producer.py json-to-stdout \
      --input-url tests/simple/simple.json \
      --record-min 40 \
       2>/dev/null

    ./stream-producer.py json-to-stdout \
      --input-url tests/simple/simple.json \
      --record-max 10 \
       2>/dev/null

    ./stream-producer.py json-to-stdout \
      --input-url tests/simple/simple.json \
      --record-min 10 \
      --record-max 20 \
       2>/dev/null

    ./stream-producer.py json-to-stdout \
      --input-url "https://s3.amazonaws.com/public-read-access/TestDataSets/loadtest-dataset-1M.json" \
      --record-max 10 \
       2>/dev/null

    ```

## Test Docker

1. Test JSON.
   Example:

    ```console
    docker run \
      --env SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/simple.json" \
      --env SENZING_SUBCOMMAND=json-to-stdout \
      --interactive \
      --rm \
      --tty \
      senzing/stream-producer:latest
    ```

1. Test CSV.
   Example:

    ```console
    docker run \
      --env SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/simple.csv" \
      --env SENZING_SUBCOMMAND=csv-to-stdout \
      --interactive \
      --rm \
      --tty \
      senzing/stream-producer:latest
    ```

1. Test Parquet.
   Example:

    ```console
    docker run \
      --env SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/userdata1.parquet" \
      --env SENZING_SUBCOMMAND=parquet-to-stdout \
      --interactive \
      --rm \
      --tty \
      senzing/stream-producer:latest
    ```

1. Test Avro.
   Example:

    ```console
    docker run \
      --env SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/twitter.avro" \
      --env SENZING_SUBCOMMAND=avro-to-stdout \
      --interactive \
      --rm \
      --tty \
      senzing/stream-producer:latest
    ```

## Test Docker-compose

### Build docker image

1. Make Docker image.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}
    make docker-build
    ```

### Choose input

1. :thinking: Choose one of the following sets of [`SENZING_INPUT_FORMAT`,`SENZING_INPUT_URL`] to test.
    1. JSON

        ```console
        export SENZING_INPUT_FORMAT=json
        export SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/simple.json"
        ```

    1. CSV

        ```console
        export SENZING_INPUT_FORMAT=csv
        export SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/simple.csv"
        ```

    1. Avro

        ```console
        export SENZING_INPUT_FORMAT=avro
        export SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/twitter.avro"
        ```

    1. Parquet

        ```console
        export SENZING_INPUT_FORMAT=parquet
        export SENZING_INPUT_URL="http://senzing.dockter.com/files/stream-producer/userdata1.parquet"
        ```

### Choose docker-compose formation

1. :thinking: Choose one of the docker-compose formations to test.
    1. RabbitMQ

        ```console
        cd ${GIT_REPOSITORY_DIR}
        sudo \
          --preserve-env \
          docker-compose --file tests/simple/docker-compose-rabbitmq.yaml up
        ```

    1. Kafka

        ```console
        cd ${GIT_REPOSITORY_DIR}
        sudo \
          --preserve-env \
          docker-compose --file tests/simple/docker-compose-kafka.yaml up
        ```

### View results

1. :thinking: View results, depending on queue type.
    1. RabbitMQ
        1. RabbitMQ is viewable at
           [localhost:15672](http://localhost:15672).
            1. **Defaults:** username: `user` password: `bitnami`
    1. Kafka
        1. Kafdrop is viewable at
           [localhost:9179](http://localhost:9179).

### Cleanup

1. :thinking: Bring down docker formation.
   Choose one of the following, depending one queue type.

    1. RabbitMQ

        ```console
        cd ${GIT_REPOSITORY_DIR}
        sudo docker-compose --file tests/simple/docker-compose-rabbitmq.yaml down
        ```

    1. Kafka

        ```console
        cd ${GIT_REPOSITORY_DIR}
        sudo docker-compose --file tests/simple/docker-compose-kafka.yaml down
        ```

## Test RabbitMQ 100K

1. Run docker-compose test.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo \
      --preserve-env \
      docker-compose --file tests/simple/docker-compose-rabbitmq-100k.yaml up
    ```

1. RabbitMQ is viewable at
   [localhost:15672](http://localhost:15672).
    1. **Defaults:** username: `user` password: `bitnami`

1. Bring down docker formation.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo docker-compose --file tests/simple/docker-compose-rabbitmq-100k.yaml down
    ```

## Test Kafka 100K

1. Run docker-compose test.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo \
      --preserve-env \
      docker-compose --file tests/simple/docker-compose-kafka-100k.yaml up
    ```

1. Kafdrop is viewable at
   [localhost:9179](http://localhost:9179).

1. Bring down docker formation.
   Example:

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo docker-compose --file tests/simple/docker-compose-kafka-100k.yaml down
    ```
