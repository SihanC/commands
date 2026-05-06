# Docker Commands

## Table of Contents
- [List Containers](#list-containers)
    - [List all running containers](#list-running-containers)
    - [List all containers](#list-all-containers)
    - [List IDs of all running containers](#list-all-container-ids)
- [Get Container ID From Name](#get-container-id-from-name)
- [Create Container](#create-container)
- [Stop Container](#stop-container)
- [Start Container](#start-container)
- [Remove Container](#remove-container)
    - [Remove All Stopped Containers](#remove-all-stopped-containers)
- [Inspect Container](#inspect-container)
- [Execute a Command in a Running Container](#exec-running-container)
- [Run Container](#run-container)
- [Container Logs](#container-logs)
- [Copy Files Between Host and Container](#copy-files)
- [Container Resource Usage](#container-resource-usage)
- [Images](#images)
- [Build Image](#build-image)
- [Pull and Push Image](#pull-and-push-image)
- [Ports](#ports)
- [Volumes](#volumes)
- [Networks](#networks)
- [Docker Compose](#docker-compose)
- [Cleanup](#cleanup)
- [Docker System Info](#docker-system-info)

## List Containers <a name="list-containers"></a>
### List all running containers <a name="list-running-containers"></a>
```console
$ docker container ls
```
Short form:
```console
$ docker ps
```
`docker container ls` 和 `docker ps` 的 output 和 options 基本一样.

#### Output
| Column | Description |
| -------- | ------- |
| Container ID | Container 的唯一 ID, 是一个 SHA-256 hash 的短版本. |
| Image | 这个 container 使用的 image. |
| Command | Container 启动时运行的主命令. |
| Created | Container 创建时间. |
| Status | Container 当前状态, 比如 created, restarting, running, paused, exited, dead. |
| Ports | Container ports 和 host ports 的映射. |
| Names | Container name. |

### List all containers <a name="list-all-containers"></a>
List containers in any state, 比如 created, running, exited.
```console
$ docker container ls -a
```
Short form:
```console
$ docker ps -a
```

### List IDs of all running containers <a name="list-all-container-ids"></a>
quiet mode, 用 `-q` 只输出 container IDs.
```console
$ docker container ls -q
```
List IDs of all containers:
```console
$ docker container ls -aq
```

## Get Container ID From Name <a name="get-container-id-from-name"></a>
推荐用 `--filter`, 比 `grep | awk` 更稳定. `^/{container_name}$` 是精确匹配 container name.
```console
$ docker container ls -aq --filter "name=^/{container_name}$"
```

如果要存到变量:
```console
$ export CONTAINER_ID=$(docker container ls -aq --filter "name=^/{container_name}$")
```

也可以用原始 pipeline:
```console
$ export CONTAINER_ID=$(docker container ls -a | grep {container_name} | awk '{print $1}')
```

## Create Container <a name="create-container"></a>
`docker container create` 会根据 image 创建一个 container, 但不会启动它. 这一步会生成 container ID, 创建 writable layer, 保存配置、env、port mapping、volume mapping 等 metadata.
```console
$ docker container create --name {container_name} {image}
```

创建之后可以用 `docker container start` 启动. 大多数日常场景会直接用 `docker run`, 因为 `docker run` 等于 `docker container create` + `docker container start`.

## Stop Container <a name="stop-container"></a>
```console
$ docker container stop {container_id|container_name}
```
`docker container stop` 是停止一个 running container, 不会删除 container, 也不会删除它的 writable layer. Container 里的 main process 会先收到 `SIGTERM`; grace period 之后如果还没退出, Docker 会发 `SIGKILL`.

用 `-t` 指定 wait time, 单位是 second.
```console
$ docker container stop -t {wait_time_in_second} {container_id|container_name}
```

## Start Container <a name="start-container"></a>
`docker container start` 是启动一个已经存在的 container, 通常是 created 或 exited 状态的 container. 它不会重新从 image 创建一个新 container.
```console
$ docker container start {container_id|container_name}
```

## Remove Container <a name="remove-container"></a>
`docker container rm` 是删除 container 本身. 删除之后, 这个 container 的 ID/name、metadata、writable layer 都没了. 但 image 不会被删除.

Remove a stopped container:
```console
$ docker container rm {container_id|container_name}
```
Short form:
```console
$ docker rm {container_id|container_name}
```

如果 container 还在 running, 普通 `rm` 会失败. 可以用 `-f` force remove, Docker 会发送 `SIGKILL`.
```console
$ docker container rm -f {container_id|container_name}
```

### Remove All Stopped Containers <a name="remove-all-stopped-containers"></a>
Remove all stopped containers.
```console
$ docker container prune
```

## Inspect Container <a name="inspect-container"></a>
`docker container inspect` 会输出 container 的低层 JSON 信息, 包括 env、mounts、network、IP、port mapping、restart policy 等. 排查 container 配置时很常用.
```console
$ docker container inspect {container_id|container_name}
```

## Execute a Command in a Running Container <a name="exec-running-container"></a>
`docker exec` 是在 already running 的 container 里额外运行一个 command. 它不会创建新 container, 也不会重启 container. 如果 container 没有 running, `exec` 会失败.
```console
$ docker exec {container_id|container_name} {command}
```

Interactive command 一般加 `-it`.
```console
$ docker exec -it {container_id|container_name} {command}
```
进入 bash:
```console
$ docker exec -it {container_id|container_name} /bin/bash
```
如果 image 里没有 bash, 可以试试 sh:
```console
$ docker exec -it {container_id|container_name} /bin/sh
```
看看这个 [Stackoverflow post](https://stackoverflow.com/questions/30137135/confused-about-docker-t-option-to-allocate-a-pseudo-tty) 的解释.

## Run Container <a name="run-container"></a>
`docker run` 是最常用的启动方式. 它等于 `docker container create` + `docker container start`: 先从 image 创建一个新 container, 再启动它.
```console
$ docker run {image}
```

如果本地没有这个 image, Docker 会先尝试从 registry pull image, 然后再 create + start.

常用组合: detached mode + container name + port mapping.
```console
$ docker run -d --name {container_name} -p {host_port}:{container_port} {image}
```
`-d` 是 detached mode, container 在后台运行, terminal 不会 attach 到 container 的 stdout/stderr.

Run and remove the container automatically after it exits.
```console
$ docker run --rm {image}
```
`--rm` 适合一次性任务. Container exit 后会自动删除, 所以之后 `docker ps -a` 里也不会留下 stopped container.

Run interactively.
```console
$ docker run -it --rm {image} /bin/bash
```
`-i` 保持 stdin open, `-t` 分配 pseudo-TTY. 两个合起来通常用于 interactive shell.

Set environment variables.
```console
$ docker run -e {KEY}={VALUE} {image}
```

Mount a local directory into container.
```console
$ docker run -v {host_path}:{container_path} {image}
```
`-v` 会把 host 上的 path mount 到 container 里. 写到 `{container_path}` 的数据会落到 host 的 `{host_path}`, 适合保存数据或挂载源码.

Set working directory inside container.
```console
$ docker run -w {container_path} {image}
```
`-w` 会设置 command 在 container 里的 working directory, 类似先 `cd {container_path}` 再执行 command.

## Container Logs <a name="container-logs"></a>
`docker logs` 查看 container 主进程写到 stdout/stderr 的内容. 它不是进入 container 读 log file; 如果应用只写文件不写 stdout/stderr, 这里可能看不到.
```console
$ docker logs {container_id|container_name}
```

Follow logs.
```console
$ docker logs -f {container_id|container_name}
```

Show recent logs.
```console
$ docker logs --tail {line_count} {container_id|container_name}
```

Show logs with timestamps.
```console
$ docker logs -t {container_id|container_name}
```

## Copy Files Between Host and Container <a name="copy-files"></a>
`docker cp` 可以在 host 和 container filesystem 之间复制文件. Container 不一定要 running, stopped container 也可以 copy.

Copy from host to container.
```console
$ docker cp {host_path} {container_id|container_name}:{container_path}
```

Copy from container to host.
```console
$ docker cp {container_id|container_name}:{container_path} {host_path}
```

## Container Resource Usage <a name="container-resource-usage"></a>
`docker stats` 查看 running containers 的 CPU, memory, network, block I/O. 它是 live view, 默认会持续刷新.
```console
$ docker stats
```

只看某一个 container.
```console
$ docker stats {container_id|container_name}
```

查看 container 里的 processes.
```console
$ docker top {container_id|container_name}
```

## Images <a name="images"></a>
Image 是只读模板, container 是 image 的运行实例. 同一个 image 可以创建多个 containers.

List local images.
```console
$ docker image ls
```
或者
```console
$ docker images
```

Remove image.
```console
$ docker image rm {image_id|image_name}
```
如果还有 container 依赖这个 image, 删除 image 可能会失败. 需要先删除相关 containers, 或确认后 force remove.

Remove unused images.
```console
$ docker image prune
```
`docker image prune` 默认只删除 dangling images, 也就是没有 tag 且没有被 container 使用的 image layers.

Remove all unused images, not just dangling images.
```console
$ docker image prune -a
```
`-a` 会删除所有没有被 container 使用的 images, 包括有 tag 的 images.

Tag an image.
```console
$ docker tag {source_image}:{tag} {target_image}:{tag}
```

## Build Image <a name="build-image"></a>
`docker build` 根据 Dockerfile 和 build context 构建 image. 最后的 `.` 是 build context, Dockerfile 里的 `COPY`/`ADD` 只能访问 context 里的文件.

Build image from Dockerfile in current directory.
```console
$ docker build -t {image_name}:{tag} .
```

Build with a specific Dockerfile.
```console
$ docker build -f {dockerfile_path} -t {image_name}:{tag} .
```

Build without cache.
```console
$ docker build --no-cache -t {image_name}:{tag} .
```

## Pull and Push Image <a name="pull-and-push-image"></a>
`pull` 是从 registry 下载 image 到本地, `push` 是把本地 image 上传到 registry. Push 前通常需要先 `docker login`, 并且 image name 要带 registry/repository 权限对应的 tag.

Pull image from registry.
```console
$ docker pull {image_name}:{tag}
```

Push image to registry.
```console
$ docker push {image_name}:{tag}
```

Login to registry.
```console
$ docker login
```

## Ports <a name="ports"></a>
Container 默认有自己的 network namespace. `-p {host_port}:{container_port}` 是把 host 的 port 转发到 container 的 port. 没有 `-p` 时, container 内部服务通常不能直接从 host 访问.

查看 container 的 port mapping.
```console
$ docker port {container_id|container_name}
```

Run container with port mapping.
```console
$ docker run -p {host_port}:{container_port} {image}
```

Bind to localhost only.
```console
$ docker run -p 127.0.0.1:{host_port}:{container_port} {image}
```

## Volumes <a name="volumes"></a>
Volume 用来持久化数据. Container 删除后, named volume 默认还在, 所以数据库数据这类内容一般放 volume, 不放 container writable layer.

List volumes.
```console
$ docker volume ls
```

Create volume.
```console
$ docker volume create {volume_name}
```

Inspect volume.
```console
$ docker volume inspect {volume_name}
```

Remove volume.
```console
$ docker volume rm {volume_name}
```

Remove unused volumes.
```console
$ docker volume prune
```

Run container with named volume.
```console
$ docker run -v {volume_name}:{container_path} {image}
```

## Networks <a name="networks"></a>
Docker network 用来控制 containers 之间怎么互相访问. 在同一个 user-defined bridge network 里的 containers 可以用 container name 互相解析.

List networks.
```console
$ docker network ls
```

Create network.
```console
$ docker network create {network_name}
```

Inspect network.
```console
$ docker network inspect {network_name}
```

Connect a running container to a network.
```console
$ docker network connect {network_name} {container_id|container_name}
```

Disconnect a container from a network.
```console
$ docker network disconnect {network_name} {container_id|container_name}
```

Run container in a specific network.
```console
$ docker run --network {network_name} {image}
```

## Docker Compose <a name="docker-compose"></a>
Docker Compose 用一个 compose file 管理多 container 应用. Service 通常对应一个 container 模板, `docker compose up` 会按配置 create/start 相关 containers、networks、volumes.

Start services in the background.
```console
$ docker compose up -d
```

Start and rebuild images.
```console
$ docker compose up -d --build
```

Stop and remove containers, networks.
```console
$ docker compose down
```
`down` 会删除 compose 创建的 containers 和默认 network, 但不会删除 named volumes.

Stop and remove containers, networks, and volumes.
```console
$ docker compose down -v
```
`-v` 会额外删除 compose 管理的 volumes. 数据库、本地开发数据在 volume 里时要小心.

View logs.
```console
$ docker compose logs
```

Follow logs for all services.
```console
$ docker compose logs -f
```

Follow logs for one service.
```console
$ docker compose logs -f {service_name}
```

List compose services.
```console
$ docker compose ps
```

Run command in a running service container.
```console
$ docker compose exec {service_name} {command}
```
`compose exec` 类似 `docker exec`, command 跑在已经 running 的 service container 里.

Run a one-off command in a new service container.
```console
$ docker compose run --rm {service_name} {command}
```
`compose run` 会为这个 service 创建一个新的 one-off container 来跑 command, 不一定使用当前已经 running 的那个 service container.

## Cleanup <a name="cleanup"></a>
Cleanup 命令会删除资源, 执行前最好先用 `docker system df` 看磁盘占用. `prune` 类命令一般只删当前没有被使用的资源.

Remove stopped containers, unused networks, dangling images, and build cache.
```console
$ docker system prune
```

Also remove unused images.
```console
$ docker system prune -a
```

Also remove unused volumes.
```console
$ docker system prune --volumes
```

Remove build cache.
```console
$ docker builder prune
```

Remove all stopped containers.
```console
$ docker container prune
```

## Docker System Info <a name="docker-system-info"></a>
Show Docker disk usage.
```console
$ docker system df
```

Show detailed Docker disk usage.
```console
$ docker system df -v
```

Show Docker server/client info.
```console
$ docker info
```

Show Docker version.
```console
$ docker version
```
