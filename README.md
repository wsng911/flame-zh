# Flame 中文版

![首页截图](.github/home.png)

> 本项目是 [pawelmalak/flame](https://github.com/pawelmalak/flame) 的中文汉化版，界面全面翻译为简体中文。

## 简介

Flame 是一个自托管的服务器起始页，设计灵感来自 [SUI](https://github.com/jeroenpardon/sui)。使用简单，内置编辑器，无需修改任何文件即可快速搭建专属导航主页。

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
| Docker 镜像 | `pawelmalak/flame` | 需自行构建 |
| 功能 | 完整 | 完整（仅汉化） |

## 安装

### 使用 Docker（推荐）

```sh
# 克隆本仓库
git clone https://github.com/wsng911/flame-zh.git
cd flame-zh

# 构建镜像
docker build -t flame-zh .

# 运行容器
docker run -d \
  -p 5005:5005 \
  -v /path/to/data:/app/data \
  -e PASSWORD=your_password \
  --name flame \
  flame-zh
```

访问 `http://your-server-ip:5005`，默认密码为 `flame_password`。

## 使用说明

1. 打开首页，点击右上角「前往设置」
2. 在「应用」标签页输入密码登录
3. 回到首页，点击右下角齿轮图标进入应用管理
4. 添加应用或书签，点击图钉固定到首页

## 致谢

- 原项目：[pawelmalak/flame](https://github.com/pawelmalak/flame)
- 原项目许可证：ISC
  -f .docker/Dockerfile.multiarch \
  -t flame:multiarch .
```

#### Docker-Compose

```yaml
version: '3.6'

services:
  flame:
    image: pawelmalak/flame
    container_name: flame
    volumes:
      - /path/to/host/data:/app/data
      - /var/run/docker.sock:/var/run/docker.sock # optional but required for Docker integration
    ports:
      - 5005:5005
    secrets:
      - password # optional but required for (1)
    environment:
      - PASSWORD=change_me
      - PASSWORD_FILE=/run/secrets/password # optional but required for (1)
    restart: unless-stopped

# optional but required for Docker secrets (1)
secrets:
  password:
    file: /path/to/secrets/password
```

##### Docker Secrets

All environment variables can be overwritten by appending `_FILE` to the variable value. For example, you can use `PASSWORD_FILE` to pass through a docker secret instead of `PASSWORD`. If both `PASSWORD` and `PASSWORD_FILE` are set, the docker secret will take precedent.

```bash
# ./secrets/flame_password
my_custom_secret_password_123

# ./docker-compose.yml
secrets:
  password:
    file: ./secrets/flame_password
```

#### Skaffold

```sh
# use skaffold
skaffold dev
```

### Without Docker

Follow instructions from wiki: [Installation without Docker](https://github.com/pawelmalak/flame/wiki/Installation-without-docker)

## Development

### Technology

- Backend
  - Node.js + Express
  - Sequelize ORM + SQLite
- Frontend
  - React
  - Redux
  - TypeScript
- Deployment
  - Docker
  - Kubernetes

### Creating dev environment

```sh
# clone repository
git clone https://github.com/pawelmalak/flame
cd flame

# run only once
npm run dev-init

# start backend and frontend development servers
npm run dev
```

## Screenshots

![Apps screenshot](.github/apps.png)

![Bookmarks screenshot](.github/bookmarks.png)

![Settings screenshot](.github/settings.png)

![Themes screenshot](.github/themes.png)

## Usage

### Authentication

Visit [project wiki](https://github.com/pawelmalak/flame/wiki/Authentication) to read more about authentication

### Search bar

#### Searching

The default search setting is to search through all your apps and bookmarks. If you want to search using specific search engine, you need to type your search query with selected prefix. For example, to search for "what is docker" using google search you would type: `/g what is docker`.

For list of supported search engines, shortcuts and more about searching functionality visit [project wiki](https://github.com/pawelmalak/flame/wiki/Search-bar).

### Setting up weather module

1. Obtain API Key from [Weather API](https://www.weatherapi.com/pricing.aspx).
   > Free plan allows for 1M calls per month. Flame is making less then 3K API calls per month.
2. Get lat/long for your location. You can get them from [latlong.net](https://www.latlong.net/convert-address-to-lat-long.html).
3. Enter and save data. Weather widget will now update and should be visible on Home page.

### Docker integration

In order to use the Docker integration, each container must have the following labels:

```yml
labels:
  - flame.type=application # "app" works too
  - flame.name=My container
  - flame.url=https://example.com
  - flame.icon=icon-name # optional, default is "docker"
# - flame.icon=custom to make changes in app. ie: custom icon upload
```

> "Use Docker API" option must be enabled for this to work. You can find it in Settings > Docker

You can also set up different apps in the same label adding `;` between each one.

```yml
labels:
  - flame.type=application
  - flame.name=First App;Second App
  - flame.url=https://example1.com;https://example2.com
  - flame.icon=icon-name1;icon-name2
```

If you want to use a remote docker host follow this instructions in the host:

- Open the file `/lib/systemd/system/docker.service`, search for `ExecStart` and edit the value

```text
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:${PORT} -H unix:///var/run/docker.sock
```

>The above command will bind the docker engine server to the Unix socket as well as TCP port of your choice. “0.0.0.0” means docker-engine accepts connections from all IP addresses.

- Restart the daemon and Docker service

```shell
sudo systemctl daemon-reload
sudo service docker restart
```

- Test if it is working

```shell
curl http://${IP}:${PORT}/version
```

### Kubernetes integration

In order to use the Kubernetes integration, each ingress must have the following annotations:

```yml
metadata:
  annotations:
  - flame.pawelmalak/type=application # "app" works too
  - flame.pawelmalak/name=My container
  - flame.pawelmalak/url=https://example.com
  - flame.pawelmalak/icon=icon-name # optional, default is "kubernetes"
```

> "Use Kubernetes Ingress API" option must be enabled for this to work. You can find it in Settings > Docker

### Import HTML Bookmarks (Experimental)

- Requirements
  - python3
  - pip packages: Pillow, beautifulsoup4
- Backup your `db.sqlite` before running script!
- Known Issues:
  - generated icons are sometimes incorrect

```bash
pip3 install Pillow, beautifulsoup4

cd flame/.dev
python3 bookmarks_importer.py --bookmarks <path to bookmarks.html> --data <path to flame data folder>
```

### Custom CSS and themes

See project wiki for [Custom CSS](https://github.com/pawelmalak/flame/wiki/Custom-CSS) and [Custom theme with CSS](https://github.com/pawelmalak/flame/wiki/Custom-theme-with-CSS).
