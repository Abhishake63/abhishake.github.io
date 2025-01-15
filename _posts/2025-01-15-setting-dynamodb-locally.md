---
layout: post
title: Setting DynamoDB Locally & Configuring AWS CLI
subtitle: A Guide to Local Setup of DynamoDB & AWS CLI Configuration
gh-repo: Abhishake63/abhishake-guides
gh-badge: [star, fork, follow]
tags: [database, dynamodb]
author: Abhishake Sen Gupta
---

## Using DynamoDB Locally

- [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)
- [LocalStack](https://docs.localstack.cloud/overview/)

### Localstack with Docker

```docker
services:
  localstack:
    container_name: "${LOCALSTACK_DOCKER_NAME:-localstack-main}"
    image: localstack/localstack
    ports:
      - "127.0.0.1:4566:4566"            # LocalStack Gateway
      - "127.0.0.1:4510-4559:4510-4559"  # external services port range
    environment:
      - DEBUG=${DEBUG:-0}
```

### DynamoDB Local with Docker

```docker
services:
  dynamodb-local:
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
```

User should have proper permission on the directory, otherwise `dynamodb-admin` won't be able to read data and throw error.

```bash
sudo chmod 777 ./docker/dynamodb
```

If you want data persistence without any fancy workaround, you should just go with the `dynamodb-local` image provided by AWS, as Localstack doesn't provide data persistence in their free image, we need to use `localstack-pro` image for that. You can also use communtity edition image of localstack `gresau/localstack-persist` for data persistence.

## Using AWS Cli / awslocal

Follow the instructions [here](https://docs.localstack.cloud/user-guide/integrations/aws-cli/#localstack-aws-cli-awslocal) to use the cli or the wrapper.

### AWS Cli

You can setup your config & credentials by typing `aws configure` command, it will add your creds and config under `~/.aws/` folder in separate files named `config` & `credentials` in `default` profile. If you need to jump around different creds and config, you can leverage it using `profile` specific config & creds.

> ~/.aws/credentials
>

```bash
[default]
aws_access_key_id = key
aws_secret_access_key = key

[localstack]
aws_access_key_id = key
aws_secret_access_key = key

[dynamodblocal]
aws_access_key_id = key
aws_secret_access_key = key
```

> ~/.aws/config
>

```bash
[default]
region = us-east-1
output = json
services = dynamodblocal-services

[profile localstack]
services = localstack-services
region = us-east-1
output = json

[profile dynamodblocal]
services = dynamodblocal-services
region = us-east-1
output = json

[services localstack-services]
dynamodb =
  endpoint_url = http://localhost:4566

[services dynamodblocal-services]
dynamodb =
  endpoint_url = http://localhost:8000
```

Now you can use commands with aws cli without mentioning endpoint-url & jump around different profiles

```bash
aws dynamodb list-tables --profile dynamodblocal
aws dynamodb list-tables --profile localstack
```

If you don't mention any profile, it will use the config & creds for default profile.

## Using DynamoDB Admin

Use the below yml & start the service with `docker compose up` command

```docker
services:
  dynamodb-admin:
    depends_on:
      - localstack
    image: aaronshaf/dynamodb-admin
    ports:
      - "8001:8001"
    environment:
      - DYNAMO_ENDPOINT=http://localstack:4566
      - AWS_REGION=us-east-1
```

Now, you can use the UI on [here](http://localhost:8001)