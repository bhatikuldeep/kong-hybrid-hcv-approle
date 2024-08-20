# Kong Gateway Hybrid Mode Setup

## Introduction

This guide explains how to set up Kong Gateway in hybrid mode using Docker Compose. The hybrid mode in Kong Gateway allows separation of the control plane and data plane, providing scalability and flexibility for large deployments. This setup includes using a PostgreSQL database for Kong, securing communication with cluster certificates, and using declarative configuration with decK.

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

### Stopping the Services

To stop the docker containers, run the following command:


```sh
make stop
```
- This command will stop and remove the containers defined in your Docker Compose file.

## Test the service
This docker compose runs  local httpbin container, check the decks/kong.yaml file to see the imported service in Kong gateway.

To test the Kong gateway service, run the following command:
`curl -X GET http://localhost:8000/anything` or `http :8000/anything`

## Kong manager
In this setup, Kong Manager is exposed on port 8002. You can access it in your web browser by navigating to http://localhost:8002.

## Conclusion
By following this guide, you will have a Kong Gateway setup running in hybrid mode with a PostgreSQL database, secured with cluster certificates, and configured using a declarative kong.yaml file. You can easily start and stop the services using the provided Makefile commands.