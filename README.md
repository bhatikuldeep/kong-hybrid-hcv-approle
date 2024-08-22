# Kong Gateway Hybrid  Mode + HashiCorp Vault Approle for Reading File based Redis Secrets to use in Kong Rate Limit Advanced Plugin 


## Introduction

This guide explains how to set up Kong Gateway in hybrid mode using Docker Compose. The hybrid mode in Kong Gateway allows separation of the control plane and data plane, providing scalability and flexibility for large deployments. This setup includes using a 
- PostgreSQL database for Kong, 
- Hashicorp Vault setup, 
- Redis stack (including RedisInsight)
- declarative configuration with decK covers RLA plugin configuration.

## Prerequisites

- Docker and Docker Compose installed
- Make, curl or http installed


## Environment Configuration

Create a `.env` file in the root of your project directory with the following content:

```env
# .env
KONG_DOCKER_IMAGE=kong/kong-gateway:3.4.3.11-rhel
```

## Usage

### Starting the Services

To start the docker containers, run the following command:

```sh
make start
```
This command will:
- Start the PostgreSQL database.
- Run Kong migrations to set up the database schema.
- Start the Kong Control Plane and Data Plane services.
- Start HashiCorp Vault with `approle` pre-enabled, and `role_id` and `secret_id` created.
- Start Redis server with RedisInsight UI
- Start a httpbin local service


## Kong manager
In this setup, Kong Manager is exposed on port 8002. You can access it in your web browser by navigating to http://localhost:8002.

## HashiCorp Vault UI

The setup enables `approle` and a test secrets `myapp/config/username` and `myapp/config/password` created with values `approle` and `s3cr3t` respectively.

In this setup, HashiCorp Vault UI is exposed on port 8200. You can access it in your web browser by navigating to http://localhost:8200.

### Steps to unseal and login to Vault UI

- Open the Vault UI at - http://localhost:8200
- When you run the first time, the vault is sealed, so you must unseal to read the secrets. 
- To unseal, use the set of keys created here - `vault-cluster-vault.json` under `keys` proerty. 
- Once unseal, enter the token from the same file - `vault-cluster-vault.json` and use the value from `token` property. 
- You should be able to login now.
- Access this link - to see the secret - http://localhost:8200/ui/vault/secrets/secret/list 

## RedisInsight UI

The setup enables Redis stack, that installs both redis server and insight UI. 

In this setup, RedisInsight UI is exposed on port 8008. You can access it in your web browser by navigating to http://localhost:8008.

### Steps to login to RedisInsight UI

- Open the RedisInsight UI at - http://localhost:8008
- Enter `root` in the password input box, rest keep as is.
- To unseal, use the set of keys created here - `vault-cluster-vault.json` under `keys` proerty. 
- Once unseal, enter the token from the same file - `vault-cluster-vault.json` and use the value from `token` property. 
- You should be able to login now.
- Access this link - to see the secret - http://localhost:8200/ui/vault/secrets/secret/list 



## Stopping the Services

To stop the docker containers, run the following command:


```sh
make stop
```
- This command will stop and remove the containers defined in your Docker Compose file.

## Test the service
This docker compose runs  local httpbin container, check the `decks/kong.yaml` file to see the imported service in Kong gateway.

To test the Kong gateway service, run the following command:
`curl -X GET http://localhost:8000/anything` or `http :8000/anything`

Make sure to test and validate various RLA counters and check the same in RedisInsight UI matching RLA key.


## Conclusion
By following this guide, you will have a Kong Gateway setup running in hybrid mode with a PostgreSQL database, secured with cluster certificates, and configured using a declarative kong.yaml file. You can easily start and stop the services using the provided Makefile commands.