# 一、docker基本操作

## 简单示例

```dockerfile
# 使用官方的 Go 镜像作为基础镜像
FROM golang:1.18-alpine

# 设置维护者信息
LABEL maintainer="your_email@example.com"

# 设置工作目录
WORKDIR /app

# 复制当前目录内容到工作目录
COPY . .

# 编译 Go 应用程序
RUN go build -o myapp

# 暴露端口
EXPOSE 8080

# 运行应用程序
CMD ["./myapp"]
```

- `FROM`: 指定构建镜像所基于的基础镜像。例如，`FROM ubuntu:20.04` 表示使用 Ubuntu 20.04 作为基础镜像。
- `LABEL`: 添加元数据，例如维护者信息。
- `ENV`: 设置环境变量。
- `RUN`: 在镜像内执行命令。通常用于安装软件包或者进行其他配置。
- `COPY` 或 `ADD`: 将文件或目录从主机复制到镜像中。
- `WORKDIR`: 设置工作目录。后续的 `RUN`、`CMD`、`ENTRYPOINT`、`COPY` 和 `ADD` 指令将会在这个目录下执行。
- `EXPOSE`: 声明容器将在运行时监听的端口。
- `CMD`: 指定容器启动时执行的命令。每个 Dockerfile 只能有一个 `CMD`，如果指定多个，只有最后一个生效。

## golang多段构建过程

```dockerfile
# 使用多阶段构建
# 第一阶段：构建阶段
FROM golang:alpine AS builder

# 安装 git（如果需要拉取私有依赖）
RUN apk add --no-cache git

# 设置工作目录
WORKDIR /app

# 将 go.mod 和 go.sum 文件复制到工作目录
COPY go.mod .
COPY go.sum .

# 下载依赖
RUN go mod download

# 复制剩余的应用程序代码
COPY . .

# 整理依赖
RUN go mod tidy

# 构建应用程序
RUN go build -o myapp .

# 第二阶段：运行阶段
FROM alpine:latest

# 安装运行所需的依赖
RUN apk add --no-cache ca-certificates

# 设置工作目录
WORKDIR /root/

# 从构建阶段复制构建好的应用程序
COPY --from=builder /app/myapp .

# 指定容器启动时运行的命令
CMD ["./myapp"]

```



# 二、docker-compose

## 简单示例

```yaml
services:
  web:
    build: .
    ports:
      - "8000:5000"
    develop:
      watch:
        - action: sync
          path: .
          target: /code
  redis:
    image: "redis:alpine"
```

# 三、go微服务分段式部署

todo



