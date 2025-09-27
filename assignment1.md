# In-Class Assessment 1

## (a) 三种主流云服务模型对比

| 缩写 | 全称 | 本质 | 开发者可控层级 | 典型产品 | 在软件开发中的落地举例 |
|----|------|------|----------------|----------|------------------------|
| IaaS | Infrastructure as a Service 基础设施即服务 | 提供裸金属或虚拟机、存储、网络 | 操作系统及以上 | AWS EC2、阿里云 ECS、腾讯云 CVM | 开一台 Ubuntu 22.04 云主机，手动装 Node.js + MySQL，部署博客系统 |
| PaaS | Platform as a Service 平台即服务 | 提供“运行时+中间件+数据库”完整平台 | 代码/容器镜像 | Heroku、Google App Engine、SAE | 直接把 Spring Boot 源码 push 到 Heroku，平台自动编译→运行→分配域名 |
| SaaS | Software as a Service 软件即服务 | 提供开箱即用的软件 | 仅应用层配置 | GitHub、钉钉、飞书、Salesforce | 使用 GitHub Issues 做需求管理，零运维即可获得高可用协作环境 |

## (b) Docker 与容器化详解

### 1. 什么是 Docker？
Docker 是一个开源的“容器引擎”，利用 Linux 内核的 cgroup & namespace 技术，把进程及其依赖打包成**轻量级、可移植、自包含**的容器镜像，保证“一次构建，到处运行”。

### 2. 典型开发场景
**背景**：小组 5 人开发 Python Flask + Redis + PostgreSQL 的电商秒杀 demo。  
**痛点**：
- 各成员电脑系统不一（Win11 / macOS / Ubuntu），Python 版本、PG 版本不同，常常出现“我这边没问题”。
- 测试小姐姐需要频繁在不同分支间切换，重新装依赖耗时 30 min+。
- 上线前要在 staging 环境再装一次，容易漏配参数。

**解决方案**：
- 编写 Dockerfile 把 Flask 代码与依赖固化为镜像 `myapp:1.0`。
- 编写 docker-compose.yml 一次性定义 3 个容器：web(Flask)、cache(Redis)、db(PG)。
- 开发者只需：
  ```bash
  git pull
  docker compose up -d