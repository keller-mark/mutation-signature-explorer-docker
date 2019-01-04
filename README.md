# iMuSE - Docker

## Setup
To run iMuSE and iMuSE-server locally using docker:

Clone this repository
```
git clone https://github.com/lrgr/mutation-signature-explorer-docker.git
```

Enter into this repository
```
cd mutation-signature-explorer-docker
```

### The data directory
Notice the `obj` folder within `mutation-signature-explorer-docker`.
This is where iMuSE-server will look for data.
Run `pwd` in this directory and make note of the absolute path.

The two metadata files in the `obj` directory can be updated to specify custom signatures and/or mutation datasets. Please see the [wiki](https://github.com/lrgr/mutation-signature-explorer-docker/wiki) for detailed instructions about file formats. 

### The database directory
Run the following command to create a `mysql` folder within `mutation-signature-explorer-docker`:

```sh
mkdir mysql
```

This is where iMuSE-server will store the database used for saving/sharing iMuSE sessions and history.
Run `pwd` in this directory and make note of the absolute path.

### The docker-compose file
Notice the `docker-compose.yml` file within `mutation-signature-explorer-docker`.

Update this file:

- Change the backend volumes path to the absolute path of your `obj` directory
- Change the db volumes path to the absolute path of your `mysql` directory

```
version: '2'
services:
  frontend:
    depends_on:
    - backend
    image: lrgr/imuse:latest
    ports:
    - "8000:80"
    environment:
    - IMUSE_SERVER_URL=http://localhost:8100/
  backend:
    depends_on:
    - db
    image: lrgr/imuse-server:latest
    ports:
    - "8100:80"
    links:
    - db
    volumes:
    - /path/to/your/obj:/obj                    <----------- UPDATE
  db:
    image: mysql:5.7
    environment:
    - MYSQL_ROOT_PASSWORD=root
    - MYSQL_DATABASE=imuse
    - MYSQL_USER=imuse
    - MYSQL_PASSWORD=imuse
    command: --default-authentication-plugin=mysql_native_password
    volumes:
    - /path/to/your/mysql:/var/lib/mysql        <----------- UPDATE
```

### Pull
Pull down the two docker containers specified in the docker-compose file
```
docker-compose pull
```


## Running

### Up
Run the two docker containers
```
docker-compose up
```

### Up - daemonized
Run the two docker containers in the background
```
docker-compose up -d
```
