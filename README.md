# 基于微服务架构的在线教育平台

> 毕业设计作品

## 项目简介

本项目是一个基于 **Spring Cloud 微服务架构**的在线教育平台，融合 **AI 智能助手**创新功能，实现了前台学习系统、后台管理系统、视频课程学习、在线支付、AI对话问答等完整功能。

## 技术栈

### 后端
| 技术 | 版本 | 说明 |
|------|------|------|
| Spring Boot | 3.2.3 | 基础框架 |
| Spring Cloud | 2023.0.0 | 微服务框架 |
| Spring Cloud Alibaba | 2023.0.1.0 | Nacos + Sentinel |
| Nacos | 2.3.1 | 服务注册/配置中心 |
| Spring Cloud Gateway | - | API网关 |
| MyBatis Plus | 3.5.7 | ORM框架 |
| MySQL | 8.0 | 关系型数据库 |
| Redis | 7 | 缓存/Session |
| JWT | 0.12.3 | Token认证 |
| MinIO | 8.5.9 | 对象存储（视频） |
| 支付宝 SDK | - | 在线支付 |
| 通义千问 API | - | AI对话引擎 |

### 前端
| 技术 | 版本 | 说明 |
|------|------|------|
| Vue 3 | 3.4+ | 前端框架 |
| TypeScript | 5.4+ | 类型安全 |
| Vite | 5.1+ | 构建工具 |
| Element Plus | 2.6+ | UI组件库 |
| Pinia | 2.1+ | 状态管理 |
| Vue Router | 4.3+ | 路由管理 |
| Video.js | 8.10+ | 视频播放器 |
| ECharts | 5.4+ | 数据可视化 |
| Axios | 1.6+ | HTTP请求 |

## 项目结构

```
online-edu/
├── edu-common/              # 公共模块（工具类、实体、异常处理）
├── edu-gateway/             # API网关服务（端口：8080）
├── edu-service/
│   ├── edu-user/            # 用户服务（端口：8081）
│   ├── edu-course/          # 课程服务（端口：8082）
│   ├── edu-order/           # 订单服务（端口：8083）
│   ├── edu-ai/              # AI助手服务（端口：8084）
│   └── edu-stat/            # 统计服务（端口：8085）
├── edu-web/                 # 前台（Vue3，端口：3000）
├── edu-admin/               # 后台管理（Vue3，端口：3001）
├── sql/
│   └── edu_all.sql          # 数据库初始化脚本
└── docker-compose.yml       # Docker一键部署
```

## 系统功能

### 一、前台用户系统
- **认证**：手机号注册/登录、密码登录、短信验证码登录、JWT Token
- **首页**：轮播图、课程分类导航、推荐/最新/热门课程展示
- **课程**：多条件筛选（分类/价格/评分）、关键词搜索、课程详情
- **学习**：免费直接观看、收费购买后观看、Video.js播放器、进度记忆
- **互动**：评论/评分/收藏、个人中心、学习进度跟踪
- **支付**：支付宝在线支付（沙箱环境）

### 二、后台管理系统
- **权限**：管理员角色校验、JWT鉴权
- **数据看板**：注册用户/课程/订单/收入统计，ECharts折线图/饼图
- **课程管理**：发布/下架/编辑，大纲章节，视频管理
- **内容运营**：讲师管理、课程分类（支持树形展示）、轮播图增删改查
- **用户管理**：会员列表、禁用/启用
- **订单管理**：订单列表、状态跟踪、支付记录

### 三、AI对话智能助手（创新亮点）
- **课程推荐**：根据兴趣描述，AI智能推荐课程方向
- **学习计划**：输入课程名称、目标时间，AI生成个性化计划
- **随课答疑**：视频学习过程中唤起AI助手提问
- **内容总结**：自动总结章节要点
- **对话历史**：记录历史对话，支持多轮对话
- **技术实现**：通义千问 DashScope API + Spring WebFlux SSE流式输出 + Redis缓存 + MySQL存储

## 快速启动

### 方式一：Docker Compose（推荐）

```bash
# 1. 启动所有基础设施和服务
docker-compose up -d

# 2. 等待约2分钟后访问：
# 前台：http://localhost:3000
# 后台：http://localhost:3001/admin
# Nacos控制台：http://localhost:8848/nacos (nacos/nacos)
# MinIO控制台：http://localhost:9001 (minioadmin/minioadmin)
```

### 方式二：本地开发环境

#### 1. 启动基础设施

```bash
# MySQL（需要先创建数据库并导入SQL）
mysql -uroot -p123456 < sql/edu_all.sql

# Redis
redis-server

# Nacos
# 下载 nacos-server-2.3.1，启动
startup.cmd -m standalone  # Windows
```

#### 2. 启动后端微服务（按顺序）

```bash
# 用 IDEA 或 Maven 分别启动：
# 1. edu-gateway（网关先启动）
# 2. edu-user
# 3. edu-course
# 4. edu-order
# 5. edu-ai
```

#### 3. 启动前端

```bash
# 前台
cd edu-web
npm install
npm run dev
# 访问：http://localhost:3000

# 后台管理
cd edu-admin
npm install
npm run dev
# 访问：http://localhost:3001
```

## 默认账号

| 角色 | 账号 | 密码 |
|------|------|------|
| 管理员 | admin | admin123456 |
| 普通用户 | test001 | 123456 |
| 测试手机号 | 13800000001 | 短信码：888888 |

## AI助手配置

在 `edu-service/edu-ai/src/main/resources/application.yml` 中配置通义千问 API Key：

```yaml
ai:
  dashscope:
    api-key: YOUR_API_KEY  # 替换为你的 DashScope API Key
```

> 申请地址：https://dashscope.aliyuncs.com/
> 
> 未配置时，系统会使用内置的模拟回复，功能仍可正常演示。

## 接口文档

启动各服务后，访问以下地址查看 Swagger 文档：
- 用户服务：http://localhost:8081/doc.html
- 课程服务：http://localhost:8082/doc.html
- 订单服务：http://localhost:8083/doc.html
- AI服务：http://localhost:8084/doc.html

## 架构图

```
浏览器/移动端
     │
     ▼
┌─────────────────────────────────┐
│       API Gateway (8080)         │  JWT鉴权 + 路由转发 + 跨域
└─────────────────────────────────┘
     │          │          │
     ▼          ▼          ▼
  用户服务    课程服务    订单服务    AI服务
  (8081)     (8082)     (8083)    (8084)
     │          │          │          │
     └──────────┴──────────┴──────────┘
                      │
              ┌───────┴────────┐
              │                │
            MySQL           Redis
         (多数据库)         (缓存/会话)
              │
           Nacos
        (注册/配置中心)
```

---


