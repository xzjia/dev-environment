# To setup development on a Windows machine

## What's this

This is the steps to set up a development environment on a Windows machine. Currently,
- A PostgreSQL database
- A Redis instance
will be covered.
Note this repo doesn't build any container images and use the public ones on docker hub.

## Install docker
There are two types of Windows: Ones support Windows natively (Windows 10), and ones that don't(Windows 7).
- If your Windows supports docker natively, go ahead with [docker for windows](https://www.docker.com/docker-windows)
- If your Windows doesn't support docker natively, go ahead with [docker toolbox](https://docs.docker.com/toolbox/toolbox_install_windows/)
    - This docker toolbox will create a VM on your linux machine using VirtualBox, and run docker inside. This machine is often refered as `docker machine`.
You should be able to see the results for `docker version` and `docker-compose version` after your installation succeeded.

```bash
$ docker version
Client:
 Version:       18.03.0-ce
 API version:   1.37
 Go version:    go1.9.4
 Git commit:    0520e24302
 Built: Fri Mar 23 08:31:36 2018
 OS/Arch:       windows/amd64
 Experimental:  false
 Orchestrator:  swarm

Server:
 Engine:        18.05.0-ce
  API version:  go1.10.1nimum version 1.12)
  Git commit:   Wed May  9 22:20:42 2018
  OS/Arch:      false/amd64
$ docker-compose version
docker-compose version 1.20.1, build 5d8c71b2
docker-py version: 3.1.4
CPython version: 3.6.4
OpenSSL version: OpenSSL 1.0.2k  26 Jan 2017
```

# Start up the resources

```bash
# The paper work: Clone this repository and cd into it.
git clone https://github.com/xzjia/dev-environment.git
cd dev-environment

# Run containers defined in docker-compose.yml
docker-compose up -d

# Check the container status
docker-compose ps
```
And now you should be able to have the resources defined in the `docker-compose.yml`.
If you are running Windows, a database instance should be running at `192.168.99.100:5432`, a redis instance running on `192.168.99.100:6379`.
NOTE: The IP address of those resources' host is not `127.0.0.1`, instead is usually `192.168.99.100`, which can be picked up via `docker-machine ip`.

# How to debug

- To get a shell at one container
    - `$ docker exec -it {container id or container name} bash`
- To login into the database
    - `# psql -U postgres`
- To go into the redis
    - `# redis-cli`

# Set up Kafka

```bash
# To create a topic
$ docker run --net=devenvironment_kafka --rm confluentinc/cp-kafka:5.0.0 \
  kafka-topics --create --topic foo --partitions 1 --replication-factor 1 \
  --if-not-exists --zookeeper zookeeper:2181

# To verify the topic creation
$ docker run --net=devenvironment_kafka --rm confluentinc/cp-kafka:5.0.0 \
  kafka-topics --describe --topic foo --zookeeper zookeeper:2181

# To publish some simple messages
$ docker run --net=devenvironment_kafka --rm confluentinc/cp-kafka:5.0.0 \
bash -c "seq 42 | kafka-console-producer --request-required-acks 1 \
  --broker-list kafka:9092 --topic foo && echo 'Produced 42 messages.'"

# To see messages in a topic
$ docker run --net=devenvironment_kafka --rm confluentinc/cp-kafka:5.0.0 \
  kafka-console-consumer --bootstrap-server kafka:9092 --topic foo --from-beginning --max-messages 42

# To get into an interactive shell
$ docker run -it --net=devenvironment_kafka --rm confluentinc/cp-kafka:5.0.0 /bin/bash
# From here, you can just run kafka related commands and see the results

# For a cheat sheet style guide, refer https://gist.github.com/ursuad/e5b8542024a15e4db601f34906b30bb5


```
