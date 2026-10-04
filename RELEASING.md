# 发布 / Releasing

## 中文

1. 上传本目录的公开说明文件到 `trying2025/51psai-mcp`。这是分发仓库，不需要上传整个业务项目。
2. 从现有实现本地构建 Windows amd64、macOS arm64/amd64 桥接；使用经过正式发布处理的 MCP 客户端包。将桥接制作成实际宿主可以安装的 MCPB 包，在 GitHub Releases 上传固定版本的包及校验值。
3. 补齐可信 HTTPS 客户端发布清单：按产品、语言、系统、架构匹配，包含准确版本、URL、SHA-256、字节数、ZIP 内入口、依赖及 MCP API 兼容信息。macOS universal 包在清单中需要能匹配 arm64 和 amd64。下载和重定向来源必须符合桥接的允许来源策略。
4. 在未安装客户端的 Windows/macOS 环境，从公开下载渠道验证实际 AI 宿主安装、自动准备、登录配对、模型生成、模板应用、图库原图和批处理。记录平台、架构、版本及未通过项目。
5. 用真实发行信息替换 `server.json.example` 所有 `REPLACE_WITH_` 占位符，保存为 `server.json`。先确保包已公开可下载；SHA-256 必须来自最终上传字节。多平台 MCPB 布局及宿主选择必须先验证，不能假设目录会自动选包。
6. 在 macOS 安装官方发布工具并验证、认证、发布：

```bash
brew install mcp-publisher
mcp-publisher validate server.json
mcp-publisher login github
mcp-publisher publish server.json
curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.trying2025/51psai-mcp"
```

GitHub 登录需要由账号所有者完成。GitHub Releases、官方 Registry、实际 AI 宿主分别验证；创建仓库不等于已发布 Registry。

### 当前剩余验收

- 正式 MCPB 打包及实际产品宿主安装。
- MCP 客户端正式分发和公开下载清单。
- Windows 干净设备运行时准备及登录后的真实业务调用。
- 正式 macOS 签名/公证和 Windows 发布签名流程。
- 配置驱动功能逐项验收；本地 PSD 拆分仍待实现。

公开上传内容使用明确的文件白名单。不要上传私有验收目录、账号登录文件、配对状态、历史任务、内部配置或原项目整体备份。

示例连接参数中 `--product 51AI --locale zh-CN` 对应中文客户端。使用英文客户端时需要匹配实际客户端产品标识（`PsAIKit`）和 `en-US`，为各宿主设置自己的 `--caller-id`。不要把本地 loopback 地址注册成互联网远程 MCP 地址。

参考：[官方发布指南](https://modelcontextprotocol.io/registry/quickstart)、[支持的包类型](https://modelcontextprotocol.io/registry/package-types)、[发布工具命令](https://github.com/modelcontextprotocol/registry/blob/main/docs/reference/cli/commands.md)。

## English

1. Upload this directory's public documentation to `trying2025/51psai-mcp`. This distribution repository does not require the entire business project.
2. Build the existing Windows amd64 and macOS arm64/amd64 bridge locally, and use desktop packages prepared for official distribution. Create real MCPB bundles installable by the target hosts, then upload versioned bundles and checksums to GitHub Releases.
3. Provide a trusted HTTPS client-release manifest matching product, locale, OS and architecture, with accurate version, URL, SHA-256, byte size, ZIP entry point, dependencies and MCP API compatibility. A universal macOS package must match both arm64 and amd64 in the manifest. Downloads and redirects must satisfy the bridge's allowed-origin policy.
4. Starting from public downloads on Windows/macOS devices without the client installed, verify installation in real AI hosts, automatic preparation, login, pairing, model generation, templates, original gallery images and batches. Record platform, architecture, version and failed cases.
5. Replace every `REPLACE_WITH_` placeholder in `server.json.example` and save it as `server.json`. Packages must already be publicly downloadable; compute hashes from the final uploaded bytes. Verify platform packaging and host selection rather than assuming the directory chooses packages automatically.
6. On macOS, install the official publisher and run the commands shown above to validate, authenticate, publish and query the Registry.

The account owner completes GitHub authentication. Verify GitHub Releases, the Registry and actual AI hosts separately; repository creation is not Registry publication.

### Remaining acceptance

- Official MCPB packaging and installation in actual product hosts.
- Production desktop distribution and public download manifest.
- Runtime preparation on clean Windows devices and authenticated production business calls.
- Official macOS signing/notarization and Windows release-signing workflows.
- Individual configuration-driven feature acceptance; local PSD splitting is pending implementation.

Use an explicit upload allowlist. Exclude private acceptance directories, login files, pairing state, task history, internal configuration and full-project backups.

The sample flags `--product 51AI --locale zh-CN` target the Chinese client. For the English client, match its actual product identifier (`PsAIKit`) and locale `en-US`, and set a separate `--caller-id` for each host. Do not register a local loopback address as an internet-accessible remote MCP endpoint.

References: [Publishing quickstart](https://modelcontextprotocol.io/registry/quickstart), [Package types](https://modelcontextprotocol.io/registry/package-types), [Publisher commands](https://github.com/modelcontextprotocol/registry/blob/main/docs/reference/cli/commands.md).
