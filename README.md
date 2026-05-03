# Flame 中文版

![首页截图](.github/home.png)

> 本项目是 [pawelmalak/flame](https://github.com/pawelmalak/flame) 的中文汉化版，界面全面翻译为简体中文。

## 简介

Flame 是一个自托管的服务器起始页，可以把它理解为**搭在自己服务器上的私人导航页**。你可以在上面添加常用网站的快捷方式、管理书签、搜索网页，还能显示天气。无需修改任何配置文件，全部通过网页界面操作。

## 功能

- 📝 通过内置图形界面直接创建、更新、删除应用和书签
- 📌 将常用项目固定到首页，快速访问
- 🔍 集成搜索栏，支持本地过滤、11 个网络搜索引擎及自定义搜索
- 🔑 认证系统，保护设置、应用和书签
- 🔨 丰富的界面定制选项，支持自定义 CSS、15 个内置主题及自定义主题构建器
- ☀️ 天气组件，显示当前温度、云量和动态天气状态
- 🐳 Docker 集成，自动根据容器标签添加应用

## 与官方版本的区别

| 项目 | 官方版 | 本中文版 |
|------|--------|---------|
| 界面语言 | 英文 | 简体中文 |
| Docker 镜像 | `pawelmalak/flame` | `wsng911/flame-zh` |
| 功能 | 完整 | 完整（仅汉化） |

---

## 安装教程（菜鸟版）

### 第一步：确认你有 Docker

打开终端（命令行），输入：

```bash
docker --version
```

如果显示版本号（如 `Docker version 24.0.0`）说明已安装，跳到第二步。

如果提示"找不到命令"，先安装 Docker：

```bash
# Linux 一键安装
curl -fsSL https://get.docker.com | sh
```

Windows / Mac 用户请下载 [Docker Desktop](https://www.docker.com/products/docker-desktop/)。

---

### 第二步：运行 Flame

复制下面这条命令，粘贴到终端执行：

```bash
docker run -d \
  -p 5005:5005 \
  -v /home/flame-data:/app/data \
  -e PASSWORD=my_password \
  --name flame \
  --restart unless-stopped \
  wsng911/flame-zh:latest
```

**参数说明：**

| 参数 | 说明 |
|------|------|
| `-p 5005:5005` | 把容器的 5005 端口映射到本机，可改成 `-p 8080:5005` 换端口 |
| `-v /home/flame-data:/app/data` | 数据保存位置，换成你想存的路径 |
| `-e PASSWORD=my_password` | 登录密码，改成你自己的密码 |
| `--restart unless-stopped` | 服务器重启后自动启动 |

执行后等几秒，然后打开浏览器访问：

```
http://你的服务器IP:5005
```

本机访问用：`http://localhost:5005`

---

### 第三步：登录

1. 点击页面右上角「**前往设置**」
2. 点击顶部「**应用**」标签页
3. 输入你设置的密码，点「**登录**」

---

## 使用教程

### 添加应用（首页大图标）

应用会显示在首页的图标网格里，适合放常用工具。

1. 点击首页右下角的 ⚙️ 齿轮图标
2. 进入「**全部应用**」页面，点「**添加**」
3. 填写信息：
   - **应用名称**：显示在图标下方的名字，如 `GitHub`
   - **应用地址**：网站地址，如 `https://github.com`
   - **应用图标**：输入 MDI 图标名，如 `github`（[点击查看所有图标](https://pictogrammers.com/library/mdi/)）
4. 点击提交后，回到首页，鼠标悬停在图标上，点击 📌 图钉固定到首页

> 💡 图标名技巧：直接输入网站名称小写，大多数常见网站都有对应图标，如 `google`、`youtube`、`github`、`netflix`

---

### 添加书签（分类链接列表）

书签适合收藏大量链接，按分类整理。

**第一步：创建分类**

1. 点击首页右下角齿轮 → 进入「**全部书签**」
2. 点「**添加分类**」，输入分类名（如：`工作`、`娱乐`、`工具`）
3. 提交后点 📌 图钉固定到首页

**第二步：添加书签**

1. 点「**添加书签**」
2. 填写书签名称、地址，选择所属分类
3. 提交即可

---

### 更换主题

1. 进入「**设置**」→「**主题**」标签页
2. 点击任意主题卡片即可切换
3. 也可以点「**用户主题**」→「**添加主题**」自定义颜色

---

### 设置搜索引擎

1. 进入「**设置**」→「**通用**」
2. 在「**主要搜索引擎**」下拉框选择你喜欢的搜索引擎
3. 点「**保存设置**」

**搜索快捷前缀：**

在搜索框输入前缀可切换搜索引擎：

| 前缀 | 搜索引擎 |
|------|---------|
| `/g` | Google |
| `/b` | Bing |
| `/d` | DuckDuckGo |
| `/y` | YouTube |
| `/r` | Reddit |

例如输入 `/y 猫咪视频` 直接在 YouTube 搜索。

---

### 设置天气

1. 去 [weatherapi.com](https://www.weatherapi.com/pricing.aspx) 免费注册，获取 API Key
2. 进入「**设置**」→「**天气**」
3. 填入 API Key
4. 填入你所在城市的经纬度（可在 [latlong.net](https://www.latlong.net/) 查询，或点「点击获取当前位置」）
5. 选择温度单位（摄氏度）
6. 点「**保存设置**」

---

### 常用管理命令

```bash
# 查看运行状态
docker ps

# 停止 Flame
docker stop flame

# 启动 Flame
docker start flame

# 查看日志（排查问题用）
docker logs flame

# 更新到最新版
docker stop flame && docker rm flame
docker pull wsng911/flame-zh:latest
docker run -d -p 5005:5005 -v /home/flame-data:/app/data -e PASSWORD=my_password --name flame --restart unless-stopped wsng911/flame-zh:latest
```

---

## Docker Compose 方式（推荐长期使用）

创建 `docker-compose.yml` 文件：

```yaml
version: '3.6'

services:
  flame:
    image: wsng911/flame-zh:latest
    container_name: flame
    ports:
      - 5005:5005
    volumes:
      - ./data:/app/data
    environment:
      - PASSWORD=my_password
    restart: unless-stopped
```

然后执行：

```bash
docker compose up -d
```

---

## 截图

![应用截图](.github/apps.png)

![书签截图](.github/bookmarks.png)

![设置截图](.github/settings.png)

![主题截图](.github/themes.png)

---

## 致谢

- 原项目：[pawelmalak/flame](https://github.com/pawelmalak/flame)
- 原项目许可证：ISC
