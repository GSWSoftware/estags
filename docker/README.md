# Environment_Setup

## Node Installation
NVM usage is recommended due to Node Version compatibilities.
You'll find updated instructions to install it [here](https://github.com/creationix/nvm#install-script).

- `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash`

## Docker Installation on Ubuntu 16

1. `sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D`
2. `sudo apt-add-repository 'deb https://apt.dockerproject.org/repo ubuntu-xenial main'`
3. `sudo apt-get update`
4. `sudo apt-get install -y docker-engine`

5. Verify Docker Daemon

   - `sudo systemctl status docker`

6. Command to prevent `sudo` usage on Docker commands
   - `sudo usermod -aG docker $(whoami)`

## Docker Compose Installation

1. `sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-\`uname -s\`-\`uname -m\` -o /usr/local/bin/docker-compose`

2. `sudo chmod +x /usr/local/bin/docker-compose` ( Permissao para docker-compose )

3. `docker-compose --v` ( Verifica instalação do docker-compose )

# POSTGRES

It's very important to configure the variables:

    export PG_USER=algumuser
    export PG_PASSWORD=algumpass
    export DATABASE_URL=postgres://$PG_USER:$PG_PASSWORD@localhost/database_test

```sh
docker exec -it <POSTGRES_DOCKER_ID> bash
```

How to dump from remote:

```sh
pg_dump -h <HOST_WITHOUT_PORT> -Fc -o -U <REMOTE_USER> <REMOTE_DATABASE_NAME> > database_dump.backup
```

How to restore:

```sh
pg_restore --no-privileges --no-owner -U <LOCAL_USER> -d <LOCAL_DB_NAME> -1 database_dump.backup
```

# RUNNING THIS BAGAÇA

The instructions to install docker are available [here](https://docs.docker.com/engine/installation/linux/ubuntu/) and [docker-compose here](https://docs.docker.com/compose/install/). Don't forget to use the post install script to manage docker as a [noon root user](https://docs.docker.com/engine/installation/linux/linux-postinstall/#manage-docker-as-a-non-root-user).

Just start the docker-compose using the command: `docker-compose up -d` and then `docker-compose ps` to check if everything is working.

- Killing docker-compose:

  `docker-compose stop`

  `docker-compose rm`

  `docker-compose up -d --remove-orphans`

- Testing database docker connection:

      `psql -h localhost -p 5432 -d database_test -U $PG_USER --password`

(The package name for `psql` usage is `postgresql`)

then, just type the password

## Restart Docker
`sudo systemctl restart docker`

## Start Redis, Mongo, RabbitMQ, ElasticSearch ( execute the command below on the folder containing docker-compose.yaml file )

1. `docker-compose up -d`

2. `docker ps` (verify if the containers were started)

## # Robomongo Installation on Linux

1. [Download Robomongo](https://robomongo.org/download)
2. Extract the downloaded file on a folder named 'robomongo'
3. Copy the content to `/usr/local/bin/`
4. Add the robomongo reference on the environment variable
   `export PATH=/usr/local/bin/robomongo/bin:$PATH`
   so the command `robomongo` will open the application

## # How to dump:

I've used mongo3:

```sh
wget http://downloads.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1404-3.4.10-rc0.tgz -O mongo3.tgz
tar -xvzf mongo3.tgz
mv mongodb-linux-x86_64-ubuntu1404-3.4.10-rc0/ mongo3
```

```sh
./mongodump --uri mongodb://chablau:pega_la_do_heroku_parça
```

How to restore:

```sh
./mongorestore --drop --noIndexRestore -d material_base dump/databasename/ &
```

_Don't_ be afraid to ask.