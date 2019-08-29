---
title: "MongoDump and MongoRestore MongoDB Database"
date: 2019-08-29T20:49:18+03:00
draft: false
toc: false
images:
tags: 
  - untagged
---

### How to Dump MongoDB Ddatabase from a container

#### What is [mongodump](https://docs.mongodb.com/manual/reference/program/mongodump/#bin.mongodump)

**mongodump** is a utility for creating a binary export of the contents of a database. mongodump can export data from either *mongod* or *mongos* instances.

Run *mongodump* from the system command line, not the *mongo* shell.

#### How to connect to a MongoDB instance

- Local MongoDB instance running on port `27017` using the default settings

```bash
mongodump
```

- By specifying a host and/or port of the MongoDB instance

  - [--uri connection string](https://docs.mongodb.com/manual/reference/program/mongodump/#cmdoption-mongodump-uri)

  ```bash
  mongodump --uri "mongodb://mongodb0.example.com:27017" [additional options]
  ```

  - [--host](https://docs.mongodb.com/manual/reference/program/mongodump/#cmdoption-mongodump-host)

  ```bash
  mongodump --host "mongodb0.example.com:27017"  [additional options]
  ```

  - Specify the hostname and port in the [--host](https://docs.mongodb.com/manual/reference/program/mongodump/#cmdoption-mongodump-host) and [--port](https://docs.mongodb.com/manual/reference/program/mongodump/#cmdoption-mongodump-port)

  ```bash
  mongodump --host "mongodb0.example.com" --port 27017 [additional options]
  ```

#### You can also check

- [Connect to a Replica Set](https://docs.mongodb.com/manual/reference/program/mongodump/#connect-to-a-replica-set)

- [Connect to a Sharded Cluster](https://docs.mongodb.com/manual/reference/program/mongodump/#connect-to-a-sharded-cluster)

### How to restore MongoDB Database from a container

Say you are using `docker-compose` to link/connect mongo with API and you need to backup your database. You will need to first of all stop your API before proceeding i.e if using docker:

  ```bash
  docker stop api-app
  ```

When backups it's important to [save file and directory permissions.](https://askubuntu.com/questions/225865/copy-files-without-losing-file-folder-permissions).

- On source computer

  ```bash
  cd /path/to/folder/to/copy
  tar cvpzf put_your_name_here.tar.gz .
  ```

- On destination computer

  ```bash
  cd /path/to/destination/folder
  tar xpvzf put_your_name_here.tar.gz
  ```

`tar`  will recreate the archived folder structure with all permissions intact

Check from your Dockerfile or docker-compose.yml  the name of MongoDB container.

1. List docker containers

  ```bash
  docker ps -a
  ```

2. Enter inside mongo container

```bash
docker exec -it mongodb /bin/bash
# mongodb is the containers name
```

3. Go to root `/` directory and backup database to files inside container to directory `/dump`

```bash
mongodump -o /dump/
```

4. Exit from inside of container and Copy backup directory `/dump` from inside of container to current directory

```bash
docker cp mongodb:/dump .
# mongodb is the containers name
```

5. Restore backup later (restore from /data/dump) -- inside container

```bash
$ docker cp dump mongodb:/data/

# /data/ is a directory inside the container

$ docker exec -it mongodb /bin/bash
$ cd /data
$ mongorestore --drop --noIndexRestore --db <database_name> /data/dump/<database_name>/
$ exit

# That db_name can be for example "cities"

$ mongorestore --drop --noIndexRestore --db cities /data/dump/cities/
```

- Restore to another mongo database, in different port.

  ```bash
  mongorestore --port <port_no>
  ```

#### Useful Backup and restore scripts

##### Backup Script

```bash
#!/bin/bash
DATE=$(date +%Y-%m-%d-%H-%M)
SCRIPTPATH="$( cd "$(dirname "$0")" ; pwd -P )"
cd $SCRIPTPATH
mkdir -p backups/$DATE
docker ps -a | grep 'mongodb' &> /dev/null
if [ $? = 0 ]; then
  docker exec -t mongodb bash -c "rm -fr /dump ; mkdir /dump ; mongodump -o /dump/"
  docker cp mongodb:/dump $SCRIPTPATH/backups/$DATE
  tar -zc -f backups/$DATE.tgz -C $SCRIPTPATH/backups/$DATE wekan
  if [ -f backups/$DATE.tgz ]; then
    rm -fr backups/$DATE
    find $SCRIPTPATH/backups/ -name "*.tgz" -mtime +7 -delete
  fi 
else
  echo "mongodb container is not running"
  exit 1
fi
```

##### Restore Script

```bash
#!/bin/bash
if [ $# -eq 0 ]
  then
    echo "Supply a path to a tgz file!"
    exit 1
fi

SCRIPTPATH="$( cd "$(dirname "$0")" ; pwd -P )"
DATE=$(date +%Y-%m-%d-%H-%M)

docker ps -a | grep 'mongodb' &> /dev/null
if [ $? = 0 ]; then

  if [ -f $1 ]; then
    mkdir -p $SCRIPTPATH/backups/$DATE-restore
    tar -zx -f $1 -C $SCRIPTPATH/backups/$DATE-restore
    docker exec -t mongodb bash -c "rm -fr /restore ; mkdir /restore"
    docker cp $SCRIPTPATH/backups/$DATE-restore/wekan mongodb:/restore
    docker exec -t mongodb bash -c "mongorestore --drop --noIndexRestore --db wekan /restore/wekan/"
  fi
else
  echo "mongodb container is not running"
  exit 1
fi
```

### Some other cool tips on this topic

The trick is to disable pseudo-tty allocation. Otherwise extra characters are added to the backup file. They prevent mongorestore from working properly.

- Using `docker`

With docker, pseudo-tty allocation is deactivated by default, but the `-i` (interactive) option is required with the restore command.

```bash
docker exec <mongodb container> sh -c 'mongodump --archive' > db.dump
```

```bash
docker exec -i <mongodb container> sh -c 'mongorestore --archive' < db.dump
```

- using `docker-compose`

With docker-compose, pseudo-tty allocation needs to be deactivated explicitly each time with `-T`

```bash
docker-compose exec -T <mongodb service> sh -c 'mongodump --archive' > db.dump
```

```bash
docker-compose exec -T <mongodb service> sh -c 'mongorestore --archive' < db.dump
```

#### References

- [mongodb](https://docs.mongodb.com/manual/reference/program/mongodump/#bin.mongodump)

- [Export Docker Mongo Data](https://github.com/wekan/wekan/wiki/Export-Docker-Mongo-Data)

- [mongodump and mongorestore with Docker](https://jeromejaglale.com/doc/programming/mongodb_docker_mongodump_mongorestore)
