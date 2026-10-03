# 项目解读

## 工程模块设计
````
base-ai-assistant/
├── energy-admin-api/          # 管理后台（知识库管理、配置管理等）
├── energy-ai-api/             # 核心服务（RAG、Agent、MCP 实现）
├── energy-ai-mcp/             # MCP 服务定义
├── energy-ai-repository/      # 数据持久化（MySQL、PGVector）
├── ces-ai-rpc/                # RPC 接口定义（Dubbo/Feign，兼容 JDK 1.8）
├── service-common/            # 通用服务（配置、工具类）
└── service-domain/            # 领域模型定义
````
以上项目架构可以分析出，energy-ai-api、energy-admin-api是项目核心模块，其他模块则是围绕这两个服务而产生。


## energy-ai-api
![img.png](images/energy-ai-api-package.png)

### controller
![img.png](images/energy-ai-api-controller.png)

1.AiController包含了同步对话和流式对话接口。
2.同步对话（/chat/sync）逻辑：
    2.1 加载最近的 N 轮"已完成"对话历史。
    2.2 处理媒体类型，插入数据库，同时使用拦截器记录。
    2.3 发送回复
3.流式对话（/chat/sse）逻辑：
    3.1 加载最近的 N 轮"已完成"对话历史。
    3.2 发送流式回复，doOnNext拼接并流式回复每轮结果、doOnComplete获取最后回复结果插入数据库。

## energy-admin-api