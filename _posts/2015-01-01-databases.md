# SQLite

SQLite是一种轻量级的嵌入式数据库，常应用于Python开发中。

### 连接数据库

Python标准库中包含 `sqlite3`模块，可用于操作SQLite数据库。使用时先导入该模块，再通过 `connect()`函数连接数据库。若数据库不存在，会自动创建。示例代码如下：

```python
import sqlite3

# 连接到SQLite数据库，文件名为example.db
conn = sqlite3.connect('example.db')
# 创建游标对象
cursor = conn.cursor()
```

### 创建表

使用游标对象的 `execute()`方法执行SQL语句来创建表。示例代码如下：

```python
# 创建一个名为users的表，包含id、name和age列
cursor.execute('''CREATE TABLE IF NOT EXISTS users
                  (id INTEGER PRIMARY KEY AUTOINCREMENT,
                   name TEXT,
                   age INTEGER)''')
# 提交事务
conn.commit()
```

### 插入数据

可使用 `execute()`方法插入单条数据，或用 `executemany()`方法插入多条数据。示例代码如下：

```python
# 插入单条数据
data_single = ('John', 30)
cursor.execute("INSERT INTO users (name, age) VALUES (?,?)", data_single)

# 插入多条数据
data_many = [('Alice', 25), ('Bob', 35)]
cursor.executemany("INSERT INTO users (name, age) VALUES (?,?)", data_many)

# 提交事务
conn.commit()
```

### 查询数据

使用 `execute()`方法执行查询语句，并用 `fetchone()`、`fetchall()`或 `fetchmany()`方法获取查询结果。示例代码如下：

```python
# 查询所有数据
cursor.execute("SELECT * FROM users")
rows = cursor.fetchall()
for row in rows:
    print(row)

# 查询单条数据
cursor.execute("SELECT * FROM users WHERE id =?", (1,))
row = cursor.fetchone()
print(row)

# 查询多条数据
cursor.execute("SELECT * FROM users LIMIT 2")
rows = cursor.fetchmany(2)
for row in rows:
    print(row)
```

### 更新数据

使用 `execute()`方法执行更新语句来更新数据。示例代码如下：

```python
# 更新数据
new_age = 31
user_id = 1
cursor.execute("UPDATE users SET age =? WHERE id =?", (new_age, user_id))
# 提交事务
conn.commit()
```

### 删除数据

使用 `execute()`方法执行删除语句来删除数据。示例代码如下：

```python
# 删除数据
user_id = 2
cursor.execute("DELETE FROM users WHERE id =?", (user_id,))
# 提交事务
conn.commit()
```

### 关闭连接

完成数据库操作后，要关闭游标和数据库连接，以释放资源。示例代码如下：

```python
# 关闭游标和连接
cursor.close()
conn.close()
```


# MYSQL

### 安装MySQL镜像

可以从Docker Hub上拉取官方的MySQL镜像，在终端中执行以下命令：

```bash
docker pull mysql:latest
```

上述命令会拉取最新版本的MySQL镜像。也可以指定具体的版本号，如 `docker pull mysql:8.0.33`。

### 运行MySQL容器

拉取镜像后，使用以下命令运行MySQL容器：

```bash
docker run -d \
    --name mysql-container \
    -e MYSQL_ROOT_PASSWORD=rootpassword \
    -p 3306:3306 \
    mysql:latest
```

上述命令中各参数的含义如下：

- `-d`：表示以守护进程（后台）模式运行容器。
- `--name mysql-container`：为容器指定一个名称，这里是 `mysql-container`。
- `-e MYSQL_ROOT_PASSWORD=rootpassword`：设置MySQL root用户的密码，这里设置为 `rootpassword`，实际使用中应替换为强密码。
- `-p 3306:3306`：将容器内的3306端口映射到主机的3306端口，这样就可以通过主机的3306端口访问容器内的MySQL服务。

### 进入MySQL容器

要进入正在运行的MySQL容器，可以使用以下命令：

```bash
docker exec -it mysql-container bash
```

上述命令中，`mysql-container`是容器的名称。进入容器后，可以使用MySQL客户端连接到MySQL服务器，例如：

```bash
mysql -u root -p
```

然后输入设置的root密码，就可以进入MySQL命令行界面。

### 在容器中执行SQL命令

可以在容器外使用 `docker exec`命令在MySQL容器中执行SQL命令，例如，要在容器中创建一个名为 `mydb`的数据库，可以执行以下命令：

```bash
docker exec -it mysql-container mysql -u root -p -e "CREATE DATABASE mydb;"
```

在上述命令中，`-e`参数后面跟着要执行的SQL命令。执行该命令时，会提示输入root密码，输入正确密码后，就会在MySQL容器中创建名为 `mydb`的数据库。

### 备份和恢复数据

- **备份数据**：可以使用 `mysqldump`工具在容器外备份容器内的MySQL数据，例如：

```bash
docker exec -it mysql-container mysqldump -u root -p mydb > mydb_backup.sql
```

上述命令会将 `mydb`数据库的备份保存到主机当前目录下的 `mydb_backup.sql`文件中。

- **恢复数据**：要恢复备份的数据，可以先进入MySQL容器，然后使用 `mysql`命令导入备份文件，例如：

```bash
docker exec -it mysql-container bash
mysql -u root -p mydb < mydb_backup.sql
```

上述命令中，先进入MySQL容器的bash shell，然后使用 `mysql`命令将 `mydb_backup.sql`文件中的数据导入到 `mydb`数据库中。

## python 增删查改

### 安装pymysql

在使用 `pymysql`之前，需要先安装它。可以使用 `pip`命令进行安装：

```
pip install pymysql
```

### 连接数据库

```python
import pymysql

# 连接数据库
conn = pymysql.connect(
    host='localhost',
    user='root',
    password='your_password',
    database='your_database',
    charset='utf8mb4'
)
# 创建游标
cursor = conn.cursor()
```

### 增

```python
# 插入单条数据
sql_insert_single = "INSERT INTO your_table (column1, column2) VALUES (%s, %s)"
data_single = ('value1', 'value2')
cursor.execute(sql_insert_single, data_single)

# 插入多条数据
sql_insert_many = "INSERT INTO your_table (column1, column2) VALUES (%s, %s)"
data_many = [('value3', 'value4'), ('value5', 'value6')]
cursor.executemany(sql_insert_many, data_many)

# 提交事务
conn.commit()
```

### 删

```python
# 删除数据
sql_delete = "DELETE FROM your_table WHERE column1 = %s"
delete_value = 'value1'
cursor.execute(sql_delete, delete_value)
# 提交事务
conn.commit()
```

### 查

```python
# 查询数据
sql_select = "SELECT * FROM your_table WHERE column1 = %s"
select_value = 'value3'
cursor.execute(sql_select, select_value)
# 获取查询结果
results = cursor.fetchall()
for row in results:
    print(row)
```

### 改

```python
# 更新数据
sql_update = "UPDATE your_table SET column2 = %s WHERE column1 = %s"
update_data = ('new_value', 'value3')
cursor.execute(sql_update, update_data)
# 提交事务
conn.commit()
```

### 关闭连接

```python
# 关闭游标和连接
cursor.close()
conn.close()
```

在上述代码中，首先通过 `pymysql.connect()`方法建立与MySQL数据库的连接，然后创建游标对象。接着分别展示了插入（单条和多条）、删除、查询和更新数据的操作，每个操作完成后都需要通过 `conn.commit()`方法提交事务，以确保数据的持久化。最后，使用 `cursor.close()`和 `conn.close()`方法关闭游标和数据库连接。

# Redis

### 基本操作

- **拉取镜像**：从Docker镜像仓库拉取Redis镜像，命令为 `docker pull redis:latest`，会拉取最新版本的Redis镜像。若要拉取指定版本，将 `latest`替换为具体版本号，如 `docker pull redis:6.2`。
- **创建并运行容器**：使用 `docker run`命令创建并运行Redis容器，如 `docker run -d --name myredis -p 6379:6379 redis`，其中 `-d`表示在后台运行容器，`--name`指定容器名称为 `myredis`，`-p`将容器内部的6379端口映射到主机的6379端口，最后指定要运行的镜像为 `redis`。
- **进入容器**：进入正在运行的Redis容器，可使用 `docker exec`命令，如 `docker exec -it myredis redis-cli`，其中 `-it`表示以交互模式进入容器，`myredis`是容器名称，`redis-cli`是Redis的命令行客户端，用于在容器内执行Redis命令。
- **停止和启动容器**：停止容器可使用 `docker stop`命令，如 `docker stop myredis`；启动已停止的容器使用 `docker start`命令，如 `docker start myredis`。

### 数据持久化

- **使用数据卷**：创建容器时使用 `-v`参数挂载数据卷，将容器内的Redis数据目录映射到主机的指定目录，实现数据持久化。如 `docker run -d --name myredis -p 6379:6379 -v /data/redis:/data redis`，将容器内的 `/data`目录映射到主机的 `/data/redis`目录。
- **使用Docker Compose**：在 `docker-compose.yml`文件中定义Redis服务，配置数据卷挂载等。示例如下：

```yaml
version: '3'
services:
  redis:
    image: redis:latest
    container_name: myredis
    ports:
      - 6379:6379
    volumes:
      - /data/redis:/data
```

在项目目录下执行 `docker-compose up -d`即可启动Redis容器，实现数据持久化。

## python增删查改

在Python中，可以使用 `redis`库来操作Redis数据库，实现增删查改等功能。

### 连接Redis

```python
import redis

# 连接Redis服务器
r = redis.Redis(host='localhost', port=6379, decode_responses=True)
```

### 增

- **设置单个键值对**

```python
r.set('name', 'Alice')
```

- **设置多个键值对**

```python
data = {'age': 30, 'city': 'New York'}
r.mset(data)
```

### 删

- **删除单个键**

```python
r.delete('name')
```

- **删除多个键**

```python
r.delete('age', 'city')
```

### 查

- **获取单个键的值**

```python
name = r.get('name')
print(name)
```

- **获取多个键的值**

```python
keys = ['age', 'city']
result = r.mget(keys)
print(result)
```

- **遍历所有键**

```python
for key in r.scan_iter():
    print(key)
```

### 改

- **修改单个键的值**

```python
r.set('name', 'Bob')
```

- **使用自增/自减操作修改数值类型的值**

```python
# 先设置初始值
r.set('count', 5)
# 自增
r.incr('count')
# 自减
r.decr('count')
```

### 用户类示例

```
import redis

class UserRedis:
    """
    Redis User 表增删查改
    """
    def __init__(
        self,
        host="127.0.0.1",
        port=6379,
        db=0,
        max_connections=100,
        user_tabelname="user",
    ):
        # 创建连接池
        self.pool = redis.ConnectionPool(
            host=host, port=port, db=db, max_connections=max_connections
        )
        self.user_tabelname = user_tabelname

    def delete_user(self, user_id):
        """
        使用delete命令删除名为user:{user_id}的哈希表
        """
        try:
            r = redis.Redis(connection_pool=self.pool)
            r.delete(f"{self.user_tabelname}:{user_id}")
        except Exception as err:
            raise err

    def update_user(self, user_id, **kwargs):
        """
        使用hmset命令更新名为user:{user_id}的哈希表中的字段和值
        """

        try:
            r = redis.Redis(connection_pool=self.pool)
            r.hmset(f"{self.user_tabelname}:{user_id}", kwargs)
        except Exception as err:
            raise err

    def get_user(self, user_id):
        """
        使用hgetall命令获取名为user:{user_id}的哈希表中的所有字段和值
        """
        try:
            r = redis.Redis(connection_pool=self.pool)
            user_info = r.hgetall(f"{self.user_tabelname}:{user_id}")
            # 将字节类型的数据解码为字符串
            return {k.decode("utf-8"): v.decode("utf-8") for k, v in user_info.items()}
        except Exception as err:
            raise err

    def save_user(
        self,
        user_id,
        phone=None,

    ):
        """
        将用户信息存储到名为user:{user_id}的哈希表中
        login_state: out-离线， in-在线
        """
        # 使用连接池获取连接
        try:
            r = redis.Redis(connection_pool=self.pool)
            r.hset(
                f"{self.user_tabelname}:{user_id}",
                mapping={
                    "phone": phone,
                },
            )
        except Exception as err:
            raise err
```

# MongoDB

## Docker安装

### 基本操作

- **拉取镜像**：从Docker镜像仓库拉取MongoDB镜像，如 `docker pull mongo:latest`，会拉取最新版本的MongoDB镜像。若要拉取指定版本，将 `latest`替换为具体版本号即可。
- **创建并运行容器**：拉取镜像后，可使用 `docker run`命令创建并运行MongoDB容器。如 `docker run -d --name mymongo -p 27017:27017 mongo`，其中 `-d`表示在后台运行容器，`--name`指定容器名称为 `mymongo`，`-p`将容器内部的27017端口映射到主机的27017端口，最后指定要运行的镜像为 `mongo`。
- **进入容器**：如需进入正在运行的MongoDB容器，可使用 `docker exec`命令，如 `docker exec -it mymongo mongo`，其中 `-it`表示以交互模式进入容器，`mymongo`是容器名称，`mongo`是要在容器内执行的命令，这里是进入MongoDB的命令行客户端。
- **停止和启动容器**：停止容器可使用 `docker stop`命令，如 `docker stop mymongo`；启动已停止的容器使用 `docker start`命令，如 `docker start mymongo`。

### 数据持久化

- **使用数据卷**：可在创建容器时使用 `-v`参数挂载数据卷，将容器内的MongoDB数据目录映射到主机的指定目录，实现数据持久化。如 `docker run -d --name mymongo -p 27017:27017 -v /data/mongodb:/data/db mongo`，将容器内的 `/data/db`目录映射到主机的 `/data/mongodb`目录。
- **使用Docker Compose**：在 `docker-compose.yml`文件中定义MongoDB服务，配置数据卷挂载等。示例如下：

```yaml
version: '3'
services:
  mongo:
    image: mongo:latest
    container_name: mymongo
    ports:
      - 27017:27017
    volumes:
      - /data/mongodb:/data/db
```

在项目目录下执行 `docker-compose up -d`即可启动MongoDB容器，实现数据持久化。

## Win安装 MongoDB

### 安装包下载

官方下载地址：[`https://www.mongodb.com/download-center#community`](https://www.mongodb.com/download-center#community)``

**注意：奇数版本（如3.5）为开发版本，不适合生产用！**

官方安装文档：[`https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/`](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)``

下载安装包后，选择 `custom`安装，手动修改安装目录，如 `C:\MongoDB`后，一路 `next` ，安装过程记得取消安装 `compass`

**4.0以上版本出现无法启动服务报错，打开 `mongodb` 安装目录下 `mongodb/bin/mongodb.cfg` 删除最后一行的 `mp:`，打开任务管理器，找到服务 `MongoDB` 启动服务即可**

### 浏览器打开 `https://localhost:27017` 看到如下信息则表示安装成功并成功启动

`It looks like you are trying to access MongoDB over HTTP on the native driver port.`

## Linux 安装

以阿里云ECS为例:`https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/`

### 配置 yum 包管理系统

```
vim /etc/yum.repos.d/mongodb-org-3.6.repo
```

### 配置信息

```
[mongodb-org-4.0]
name = MongoDB Repository
baseurl = https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck = 1
enabled = 1
gpgkey = https://www.mongodb.org/static/pgp/server-4.0.asc
```

**注意：奇数版本（如3.5）为开发版本，不适合生产用！**

### 安装 MongoDB

```
yum install -y mongodb-org
```

### 启动 MongoDB 服务

```
service mongod start      #启动
service mongod stop       #停止
service mongod restart    #重启
```

### 当然，也可以配置 MongoDB 服务跟随系统启动

```
chkconfig mongod on
```

### 浏览器打开 [https://localhost:27017](https://localhost:27017/) 看到如下信息则表示安装成功并成功启动

`It looks like you are trying to access MongoDB over HTTP on the native driver port.`

## python 增删查改

在Python中可以使用 `pymongo`库来操作MongoDB数据库，实现增删查改等功能，以下是具体示例：

### 连接

- **安装pymongo**：在命令行中执行 `pip install pymongo`即可安装。
- **连接MongoDB**

```python
import pymongo

# 建立连接
client = pymongo.MongoClient("mongodb://localhost:27017/")
# 选择数据库
db = client["mydb"]
# 选择集合
collection = db["mycollection"]
```

### 增

- **插入单个文档**

```python
document = {"name": "John", "age": 30, "city": "New York"}
result = collection.insert_one(document)
print(result.inserted_id)
```

- **插入多个文档**

```python
documents = [
    {"name": "Alice", "age": 25, "city": "Paris"},
    {"name": "Bob", "age": 35, "city": "London"}
]
result = collection.insert_many(documents)
print(result.inserted_ids)
```

### 删

- **删除单个文档**

```python
query = {"name": "John"}
result = collection.delete_one(query)
print(result.deleted_count)
```

- **删除多个文档**

```python
query = {"age": {"$lt": 30}}
result = collection.delete_many(query)
print(result.deleted_count)
```

### 查

- **查询单个文档**

```python
query = {"name": "Alice"}
result = collection.find_one(query)
print(result)
```

- **查询多个文档**

```python
query = {"age": {"$gt": 25}}
results = collection.find(query)
for result in results:
    print(result)
```

- **投影查询**：指定返回的字段，不返回 `_id`字段

```python
query = {"city": "London"}
projection = {"_id": 0, "name": 1, "age": 1}
results = collection.find(query, projection)
for result in results:
    print(result)
```

### 改

- **更新单个文档**

```python
query = {"name": "Bob"}
update = {"$set": {"age": 40}}
result = collection.update_one(query, update)
print(result.modified_count)
```

- **更新多个文档**

```python
query = {"city": "Paris"}
update = {"$set": {"age": {"$inc": 5}}}
result = collection.update_many(query, update)
print(result.modified_count)
```

---
