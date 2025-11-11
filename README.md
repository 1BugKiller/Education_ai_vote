# Education_ai_vote
This project aims to develop an online teaching assistance system involving multiple AIs to address current issues such as knowledge hallucination and insufficient professional capability in single LLM-based teaching assistance (referencing core pain points from relevant papers).


2025-11-11 22:27李振：
# Cool Admin 项目部署指南（快速上手版）
## 项目简介
基于 Cool Admin（Java 版）的后台权限管理系统，支持 Docker 一键部署，以下是从代码拉取到项目运行的完整流程，适用于团队快速搭建运行环境。

## 环境要求
| 工具           | 版本要求    | 说明                     |
|----------------|-------------|--------------------------|
| Docker         | 20.10+      | 容器化部署核心依赖       |
| Docker Compose | 2.10+       | 多容器编排工具           |
| Git            | 任意版本    | 用于拉取项目代码         |

## 核心步骤（从拉取到运行）
### 1. 拉取项目代码
打开终端/命令行，执行以下命令克隆代码并进入项目根目录：
```bash
# 克隆 GitHub 仓库代码（替换为你的仓库地址）
git clone https://github.com/[你的仓库地址]/Education_ai_vote.git

# 进入项目根目录
cd Education_ai_vote
```

### 2. 一键部署所有服务
项目已配置 Docker Compose 一键编排，执行以下命令即可自动构建并启动 MySQL、Redis、后端、前端所有服务：
```bash
# 构建并启动所有服务（首次部署会自动拉取镜像、建表、初始化数据）
docker compose up -d --build
```
- 执行说明：
  - 首次部署耗时约 5-10 分钟（取决于网络速度，需拉取 MySQL、Redis 等镜像）；
  - 后端会自动完成数据库表创建和基础数据初始化（管理员账号、菜单等），无需手动操作。

### 3. 验证服务启动状态
执行以下命令查看所有容器运行状态，确保服务正常启动：
```bash
# 查看容器状态
docker compose ps
```
- 正常结果：4 个容器（`mysql`、`redis`、`backend`、`frontend`）的 `STATUS` 均为 `Up`（表示运行中）。

### 4. 访问项目
#### （1）前端管理页面
- 访问地址：`http://localhost:9000`
- 默认账号：`admin`
- 默认密码：`123456`
- 说明：登录后即可进入系统后台，体验完整功能。

#### （2）后端服务验证
- 访问地址：`http://localhost:8001`
- 正常结果：页面显示 Cool Admin 启动成功提示（说明后端服务正常运行）。

#### （3）接口文档（开发用）
- 访问地址：`http://localhost:8001/swagger-ui.html`
- 说明：可查看所有后端接口详情，用于二次开发时接口对接。

## 基础操作命令（可选）
### 1. 停止项目
```bash
# 停止所有服务（数据会持久化，下次启动可恢复）
docker compose down
```

### 2. 重启项目
```bash
# 重启所有服务
docker compose restart
```

### 3. 查看服务日志（可选）
```bash
# 查看后端日志（如需确认启动状态）
docker compose logs -f backend

# 查看前端日志（如需排查访问问题）
docker compose logs -f frontend
```

## 二次开发入口（简要说明）
若需基于现有框架定制开发，核心流程如下：
1. 本地开发：分别打开 `cool_java/cool-admin-java`（后端）和 `cool_vue/cool-admin-vue`（前端）进行代码修改；
2. 本地测试：后端启动 `CoolApplication.java`，前端执行 `npm run dev` 启动本地开发环境；
3. 重新部署：修改完成后，回到项目根目录执行 `docker compose up -d --build [服务名]`（如 `backend`/`frontend`），即可更新容器内代码。

---

### 备注
- 首次部署默认使用 `local` 环境，自动建表+初始化数据，无需手动配置数据库；
- 项目端口、数据库密码等配置可在 `docker-compose.yml` 中修改，修改后重启服务即可生效。