# Redis Cluster with Docker Compose

## 仓库说明

本仓库的核心目的是帮助开发者在Windows本地环境下，使用基于WSL2的Docker快速创建一个本地Redis集群，用于本地开发和调试。

该配置方案解决了Windows Docker环境下Redis集群的网络通信问题，提供了完整的配置指南和使用说明，确保开发者能够快速搭建可用的Redis集群环境。

## Windows Docker 配置注意事项

为了确保Redis集群能够正常工作，需要在Windows上进行以下Docker和WSL配置：

### 1. Docker 设置
- 打开 Docker Desktop
- 进入 Settings
- 选择 Resources
- 勾选 "Enable host networking"

### 2. WSL 设置
- 打开 Windows Subsystem for Linux (WSL) 设置
- 开启 "主机地址环回" 选项
- 将网络模式更改为 "Mirrored"
- 启用 "localhost 转发"

## 使用说明

### 启动集群
```bash
docker-compose up -d
```

### 停止集群
```bash
docker-compose down
```

### 手动创建redis集群
```bash
docker exec -it redis-1 sh -c "REDISCLI_AUTH=<REDIS_PASSWORD> redis-cli --cluster create <宿主机IP>:7001 <宿主机IP>:7002 <宿主机IP>:7003 <宿主机IP>:7004 <宿主机IP>:7005 <宿主机IP>:7006 --cluster-replicas 1 --cluster-yes"
```
将 `<宿主机IP>` 替换为您的实际宿主机IP地址，例如 `192.168.0.154`
将 `<REDIS_PASSWORD>` 替换为您的实际Redis密码

### 查看集群状态
```bash
docker exec -it redis-1 redis-cli -a <REDIS_PASSWORD> cluster nodes
```

### 查看集群信息
```bash
docker exec -it redis-1 redis-cli -a <REDIS_PASSWORD> cluster info
```

## 集群配置

- 3个主节点 (redis1, redis2, redis3)
- 3个从节点 (redis4, redis5, redis6)
- 每个节点都有独立的端口映射
- 使用密码认证保护