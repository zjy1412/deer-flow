# DeerFlow Docker 部署指南

本指南将帮助您在Linux服务器上使用Docker部署DeerFlow项目。

## 前提条件

确保您的服务器上已安装以下软件：

- Docker (20.10.0+)
- Docker Compose (2.0.0+)

可以使用以下命令安装Docker和Docker Compose：

```bash
# 安装Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# 安装Docker Compose
sudo apt-get update
sudo apt-get install docker-compose-plugin
```

## 部署步骤

### 1. 克隆项目

```bash
git clone https://github.com/bytedance/deer-flow.git
cd deer-flow
```

### 2. 配置环境变量

创建并编辑`.env`文件：

```bash
cp .env.example .env
nano .env
```

根据您的需求修改环境变量，特别是API密钥：

```
# 必须配置的项目
TAVILY_API_KEY=your_tavily_api_key

# 可选配置
# VOLCENGINE_TTS_APPID=xxx
# VOLCENGINE_TTS_ACCESS_TOKEN=xxx
```

### 3. 配置LLM模型

创建并编辑`conf.yaml`文件：

```bash
cp conf.yaml.example conf.yaml
nano conf.yaml
```

根据您的需求配置LLM模型，例如：

```yaml
BASIC_MODEL:
  base_url: "https://api.openai.com/v1"
  model: "gpt-4o"
  api_key: "your_openai_api_key"
```

### 4. 构建和启动Docker容器

```bash
docker-compose up -d
```

这将在后台启动后端和前端服务。

### 5. 访问应用

应用将在以下地址可用：

- Web UI: http://your-server-ip:3000
- API: http://your-server-ip:8000

## 常见问题排查

### 查看日志

```bash
# 查看后端日志
docker-compose logs backend

# 查看前端日志
docker-compose logs frontend

# 实时查看所有日志
docker-compose logs -f
```

### 重启服务

```bash
docker-compose restart
```

### 停止服务

```bash
docker-compose down
```

### 更新应用

当有新版本时，可以按照以下步骤更新：

```bash
# 拉取最新代码
git pull

# 重新构建并启动容器
docker-compose up -d --build
```

## 高级配置

### 使用外部LLM服务

如果您想使用本地Ollama模型，可以在`conf.yaml`中配置：

```yaml
BASIC_MODEL:
  model: "ollama/llama3"
  base_url: "http://host.docker.internal:11434"
```

注意：在Docker环境中访问宿主机服务需要使用`host.docker.internal`而不是`localhost`。

### 自定义端口

如果您想更改默认端口，可以编辑`docker-compose.yml`文件中的端口映射：

```yaml
ports:
  - "8080:8000"  # 将后端端口从8000改为8080
  - "80:3000"    # 将前端端口从3000改为80
```

## 安全注意事项

- 不要将包含API密钥的配置文件提交到版本控制系统
- 考虑在生产环境中使用HTTPS
- 限制Docker容器的资源使用（CPU、内存）

## 资源需求

DeerFlow在Docker环境中的最低资源需求：

- CPU: 2核
- 内存: 4GB
- 磁盘空间: 2GB

推荐配置：

- CPU: 4核
- 内存: 8GB
- 磁盘空间: 10GB
