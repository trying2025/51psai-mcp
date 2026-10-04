# 51PSAI MCP

[中文](#中文) · [English](#english)

Website / 官网: https://51psai.cn/

Repository / 仓库: https://github.com/trying2025/51psai-mcp

Registry name / 注册名称: `io.github.trying2025/51psai-mcp`

## 中文

51PSAI MCP 通过独立 Go stdio 桥接程序，让 AI 助手访问 Windows/macOS 本地客户端中的图像生成、修图、提示词模板和批量处理工作流。

### 产品功能与 MCP 状态

51PSAI 产品及 ai-function 配置覆盖 Banana、GPTImage、商品精修、模特换装、电商套图与详情图、抠图去背景、高清及老照片修复、尺寸放大、图片翻译和证件照等场景。参数来自当前功能配置，包含默认值、显示/隐藏条件、必填校验及模板选项。

产品功能列表不代表每个功能已经通过 MCP 验收。2026-10-04 的生产验收覆盖 Banana/GPTImage 生成、提示词模板、图库原图导入、单图多次生成及多图批处理。本地 PSD 图层拆分尚未接入，证件照等其他场景仍需逐项验收。GPT-Image2.5 曾出现请求尺寸与实际输出尺寸不一致的案例。

### 发布状态

本仓库目前是公开分发文档框架。正式 MCPB 包、客户端自动下载清单和官方 Registry 条目尚待发布。普通 ZIP 不能通过修改扩展名变成 MCPB；Registry 只保存元数据。

### 安装与连接

1. 正式包发布后，下载符合系统和架构的桥接程序以及支持 MCP 的客户端。当前官网旧版本不能据此视为具备 MCP 支持。
2. Windows 客户端 ZIP 解压到当前用户有写入权限的目录运行；macOS 使用相应客户端 app。Windows 需要可用的 WebView2 运行时。
3. 登录客户端，在 MCP 设置中开启服务并为 AI 宿主配置配对和所需权限。
4. 在支持本地 stdio MCP 的宿主中填写桥接程序绝对路径，参考 `examples/mcp-settings.json.example`。该示例需要替换路径，不是通用于全部宿主的配置格式。
5. 调用 `get_client_status` 查看连接和登录状态。生成可能消耗账号积分。

自动准备使用 `ensure_client_ready`，它返回操作 ID；之后用该 ID 调用 `get_setup_status`。需要预先允许自动准备，并配置可信 HTTPS 客户端发布清单和下载来源。创建仓库或连接 MCP 本身不会自动下载客户端。正式清单尚未发布，当前先使用手动准备方式。

### 调用流程

- 普通任务：`list_functions` → `get_function_schema` → 按需 `import_image` → `resolve_function_parameters` → `submit_image_task` → `get_task` → `read_result`。
- 模板：`list_templates` → `list_template_configs` → `apply_template`。应用模板返回参数，后续仍需解析及提交生成。
- 批处理：`list_batch_functions` → `get_batch_function_schema` → `resolve_batch_parameters` → `submit_batch_task` → `get_batch_task`。

以当前工具 Schema 为准，不固定模型枚举、字段名或依赖规则。重试提交必须保持原幂等键及输入。

### 发布资料

- `server.json.example`：官方 Registry 元数据模板，含不可发布的占位符。
- `examples/mcp-settings.json.example`：手动连接示例。
- `RELEASING.md`：发布顺序及验收清单。

## English

51PSAI MCP connects AI assistants to local Windows/macOS image generation, editing, prompt-template and batch workflows through a standalone Go stdio bridge.

### Product capabilities and MCP status

The product and ai-function configurations cover Banana, GPTImage, product retouching, model outfit changes, e-commerce image sets and detail images, background removal, photo restoration, upscaling, image translation and ID photos. Parameters follow the current configuration, including defaults, visibility conditions, required-field validation and template options.

Product capabilities are not a statement that every feature has passed MCP acceptance. Production checks on 2026-10-04 covered Banana/GPTImage generation, prompt templates, original gallery imports, repeated single-image generation and multiple-image batches. Local PSD layer splitting is pending, and ID photos and other scenarios still need individual acceptance. A GPT-Image2.5 case returned dimensions different from the requested size.

### Release status

This repository currently provides distribution documentation. Official MCPB packages, the automatic client-download manifest and the official Registry entry are pending publication. Renaming a ZIP does not create an MCPB package; the Registry stores metadata only.

### Installation and connection

1. Once official packages are published, download the bridge for your platform and architecture and an MCP-enabled desktop client. An older website download should not be assumed to support MCP.
2. Extract the Windows client ZIP into a directory writable by the current user, or use the macOS client app. Windows requires a working WebView2 runtime.
3. Sign in, enable MCP in the desktop settings and pair the AI host with the required permissions.
4. Configure the bridge's absolute path in a host supporting local stdio MCP. See `examples/mcp-settings.json.example`; replace the path and adapt the format to your host.
5. Call `get_client_status` to check connectivity and login. Generation may consume account credits.

Automatic preparation uses `ensure_client_ready`, which returns an operation ID for polling with `get_setup_status`. It requires an allowed preparation policy and a trusted HTTPS client-release manifest and download origins. Creating the repository or connecting MCP does not automatically download the client. The production manifest is not published yet; use manual preparation for now.

### Tool workflow

- Image tasks: `list_functions` → `get_function_schema` → `import_image` when needed → `resolve_function_parameters` → `submit_image_task` → `get_task` → `read_result`.
- Templates: `list_templates` → `list_template_configs` → `apply_template`. Apply returns parameters; resolve and submit separately to generate.
- Batches: `list_batch_functions` → `get_batch_function_schema` → `resolve_batch_parameters` → `submit_batch_task` → `get_batch_task`.

Follow the current tool schemas rather than hard-coding model choices, field names or dependencies. Reuse the original idempotency key and inputs when retrying a submission.

### Release materials

- `server.json.example`: Registry metadata template with placeholders that cannot be published.
- `examples/mcp-settings.json.example`: manual connection example.
- `RELEASING.md`: publication steps and acceptance checklist.
