# 李家傲

Java 后端开发 / agent 开发

电话：+86 13181304035｜邮箱：jaffreyli197@gmail.com｜性别：男｜年龄：27

![个人照片](photo.jpg)

## 教育背景

西安建筑科技大学华清学院｜本科 / 计算机科学与技术｜2017.09--2021.07

西澳大利亚大学（QS77）｜硕士 / 信息技术｜2024.07--2026.12

## 实习 / 工作经历

### 天津易天数字化有限公司｜Java 后端实习生｜2025.11--2026.02

**应急网格业务插件开发｜Java 后端**

- 参与基于 YtCloud 微服务底座的应急网格业务插件开发，负责巡查、隐患多条件查询与 Excel 导出，完成十余个基于动态条件筛选的导出接口；基于 Spring Boot、MyBatis、EasyExcel 完成 Controller-Service-Mapper 业务链路，并结合登录用户上下文控制不同角色的数据范围。
- 梳理服务数据所有权，设计基于 OpenFeign、Nacos 和批量字典加载的解耦方案。
- 在企业信息接口性能排查中，定位图标、海报等 Base64 大字段随业务数据查询造成的数据库 I/O、结果集传输及序列化开销；将图片迁移至 MinIO，数据库改存对象 Key，并完成存量数据兼容迁移，实现结构化数据与文件资源解耦。

### 西部联通创新研究院｜Java 后端实习生｜2026.06--2026.09

**智慧安保展会数据可视化平台｜Java 后端 / 项目交付**

- 基于成熟的 Spring Boot 多模块底座完成工程初始化与业务模块接入，统一配置管理、异常处理、参数校验和接口文档。
- 设计“1 张展会主数据表 + 8 张统计指标表”的数据模型，按时间、地域、停留时长等业务维度建立唯一键，通过 MySQL UPSERT 实现统计数据幂等刷新；实现大屏聚合查询接口，完成日期分组、时间轴对齐、缺失数据补齐和比例计算，并增加 Token 鉴权与接口限流。
- 参与 CentOS 服务器部署联调，完成 JDK、Docker、Nginx 配置，MySQL、Redis 容器化及 API 反向代理。

**多功能 AI 实时监控系统｜Java 后端**

- 参与基于 RabbitMQ 的异步告警链路建设，消费 YOLOv8 推理结果并关联监测任务，将告警写入 Elasticsearch，支持多条件组合检索、分页排序和多时间维度趋势统计。
- 接入 Prometheus、Grafana 监控及钉钉告警通知，完善异常发现和告警闭环。

## 项目经历

### AI Agent 用户抽奖与激励平台｜用户增长与激励系统｜2026.05--2026.08

技术栈：Spring Boot、Spring Cloud Alibaba、RabbitMQ、Redis、Canal、XXL-JOB、Vue

- 项目描述：一个为智能体平台打造的用户增长与激励系统，引导用户参与 AI Agent 体验，实现用户拉新、留存与促活。项目主要包含抽奖、活动、奖品、积分四大模块，提供抽奖、积分、兑换、返利等服务，支持黑名单校验、N 消耗积分指定抽奖范围、抽奖 N 次解锁奖品、积分兑换奖品等玩法。
- 工作内容：使用模板方法定义抽奖流程标准，在模板流程中调用责任链完成抽奖前过滤与抽奖；将抽奖中、抽奖后的动作抽象为组合模式规则树，实现业务规则动态编排。
- 针对奖品库存扣减，采用 Redis DECR + SETNX 分段消费与加锁兜底设计；消费成功的库存通过异步队列配合定时任务回写数据库，在避免超卖的同时降低数据库压力。
- 使用 RabbitMQ 解耦抽奖发奖、异步行为返利等流程；当 Redis 缓存库存消耗完毕后，通过 MQ 推送库存更新消息，并清理延迟队列。
- 构建基于定时任务的异步补偿机制，自动扫描积分支付与返利链路中未成功发放或入账的任务并通过 MQ 重发，结合业务唯一键完成幂等校验。
- 采用分布式架构，使用 OpenFeign 提供服务间接口、Nacos 作为注册中心，并通过 XXL-JOB 调度多机实例任务；使用 Canal 同步分库分表产生的 Binlog 日志至 Elasticsearch，支持聚合查询。

---

### 企业级 Agentic RAG 知识问答系统｜企业级智能知识库｜2026.05--2026.07

项目描述：企业级 Agentic RAG 知识问答系统，使用 LLM Agent 替代传统硬编码检索流水线。模型运行时自主分析问题意图，动态编排向量检索、关键词检索、联网搜索、长期记忆等工具组合；答案经 LLM 自检纠错，并可追溯至原始文档章节与页码。系统通过 MCP 协议开放检索能力，支持任意 AI Agent 调用企业知识库。

核心技术：Spring Boot、Spring AI、Milvus、Lucene、Redis、MySQL、MyBatis-Plus、MinIO、Vue 3、TypeScript

核心职责与贡献：

1. **Agentic RAG 核心引擎：**基于 Spring AI Function Calling 实现 ReAct Agent 决策循环，LLM 根据问题特征实时决定工具、调用次数及是否追加检索，替代传统 if-else 路由；设计 QueryRouter 规则路由作为降级路径，Function Calling 异常时无缝切换，保证检索链路不中断。
2. **七步检索精炼链路（核心亮点）：** Query Rewrite、混合召回、RRF 倒数秩融合、Cross-Encoder 精排、上下文压缩、Self-Reflection 自检。
3. **MCP 工具复用架构：**设计 `@Tool` 单实例双轨接入，内部 Agent 通过 Function Calling 调用，外部客户端通过 MCP 协议调用，共享同一组 Bean；设计 `AgentToolContext (ThreadLocal)` 自动收集工具执行结果，工具层无需感知调用来源与上下文参数。
4. **跨会话长期记忆：**Agent 自动识别用户显式偏好并写入 Redis，后续会话通过 `recall_memory` 工具主动召回，结合用户画像注入 Prompt，实现追问感知与个性化回答。
5. **全链路容错与热配置：**为各层设计独立降级方案，包括嵌入失败、重排不可用及安全审查失败等场景；所有 RAG 参数与 LLM 模型支持数据库级热切换，运行时生效且无需重启。

## 专业技能与其他

**Java 与设计模式：**掌握 JUC、JVM 等基础 Java 知识，熟悉工厂、模板、责任链、策略等设计模式并能在项目中应用；**主流框架：**掌握 Spring、Spring Boot、MyBatis，了解 Spring Cloud、Dubbo、ZooKeeper、XXL-JOB。

**中间件与架构：**掌握 RabbitMQ 基本原理及解耦、异步处理；了解 DDD 领域驱动设计，熟悉分层架构，理解依赖倒置、充血模型、实体、聚合、值对象等概念。

**工具与 AI：**熟悉 IDEA、Git、Swagger、Apifox，掌握基本 Linux、Docker 命令；近两年持续高频使用 GPT、Claude、Gemini、Codex、Claude Code、Cursor、NotebookLM，将 AI 融入日常学习、资料检索、代码辅助、调试分析与知识整理。

**语言能力：**PTE 64 分（雅思 6.5 等效）
