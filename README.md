# Dockerized Tilemill

This Dockerfile builds an image that runs a [tilemill](https://github.com/tilemill-project/tilemill) server. It is not optimized for space and takes up almost 2GB.

## Run with docker-compose

Install docker and docker-compose and clone this repo. Change line 11 (below `volumes:` in the `docker-compose.yaml` file so that it points to your directory where your map projects are or just an empty folder. The line should then read `/your/folder:/root/Documents/MapBox`

Then do this to run a tilemill server on your local machine:

```sh
$ cd /path/to/this/git/repo
$ docker-compose up
```

You can use tilemill in your browser under `127.0.0.1:20009`.

Stop the server again with `Ctrl+C`. If you want to keep the server running in the background, use `$ docker-compose up -d`. You can then stop the server only with `$ docker-compose down` from the same directory.


## Without docker-compose

### Build

Simply use docker build and tag it anyway you like (this project is not on dockerhub):

```sh
$ cd /path/to/this/git/repo/context
$ docker build -t schachmett/tilemill .
```

This builds tilemill on top of Ubuntu 20.04 and nodejs 6.11 (and throws a lot of deprecation warnings). The `config.json` file for tilemill inside the image then configures it to be a server and listen on port `0.0.0.0`.

### Run

Mount your MapBox project folder (if you don't have one already, just make a new empty folder) into the container with `-v`. 

You can also link in a postgres server with the `--link` option. For example you can use [this](https://hub.docker.com/r/openfirmware/postgres-osm) docker image which can be fed `.pbf` files with [this other](https://hub.docker.com/r/openfirmware/osm2pgsql) docker image. There is a nice [tutorial in the corresponding github repo](https://github.com/openfirmware/docker-osm2pgsql/blob/master/TUTORIAL.markdown).

This is the command to run the tilemill server the first time then:

```sh
$ cd /path/to/this/git/repo
$ docker run \
  --name tilemill \
  -d \
  -p 127.0.0.1:20008:20008 -p 127.0.0.1:20009:20009 \
  --link postgres-osm:pg \		# optional, has to be running first
  -v /path/to/your/mapbox/projects:/root/Documents/MapBox \
  schachmett/tilemill:latest
```

### Use

After running the first time, you can stop the container using `$ docker stop tilemill` and start it again with `$ docker start tilemill`. Pay attention that you did not move the mapbox project folder, though. If you linked in a postgres database server, start that one first.
