# Lady Station

> 磁力链接管理与云盘离线下载服务

Lady Station 是一个功能完善的磁力链接管理系统，支持多数据源同步、CloudDrive2 云盘离线下载、定时任务调度等功能。

## ✨ 功能特性

### 📊 磁力管理
- 磁力链接搜索、筛选、分页浏览
- 按日期、分类、来源多维度筛选
- 支持复制磁力链接、一键离线下载

### 🔄 数据源同步
- 支持多数据源配置（PostgreSQL 直连 / LS-API）
- 主备数据源自动故障切换
- 手动同步 / 定时自动同步
- 增量同步，避免重复数据

### ☁️ 云盘离线
- 集成 CloudDrive2 实现云盘离线下载
- 支持阿里云盘、115网盘等多种云盘
- 自动创建分类/日期目录结构
- 手动离线 / 定时自动离线
- 配额检查，避免超限

### 🕷️ 爬虫功能
- 支持多站点数据爬取
- 手动爬取 / 定时自动爬取
- 可配置爬取参数

### 🔔 通知推送
- 企业微信通知
- Telegram Bot 通知
- 任务执行结果实时推送

### 🔐 安全特性
- API Key 只读认证（查询/同步/统计）
- 用户登录认证
- 敏感信息脱敏显示

## 🛠️ 技术栈

### 后端
- Python 3.12+
- FastAPI
- SQLAlchemy 2.0 (异步)
- SQLite (本地缓存)
- APScheduler (定时任务)

### 前端
- React 18
- TypeScript
- Vite
- Tailwind CSS
- TanStack Query

## 🚀 快速开始

### Docker 部署（推荐）

```bash
# 拉取镜像
docker pull dahai123/lady-station:latest

# 运行容器（生产环境，数据持久化）
docker run -d --name lady-station -p 8001:8000 \
  -v /path/to/data:/data \
  dahai123/lady-station:latest
```

访问 `http://localhost:8000`，默认账号：`admin` / `admin123`

### 本地开发

> **注意**：本地开发环境直接运行 Python 源码，无需编译。生产环境（Docker）会将 Python 代码编译为 .so 文件。

```bash
# 克隆项目
git clone https://github.com/dahai1881/lady-station.git
cd lady-station

# 安装后端依赖
pip install -r requirements.txt

# 安装前端依赖
cd frontend && npm install

# 方式一：开发模式（推荐，前后端分离运行）
# 使用开发脚本同时启动后端和前端
./start_dev.sh
# 后端运行在 http://localhost:8000
# 前端运行在 http://localhost:3000（Vite 开发服务器）

# 方式二：生产模式（前端构建后由后端服务）
# 构建前端
cd frontend && npm run build && cd ..

# 启动服务（前端静态文件由后端服务）
python start.py
```

## 📁 项目结构

> **说明**：本项目在生产环境（Docker）中会将 Python 代码编译为 .so 文件，使用 `build.py` 进行 Cython 编译。

```
lady-station/
├── app/                      # 后端代码（开发环境为 .py，生产环境编译为 .so）
│   ├── api/                  # API 路由聚合（v1/endpoints）
│   ├── core/                 # 配置、鉴权、日志、启动等核心能力
│   ├── db/                   # 数据库会话与迁移入口
│   ├── modules/              # 业务模块（services/repositories/usecases）
│   ├── integrations/         # 第三方适配器
│   ├── chain/                # 跨模块流程编排
│   ├── plugins/              # 插件入口
│   ├── shared/               # 共享类型/模型/ports
│   └── startup/              # 启动装配
├── frontend/                 # 前端代码
│   └── src/
│       ├── features/         # 业务域功能
│       ├── pages/            # 页面入口
│       ├── components/       # 通用组件
│       ├── api/              # API 调用
│       └── types/            # TypeScript 类型
├── data/                     # 数据目录
├── logs/                     # 日志目录
├── build.py                  # Python 代码编译脚本（Cython）
├── start.py                  # 生产环境启动入口
├── start_dev.sh              # 开发环境启动脚本（前后端分离）
├── Dockerfile                # Docker 构建文件（多阶段构建，包含编译步骤）
└── docker-compose.yml        # Docker Compose 配置
```

### 编译说明

- **开发环境**：直接运行 Python 源码（`.py` 文件），无需编译
- **生产环境**：使用 `build.py` 将 Python 代码编译为 `.so` 文件（Cython），提升性能并保护源码
- **Docker 构建**：Dockerfile 采用多阶段构建，自动完成前端构建和后端编译

## 🔧 配置说明

### 数据源配置
支持两种模式：
- **PostgreSQL 直连**：直接连接 PostgreSQL 数据库
- **LS-API**：通过 API 连接其他 Lady Station 实例

### CD2 配置
- URL：CloudDrive2 服务地址
- 用户名/密码：CD2 登录凭据
- 保存路径：离线文件保存目录
- 目录结构：分类/日期 或 日期/分类

### 通知配置
- 企业微信：需配置 CorpID、Secret、AgentID
- Telegram：需配置 Bot Token、Chat ID

## 🔌 API 文档

启动服务后访问 `http://localhost:8000/docs` 查看 Swagger API 文档。

主要接口：
- `GET /api/v1/magnets` - 查询磁力链接
- `GET /api/v1/magnets/sync` - 游标分页同步（对外只读）
- `POST /api/v1/sync/trigger` - 触发同步
- `POST /api/v1/transfer/trigger` - 触发离线
- `GET /api/v1/config/*` - 配置管理

对外只读 API Key 的可用接口与示例请参考 `docs/api/API.md`。

## 📄 License

MIT License

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！
