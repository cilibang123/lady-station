# Lady Station

> 榜单、磁力采集、磁力推送、订阅、站点资源搜索一体化服务

---

## 项目简介

Lady Station 是一个前后端一体化服务，聚焦资源发现、采集、推送与订阅自动化。

---

## ✨ 核心能力

### 📊 榜单

- 榜单刷新与浏览
- 榜单自动订阅
- 订阅状态回显

### 🕷️ 磁力采集

- 多来源采集
- 手动触发与定时执行
- 磁力标准化与去重

### 🚀 磁力推送

- 规则化推送到下载器/离线路径
- 默认下载器与自定义保存路径
- 推送结果通知

### 📌 订阅

- 订阅规则配置
- 自动执行与回退策略
- 与榜单/采集/推送联动

### 🔎 站点资源搜索

- 站点级搜索与筛选
- 搜索结果直达推送/订阅

---

## 镜像信息

- 📦 DockerHub 镜像：`xyp198988/lady-station`
- 推荐标签：`latest`
- 容器内服务端口：`8000`
- 数据目录：`/data`

---

## 🚀 部署方式一：`docker run`（最快）

```bash
docker pull xyp198988/lady-station:latest

docker run -d \
  --name lady-station \
  --restart always \
  -p 8123:8000 \
  -v /your/path/lady-station/data:/data \
  -e TZ=Asia/Shanghai \
  -e LICENSE_KEY=你获取到的KEY \
  xyp198988/lady-station:latest
```

启动后访问：`http://<你的服务器IP>:8123`

默认账号：`admin` / `admin123`

---

## 🧩 部署方式二：Docker Compose（推荐长期运行）

先准备目录：

```bash
mkdir -p lady-station/data
cd lady-station
```

创建 `docker-compose.yml`：

```yaml
services:
  lady-station:
    image: xyp198988/lady-station:latest
    container_name: lady-station
    restart: always
    ports:
      - "8123:8000"
    volumes:
      - ./data:/data
    environment:
      - TZ=Asia/Shanghai
      - LICENSE_KEY=你获取到的KEY
```

启动与查看：

```bash
docker compose up -d
docker compose ps
docker compose logs -f lady-station
```

---

## 🔄 升级方式

```bash
docker compose pull
docker compose up -d
```

如果是 `docker run` 方式，建议先备份 `data/` 再重建容器。

---

## 🌐 访问地址

- Web 页面：`http://<IP>:8123`

---

## 项目链接

- 🐙 [GitHub](https://github.com/cilibang123/lady-station)
- 💬 [Telegram 交流群](https://t.me/+bG-XdlWLIZJmZDJh)


欢迎加入 Telegram 群组交流使用问题、功能建议与部署经验。

---
