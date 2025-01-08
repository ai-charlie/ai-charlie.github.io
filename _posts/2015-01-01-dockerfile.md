Dockerfile是用于构建Docker镜像的文本文件，其中包含了一系列指令，用于描述镜像的创建过程。以下是Dockerfile的基本语法：

### 指令格式

- 每条指令都以大写字母开头，后面跟着参数，指令和参数之间用空格分隔，例如 `FROM ubuntu:latest`。
- 指令不区分大小写，但通常遵循大写的约定，以提高可读性。
- 可以使用 `#`进行单行注释，从 `#`开始到行末的内容都会被Docker忽略，例如 `# 这是一个注释`。

### 基础指令

- **FROM**：指定基础镜像，是Dockerfile中必须的第一条指令，如 `FROM ubuntu:22.04`，表示基于Ubuntu 22.04镜像构建新镜像。
- **MAINTAINER**：指定镜像的维护者信息，格式为 `MAINTAINER <name>`或 `MAINTAINER <name> <email>`，如 `MAINTAINER John Doe johndoe@example.com`，该指令已被 `LABEL maintainer`替代。
- **RUN**：用于在镜像构建过程中执行命令，有两种格式， shell格式 `RUN <command>`和exec格式 `RUN ["executable", "param1", "param2"]`，如 `RUN apt-get update && apt-get install -y python3`。
- **CMD**：指定容器启动时要执行的命令，有三种格式，`CMD ["executable","param1","param2"]`（exec格式，推荐）、`CMD ["param1","param2"]`（作为ENTRYPOINT的参数）和 `CMD command param1 param2`（shell格式）。如 `CMD ["python3", "app.py"]`。
- **ENTRYPOINT**：配置容器启动时要执行的可执行文件，让容器像命令一样运行。格式与CMD类似，如 `ENTRYPOINT ["nginx", "-g", "daemon off;"]`。

### 配置指令

- **WORKDIR**：设置容器内的工作目录，后续的指令如 `RUN`、`CMD`、`COPY`等操作都将在该目录下进行，可多次使用，如 `WORKDIR /app`。
- **EXPOSE**：声明容器运行时监听的端口，格式为 `EXPOSE <port> [<port>/<protocol>...]`，如 `EXPOSE 8080`或 `EXPOSE 8080/tcp`。
- **VOLUME**：创建挂载点，用于将容器内的数据存储到宿主机或其他容器中，格式为 `VOLUME ["/data"]`或 `VOLUME /data`，如 `VOLUME ["/app/data"]`。
- **USER**：设置运行容器时的用户名或UID，如 `USER appuser`或 `USER 1001`。

### 构建指令

- **ADD**：将本地文件或目录复制到容器中，还可以自动解压URL和tar文件，格式为 `ADD <src>... <dest>`，如 `ADD. /app`。
- **COPY**：将本地文件或目录复制到容器中，格式与ADD类似，但不支持自动解压和URL，如 `COPY. /app`。
- **ARG**：定义构建时的变量，可在构建命令中通过 `--build-arg`参数传递值，格式为 `ARG <name>[=<default value>]`，如 `ARG VERSION=1.0`。

### 元数据指令

- **LABEL**：为镜像添加元数据标签，格式为 `LABEL <key>=<value> [<key>=<value>...]`，如 `LABEL version="1.0" description="My app image"`。

### 多阶段构建指令

- 可以使用多个 `FROM`指令在一个Dockerfile中进行多阶段构建，以减小最终镜像的大小。例如：

```dockerfile
FROM golang:1.18 as build
WORKDIR /app
COPY..
RUN go build -o myapp

FROM alpine:latest
WORKDIR /app
COPY --from=build /app/myapp.
CMD ["./myapp"]
```
