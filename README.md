# 多Agent协同软件开发自动化系统

这是一个完整可运行的 Java/Spring Boot 示例项目，用于演示“多 Agent 协同完成软件开发自动化”的主流程：

1. 产品分析 Agent
2. 需求分析 Agent
3. 架构设计 Agent
4. 代码生成 Agent
5. 测试 Agent
6. 部署 Agent

## 环境要求

- JDK 17
- Maven 3.8+
- 可选：Docker
- 可选：Ollama

## 启动

```bash
mvn spring-boot:run
```

浏览器访问：

```text
http://localhost:8088
```

## 接口

### 启动工作流

```bash
curl -X POST http://localhost:8088/api/workflows/run \
  -H "Content-Type: application/json" \
  -d '{"projectName":"设备台账管理系统","techStack":"Spring Boot","description":"支持设备登记、查询、版本记录、变更日志"}'
```

### 查询工作流

```bash
curl http://localhost:8088/api/workflows/{workflowId}
```

### 下载生成项目

```text
http://localhost:8088/api/workflows/{workflowId}/download
```

## 接入 Ollama

修改 `src/main/resources/application.yml`：

```yaml
multi-agent:
  llm:
    mode: ollama
    ollama-url: http://localhost:11434/api/generate
    ollama-model: qwen2.5-coder:7b
```

然后确保本机 Ollama 已启动，并且已拉取模型：

```bash
ollama pull qwen2.5-coder:7b
```

## 如何扩展 Agent

新增一个类实现：

```java
public interface DevAgent {
    String name();
    String stage();
    void execute(WorkflowContext context) throws Exception;
}
```

然后加上 `@Component`，系统会自动注入执行链。

如需控制执行顺序，修改 `WorkflowEngine#order`。

## 生成目录

默认生成到：

```text
./generated/{workflowId}/
```

每次运行都会生成一个新的工作目录。
