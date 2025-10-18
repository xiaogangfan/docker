# 服务连接信息

开发环境服务连接配置。

## 📋 服务列表

### MySQL
- **端口**: `3306`
- **用户名**: `root`
- **密码**: `root`
- **数据库**: `web3_asset`
- **连接命令**:
  ```bash
  mysql -h localhost -P 3306 -u root -proot web3_asset
  ```
- **连接字符串**:
  ```
  mysql://root:root@localhost:3306/web3_asset
  ```

### Redis
- **端口**: `6379`
- **密码**: 无
- **连接命令**:
  ```bash
  redis-cli -h localhost -p 6379
  ```
- **连接字符串**:
  ```
  redis://localhost:6379
  ```

### MongoDB
- **端口**: `27017`
- **用户名**: 无需认证
- **密码**: 无
- **数据库**: `web3_asset`
- **连接命令**:
  ```bash
  mongosh mongodb://localhost:27017/web3_asset
  ```
- **连接字符串**:
  ```
  mongodb://localhost:27017/web3_asset
  ```

### Kafka
- **宿主机端口**: `9092` (从本地连接)
- **Docker 内部端口**: `29092` (容器间通信)
- **宿主机连接**: `localhost:9092`
- **Docker 内部连接**: `kafka:29092`
- **UI 管理界面**: `http://localhost:8080`
- **说明**: Kafka 配置了两个监听器，支持从宿主机和 Docker 网络内部同时访问

### Nacos
- **端口**: `8848` (主服务端口)
- **gRPC 端口**: `9848`
- **Web 控制台**: `http://localhost:8848/nacos`
- **用户名**: `nacos`
- **密码**: `nacos`
- **命名空间**: `public` (默认)
- **数据存储**: 内置数据库 (Derby)
- **模式**: standalone (单机模式)
- **镜像**: `nacos/nacos-server:v2.3.1-slim` (官方 ARM64 支持版本)
- **说明**: 服务发现和配置管理平台，使用官方 slim 版本，完美支持 ARM64/Apple Silicon

## 🚀 快速启动

```bash
# 启动所有服务
docker-compose up -d

# 查看服务状态
docker-compose ps

# 停止所有服务
docker-compose down

# 停止并删除数据卷
docker-compose down -v
```

## 🔧 服务管理

```bash
# 重启单个服务
docker-compose restart mysql
docker-compose restart redis
docker-compose restart mongodb

# 查看服务日志
docker-compose logs mysql
docker-compose logs redis
docker-compose logs mongodb
docker-compose logs kafka

# 进入容器
docker exec -it mysql bash
docker exec -it redis sh
docker exec -it mongodb bash
```

## 📊 服务端口速查表

| 服务 | 端口 | 访问方式 | 认证信息 | 状态 |
|------|------|----------|----------|------|
| MySQL | 3306 | `localhost:3306` | root/root | ✅ 启用 |
| Redis | 6379 | `localhost:6379` | 无密码 | ✅ 启用 |
| MongoDB | 27017 | `localhost:27017` | 无认证 | ✅ 启用 |
| Kafka | 9092 | `localhost:9092` | 无认证 | ✅ 启用 |
| Kafka UI | 8080 | `http://localhost:8080` | 无认证 | ✅ 启用 |
| Nacos | 8848 | `http://localhost:8848/nacos` | nacos/nacos | ✅ 启用 (ARM64 兼容) |

## 💡 代码连接示例

### Go - MySQL
```go
import "gorm.io/driver/mysql"
import "gorm.io/gorm"

dsn := "root:root@tcp(localhost:3306)/web3_asset?charset=utf8mb4&parseTime=True&loc=Local"
db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
```

### Go - Redis
```go
import "github.com/redis/go-redis/v9"

rdb := redis.NewClient(&redis.Options{
    Addr: "localhost:6379",
})
```

### Go - MongoDB
```go
import "go.mongodb.org/mongo-driver/mongo"
import "go.mongodb.org/mongo-driver/mongo/options"

client, err := mongo.Connect(ctx, options.Client().ApplyURI("mongodb://localhost:27017"))
database := client.Database("web3_asset")
```

---

⚠️ **注意**: 此配置仅用于开发环境。生产环境请：
- 使用强密码
- 启用访问控制和身份验证
- 配置防火墙规则
- 使用 SSL/TLS 加密连接

