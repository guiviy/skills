# vLLM

Source: github.com/vllm-project/vllm（Apache-2.0）。高吞吐、低显存占用的大语言模型推理与服务引擎，源自 UC Berkeley Sky Computing Lab。GitHub 本周趋势榜第 12 位，2026-10-26 一周。

## 这是什么

- 用 PagedAttention 高效管理注意力 KV 显存，配合连续批处理（continuous batching）、分块预填充、前缀缓存，服务吞吐显著高于逐条推理。
- 支持 200 多种 Hugging Face 模型架构：解码器 LLM（如 Llama、Qwen、Gemma）、MoE（如 DeepSeek-V3、Qwen-MoE）、多模态、Embedding 与奖励模型等。
- 支持量化（FP8、INT8、INT4、GPTQ/AWQ、GGUF 等）、推测解码、结构化输出、工具调用与多 LoRA。

## 什么时候用

- 需要在自己的 GPU 机器上批量跑模型或对外提供推理服务时，例如离线批量生成、评测、RAG 后端。
- 需要 OpenAI 兼容接口时：vLLM 的服务接口兼容 OpenAI API，另支持 Anthropic Messages API 与 gRPC。
- 需要多卡并行时：支持张量并行、流水线并行、数据并行、专家并行与上下文并行。

## 安装

- 推荐用 `uv` 安装：`uv pip install vllm`，再跑 `vllm download-kernels` 预下载编译好的 kernel 以加快启动。
- 或用 `pip` 安装；开发调试可从源码构建（见官方文档的 build from source）。
- 硬件：支持 NVIDIA、AMD、Intel GPU 及多种加速硬件；CPU 也能跑但只适合小模型测试，不要指望 CPU 跑大模型出速度。
- 本技能只存用法说明，不替用户安装、不花钱。

## 核心用法

- 离线批量推理：用 Python 库加载模型后批量生成，适合评测与数据处理。
- 在线服务：`vllm serve <模型名>` 起一个 OpenAI 兼容服务，再用普通 OpenAI 客户端调用。
- 关键调参方向：并行方式（单卡先不设）、KV 缓存占比、最大并发序列数、量化方式——先跑默认配置测吞吐，再按显存与延迟目标调。
- 选模型前先查官方支持模型列表，确认架构与量化格式被支持，避免下载后才发现跑不起来。

## 注意事项

- 大模型 + 推理服务是显存密集型工作：先确认显卡显存与模型量化后体积匹配，不要在小显存卡上硬上满血大模型。
- 首次启动会下载模型与 kernel，耗时取决于网络与模型大小，属正常现象。
- 生产服务要另做鉴权、限流与日志，vLLM 本身只是推理引擎，不包办网关安全。
