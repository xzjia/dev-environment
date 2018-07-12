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
docker-compose up -d
docker-compose ps
```
