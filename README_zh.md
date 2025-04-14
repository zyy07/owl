<h1 align="center">
	🦊 MCPBench: A Benchmark for Evaluating MCP Servers
</h1>


<div align="center">

[![文档][docs-image]][docs-url]
[![许可证][package-license-image]][package-license-url]

</div>

MCPBench 是一个用于评估 MCP 服务器的评估框架。它支持评估两种类型的服务器：网络搜索和数据库查询，并且兼容本地和远程 MCP 服务器。该框架主要在相同的 LLM 和 Agent 配置下，从任务完成准确性、延迟和Token消耗等方面评估不同的 MCP 服务器（如 Brave Search、DuckDuckGo 等）。

本项目基于 [LangProBe: a Language Programs Benchmark](https://arxiv.org/abs/2502.20315) 实现。

<hr>

<div align="center">
<h4 align="center">

[中文](https://github.com/modelscope/MCPBench/blob/main/README_zh.md) |
[English](https://github.com/modelscope/MCPBench/blob/main/README.md)

</h4>
</div>

# 📋 目录

- [🔥 新闻](#新闻)
- [🛠️ 安装](#安装)
- [🚀 快速开始](#快速开始)
  - [LLM 配置](#llm-配置)
  - [启动 MCP 服务器](#启动-mcp-服务器)
  - [启动评估](#启动评估)
- [🧂 数据集和实验](#数据集和实验)
- [🚰 引用](#引用)

# 🔥 新闻
+ `2025年4月14日` 🌟 我们很高兴地宣布 MCPBench 现已开源。

# 🛠️ 安装
本框架需要 Python 版本 >= 3.11，nodejs 和 jq。

```bash
conda create -n mcpbench python=3.11 -y
conda activate mcpbench
pip install -r requirements.txt
```

# 🚀 快速开始
## LLM 配置
在环境变量中准备 LLM 密钥和端点：
```bash
export MODEL_KEY=your_api_key_here
export MODEL_ENDPOINT=your_model_endpoint_here
```

## 启动 MCP 服务器
### 将 stdio MCP 作为 SSE 启动
如果 MCP 不支持 SSE，请按如下方式编写配置：
```json
{
  "name": "DuckDuckGo",
  "command": "uvx duckduckgo-mcp-server",
  "args": "",
  "port": 8001,
  "tool_name": "search",
  "tool_keyword": "query"
}
```

将此配置文件保存在 `configs` 文件夹中，并使用以下命令启动：

```bash
sh launch_mcp_as_sse.sh YOUR_CONFIG_FILE
```

例如，如果配置文件是 duckduckgo.json，则运行：
```bash
sh launch_mcp_as_sse.sh duckduckgo.json
```

### 启动 SSE MCP
如果您的服务器支持 SSE，您可以直接使用它。URL 将是 http://localhost:8001/sse

对于支持 SSE 的 MCP 服务器，请按如下方式编写配置：
```json
{
  "name": "Exa Search",
  "command": "",
  "args": "",
  "url": "https://mcp-a11aaa67-0a82-40a2.api-inference.modelscope.cn/sse",
  "port": 0,
  "tool_name": "web_search",
  "tool_keyword": "query"
}
```

## 启动评估
要评估 MCP 服务器在网络搜索任务上的性能：
```bash
sh evaluation_websearch.sh YOUR_CONFIG_FILE
```

要评估 MCP 服务器在数据库查询任务上的性能：
```bash
sh evaluation_db.sh YOUR_CONFIG_FILE
```

# 🧂 数据集和实验
我们的框架提供了两个用于评估的数据集。对于 WebSearch 任务，数据集位于 `MCPBench/langProBe/WebSearch/data/frames_test.jsonl`，包含来自 [Frames](https://arxiv.org/abs/2409.12941)、新闻和技术领域的各 200 个问答对。我们用于自动构建评估数据集的框架将在稍后开源。

对于数据库查询任务，数据集位于 `MCPBench/langProBe/DB/data/car_bi.jsonl`。您可以按以下格式添加自己的数据集：

```json
{
  "unique_id": "",
  "Prompt": "",
  "Answer": ""
}
```

我们已经在这两个任务上评估了主流的 MCP 服务器。有关详细的实验结果，请参阅[文档]()

# 🚰 引用
如果您觉得这项工作有用，请考虑引用我们的项目：

```bibtex
@misc{mcpbench,
  title={MCPBench: A Benchmark for Evaluating MCP Servers},
  author={Zhiling Luo,Xiaorong Shi, Xuanrui Lin, Yangjin Gao,Bolin Ding},
  howpublished = {\url{https://github.com/modelscope/MCPBench}},
  year={2025}
}
```

[docs-image]: https://img.shields.io/badge/Documentation-EB3ECC
[docs-url]: https://camel-ai.github.io/camel/index.html
[package-license-image]: https://img.shields.io/badge/License-Apache_2.0-blue.svg
[package-license-url]: https://github.com/modelscope/MCPBench/blob/main/LICENSE
