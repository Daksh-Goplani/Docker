# Docker Container Commands

This is a working note for the Docker commands used by the test app. Add new commands and updates here as the project grows.

## Prerequisite

Create the shared network once before starting either container:

```cmd
docker network create mongo-network
```

## 1. MongoDB container

Starts MongoDB in the background, publishes port `27017`, and joins the shared network.

```cmd
docker run -d ^
  -p 27017:27017 ^
  --name mongo ^
  --network mongo-network ^
  -e MONGO_INITDB_ROOT_USERNAME=admin ^
  -e MONGO_INITDB_ROOT_PASSWORD=asdf ^
  mongo
```

## 2. Mongo Express container

Starts the Mongo Express web interface on port `8081` and connects it to the MongoDB container over the shared network.

```cmd
docker run -d ^
  -p 8081:8081 ^
  --name mongo-express ^
  --network mongo-network ^
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin ^
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=asdf ^
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:asdf@mongo:27017" ^
  mongo-express
```

## Future updates

- Add container stop, start, and removal commands.
- Add commands for the application container when it is Dockerized.
- Update credentials and connection settings if they change.
