# Docker Commands

## Table of Contents
- [List Containers](#list-containers)
    - [List all running containers](#list-running-containers)
    - [List all containers](#list-all-containers)
    - [List IDs of all running containers](#list-all-container-ids)
- [Get Container ID From Name](#get-container-id-from-name)
- [Stop Container](#stop-container)
- [Start Container](#start-container)
- [Remove Container](#remove-container)
- [Inspect Container](#inspect-container)
- [Execute a Command in a Running Container](#exec-running-container)

## List Containers <a name="list-containers"></a>
### List all running containers <a name="list-running-containers"></a>
```console
$ docker container ls
```
或者
```console
$ docker ps
```
它们的 output 和 option flag 都是一样的.

#### Output
| Column | Description |
| -------- | ------- |
| Container ID | This is the unique ID of the container. It is an SHA-256 |
| Image | This is the name of the container image from which this container is instantiated. |
| Command | This is the command that is used to run the main process in the container. |
| Created | This is the date and time when the container was created. |
| Status | This is the status of the container (created, restarting, running, removing, paused, exited, or dead). |
| Ports | This is the list of container ports that have been mapped to the host. |
| Names | This is the name assigned to this container (multiple names are possible). |

### List all containers <a name="list-all-containers"></a>
This will list containers in any state, such as created, running, or exited.
```console
$ docker container ls -a
```

### List IDs of all running containers <a name="list-all-container-ids"></a>
quiet mode, 用 -q 来 list IDs of containers. 要 list all 就再加一个 -a.
```console
$ docker container ls -q
```

## Get Container ID From Name <a name="get-container-id-from-name"></a>
```console
$ export CONTAINER_ID=$(docker container ls -a | grep {container_name} | awk '{print $1}')
```

## Stop Container <a name="stop-container"></a>
```console
$ docker container stop {container_id|container_name}
```
The main process inside the container will receive SIGTERM, and after a grace period, SIGKILL. The first signal can be changed with the STOPSIGNAL instruction in the container's Dockerfile, or the --stop-signal option to docker run.

可以用 -t 来specify wait的时间
```console
$ docker container stop {container_id} -t {wait_time_in_second}
```

## Start Container <a name="start-container"></a>
```console
$ docker container start {container_id|container_name}
```

## Remove Container <a name="remove-container"></a>
When we run the docker container ls -a command, we can see quite a few containers that are in the Exited status. If we don't need these containers anymore, then it is a good thing to remove them from memory; otherwise, they unnecessarily occupy precious resources.
```console
$ docker container rm {container_id|container_name}
```
或者
```console
$ docker rm {container_id|container_name}
```
Sometimes, removing a container will not work as it is still running. If we want to force a removal, no matter what the condition of the container currently is, we can use the command-line parameter -f or --force, which sends a SIGKILL.
```console
$ docker container rm {container_id|container_name} -f
```

### Remove All Stopped Containers <a name="remove-all-stopped-containers"></a>

```console
$ docker container prune
```

### Inspect Container <a name="inspect-container"></a>
```console
$ docker container inspect {container_id|container_name}
```

### Execute a Command in a Running Container <a name="exec-running-container"></a>
```console
$ docker exec {container_id|container_name}
```
一般用 -it
```console
$ docker exec -it {container_id|container_name}
```
看看这个 [Stackoverflow post](https://stackoverflow.com/questions/30137135/confused-about-docker-t-option-to-allocate-a-pseudo-tty) 的解释.
