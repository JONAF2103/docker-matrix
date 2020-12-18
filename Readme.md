# Synapse Matrix Docker

This project aims to simplify the configuration and execution of the synapse matrix server.
With this you can startup a new synapse matrix server locally or on your production server
without having to do hard configuration steps.

## Pre Requisites

- Docker Installed
- Docker Compose Installed

## How to use it

First clone this repository to your local, then that you need to configure the required environment variables:

- **VIRTUAL_HOST:** the hostname where the matrix server will be running
- **VIRTUAL_PORT:** the port number where the matrix server will be exposed
- **LETSENCRYPT_HOST:** same as VIRTUAL_HOST var, where the reverse proxy will be running
- **SYNAPSE_SERVER_NAME:** the synapse server name, this should be the same as the VIRTUAL_HOST for synapse internal use
- **SYNAPSE_REPORT_STATS:** synapse flag to show stats on or not

You can edit these variables by editing `synapse/docker-compose.yml` file like this:
```yaml
...
    environment:
      VIRTUAL_HOST: "subdomain.domain.com"
      VIRTUAL_PORT: 8008
      LETSENCRYPT_HOST: "subdomain.domain.com"
      SYNAPSE_SERVER_NAME: "subdomain.domain.com"
      SYNAPSE_REPORT_STATS: "no"
...
```

**Note: you can just export this variables on your environment before using the `matrix` script.**

After you have the variables already set you need to create a docker network called server, you can do it like this:

```
sudo docker network create server
```

Next you have to create the data directory inside the synapse folder and set right permissions by running:

```
mkdir synapse/data
chown -R $USER synapse
chmod -R a+rw synapse
```

The last step is to generate synapse data folder like this:

```
cd synapse
sudo docker-compose run --rm synapse generate
```

Finally you can go back to root folder and use the `matrix` script to start the server.
