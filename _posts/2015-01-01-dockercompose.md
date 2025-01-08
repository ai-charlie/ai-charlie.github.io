Docker Compose是用于定义和运行多容器Docker应用程序的工具，通过YAML文件来配置应用程序的服务、网络和卷等。以下是Docker Compose的基本语法：

### 版本声明

在Docker Compose文件的开头，需要指定Compose文件的版本，这决定了文件中可以使用的语法和功能。例如：

```yaml
version: '3.9'
```

### 服务定义

- **services**：是Docker Compose文件的核心部分，用于定义应用程序中的各个服务。每个服务都有一个名称，以下是一个简单的示例：

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
  db:
    image: mysql:5.7
    environment:
      - MYSQL_ROOT_PASSWORD=secret
```

- **image**：指定服务使用的Docker镜像。
- **ports**：用于将容器内部的端口映射到宿主机的端口，格式为 `宿主机端口:容器端口`。
- **environment**：设置容器内的环境变量，可以是键值对的形式。

### 网络配置

- **networks**：用于定义服务之间的网络连接。可以为服务指定自定义的网络，使它们能够通过服务名称进行通信。示例如下：

```yaml
services:
  web:
    image: nginx:latest
    networks:
      - my_network
  db:
    image: mysql:5.7
    networks:
      - my_network

networks:
  my_network:
    driver: bridge
```

- **driver**：指定网络驱动类型，常见的有 `bridge`、`host`、`overlay`等。

### 卷配置

- **volumes**：用于定义数据卷，实现容器与宿主机之间的数据共享或容器之间的数据共享。示例如下：

```yaml
services:
  web:
    image: nginx:latest
    volumes:
      -./html:/usr/share/nginx/html
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

- 可以使用宿主机路径和容器内路径的映射形式，也可以指定一个命名卷，让Docker自动管理卷的创建和挂载。

### 其他配置选项

- **depends_on**：用于定义服务之间的依赖关系，确保在启动某个服务之前，先启动其依赖的服务。例如：

```yaml
services:
  web:
    image: nginx:latest
    depends_on:
      - db
  db:
    image: mysql:5.7
```

- **restart**：设置服务的重启策略，常见的值有 `always`（总是重启）、`on-failure`（只有在容器以非零退出码退出时才重启）、`no`（不重启）。例如：

```yaml
services:
  web:
    image: nginx:latest
    restart: always
```

- **build**：如果服务的镜像是基于本地Dockerfile构建的，可以使用 `build`选项指定构建上下文和Dockerfile路径。例如：

```yaml
services:
  app:
    build:
      context:.
      dockerfile: Dockerfile.dev
```
