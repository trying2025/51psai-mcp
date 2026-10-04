# 51PSAI MCP

**Generate, retouch and batch-process images through natural-language requests.**

[简体中文](README.md) | English

[Website](https://51psai.cn/) · [Tutorials](https://51psai.cn/tutorials/) · [Releases](https://github.com/trying2025/51psai-mcp/releases) · [Report an issue](https://github.com/trying2025/51psai-mcp/issues)

51PSAI MCP connects AI assistants to the image-processing features of 51PSAI / PsAIKit. Describe the result you want, and your assistant can select available models such as Banana and GPTImage, use reference images and prompt templates, submit generation tasks and retrieve results.

Built for e-commerce designers, photo retouchers, content creators and teams processing multiple images. It runs on Windows and macOS, connects through local stdio MCP and uses the desktop client for login and task execution.

> For example: “Use Banana to give these product images a consistent white-background e-commerce style. Preserve each product's colors, logo and text, then show me the results.”

## What you can do

| Use case | Tasks |
| --- | --- |
| Image generation and editing | Generate images with Banana or GPTImage, or edit them using text and reference images |
| E-commerce images | Product retouching, product image sets and detail images for listings and marketing |
| Fashion and portraits | Model outfit changes, AI models and ID-photo workflows |
| Image processing | Background removal, watermark removal, enhancement, old-photo restoration, upscaling and image translation |
| Templates and galleries | Find and apply prompt templates, personal prompts and gallery reference images |
| Batch processing | Generate multiple results from one image or process multiple images with a shared task and track each result |

Available functions and options come from the client's current function list. The assistant reads model choices, defaults and parameter dependencies, so you do not need to fill in API fields manually.

## MCP tools

| Purpose | Tools |
| --- | --- |
| Client status | `get_client_status` |
| Client preparation and progress | `ensure_client_ready`, `get_setup_status` |
| Function discovery and parameters | `list_functions`, `get_function_schema`, `resolve_function_parameters` |
| Image import | `import_image` |
| Image tasks and results | `submit_image_task`, `get_task`, `read_result` |
| Templates and galleries | `list_templates`, `list_template_configs`, `apply_template` |
| Batch configuration | `list_batch_functions`, `get_batch_function_schema`, `resolve_batch_parameters` |
| Batch tasks | `submit_batch_task`, `get_batch_task` |

Banana, GPTImage and other image functions use shared task tools, with models and parameters determined by their configurations. `ensure_client_ready` starts or prepares the client according to the connection configuration; use its operation ID with `get_setup_status` to check progress. Downloading the client requires a valid release manifest and allowed download origins configured in advance.

## Quick start

### 1. Prepare the desktop client

You need:

- A Windows or macOS computer.
- An MCP-enabled 51PSAI / PsAIKit desktop client and its companion `51ai-mcp` bridge.
- An AI client that supports **local stdio MCP**.
- A 51PSAI / PsAIKit account. Generation consumes credits according to the selected feature and your account's rules.

See [Releases](https://github.com/trying2025/51psai-mcp/releases) for packages and version notes. **The first public MCP package has not been published yet; the instructions below apply to users who already have an MCP preview package.**

On Windows, extract the client ZIP into a writable directory and launch the client; do not run it from inside the archive. On macOS, extract and open the client app. Sign in after launch. Windows requires a working WebView2 runtime.

### 2. Enable MCP and pair your assistant

Open **Settings → MCP access** in the desktop client:

1. Select **Enable MCP**.
2. To use local images, add their directories under **Allowed image directories**, one per line. To use online or gallery images, add the corresponding **Allowed HTTPS image origins**, such as the `https://image-domain` portion of an image URL.
3. Click **Save MCP settings** and confirm the status is **Listening**.
4. Enter a **Caller name**, such as `my-ai-assistant`. Use a different name for each AI client.
5. Select the permissions you need: **Status and function discovery**, **Image generation (may use credits)** and **Assets and results**. Click **Pair or re-pair**.
6. Under **Full path to the MCP bridge executable**, enter the absolute path to `51ai-mcp.exe` on Windows or `51ai-mcp` on macOS. Click **Copy host connection configuration**.

Keep the desktop client running while you connect your AI assistant for the first time.

### 3. Add the MCP server to your AI client

Add a local server in your AI client's MCP settings and paste the copied configuration. It contains the executable path and the local connection-file path; use the actual values generated by the desktop client.

For clients using a `mcpServers` JSON configuration, the structure looks like this:

```json
{
  "mcpServers": {
    "51AI": {
      "command": "/absolute/path/to/51ai-mcp",
      "args": ["--config", "/absolute/path/to/connection.json"]
    }
  }
}
```

These paths illustrate the structure; replace them with the generated values. In JSON, Windows paths use doubled backslashes, for example `C:\\Apps\\51AI\\51ai-mcp.exe`. If you already use other MCP servers, merge the new entry into your existing configuration.

If your AI client uses another configuration format, use the copied `command` as the executable and `args` as its arguments. Select **stdio / local command** as the server type.

Save and reload the MCP server, or restart your AI client.

### 4. Check the connection and start using it

Send this to your AI assistant first:

```text
Check the 51PSAI connection and login status. List the currently available image
and batch-processing functions. Do not generate any images yet.
```

Once the assistant can read the status and function lists, describe your task. For reference images, provide absolute file paths inside an allowed directory or image URLs from an allowed origin.

## Usage

### Image generation and editing

1. Use `list_functions` to discover available functions and `get_function_schema` to read the selected function's parameters.
2. For editing, import a local image or image URL with `import_image` to obtain an `asset_id`.
3. Use `resolve_function_parameters` to resolve defaults, visibility dependencies and required fields.
4. Submit with `submit_image_task` and retain the returned `task_id`.
5. Query `get_task` and, when complete, use its result ID with `read_result`.

### Templates and gallery images

Use `list_templates` for categories, `list_template_configs` for entries and `apply_template` to apply an entry to the selected function's parameters. Prompt templates populate text fields, while gallery assets are imported as images. Continue through parameter resolution and task submission after applying a template.

### Batch processing

Select a configuration using `list_batch_functions` and read `get_batch_function_schema`. Import images, validate with `resolve_batch_parameters`, submit with `submit_batch_task` and query the group using `get_batch_task`.

You can generate several results from one image or process multiple images using shared or per-line prompts. Generation counts, image input modes and prompt options follow the current batch configuration.

### Parameter defaults

Parameters come from the desktop client's current ai-function configuration. Missing values use configured defaults; visibility dependencies, hidden conditions and required fields are resolved together. The assistant should read the configuration and resolve parameters before submitting a task.

## Example prompts

Send these examples directly to your AI assistant. Replace the image paths with your own files and use models or functions available in the current function list.

### Generate an image

```text
Use Banana in 51PSAI to generate a product photo of a red ceramic mug on a white
background, with soft studio lighting and a clean composition. Check the available
ratios and sizes, choose 1:1, generate one image and show the result.
```

### Retouch a product photo

```text
Use 51PSAI to retouch this product photo: <absolute image path>.
Preserve its shape, logo, text and original colors. Improve the material detail
and lighting for an e-commerce listing. Check the available function and required
parameters, then generate one result.
```

### Use prompt templates and gallery images

```text
Find prompt templates and gallery images in 51PSAI suitable for a fashion-model
presentation. Show me the choices first. After I choose, apply them to my image
task, check that reference images and required parameters are complete, and submit.
```

### Process a batch

```text
Use 51PSAI to batch-process these images: <image path 1>, <image path 2>, <image path 3>.
Apply a consistent white-background e-commerce style while preserving each product's
colors, logo and text. Generate one result per image, show the processing plan before
submission, and summarize each image's status and result when finished.
```

### Check an existing task

```text
Check the progress of my previous 51PSAI batch task. Show completed results and
explain any failures. Only query the existing tasks; do not submit new generation.
```

## Local images and task results

- **Image input:** local files must be inside allowed directories; online images must use allowed HTTPS origins. Chat attachments work as MCP inputs only when the AI client can provide a readable path or URL.
- **Prompt templates:** browse public templates and your account's personal prompts, then apply them to the appropriate prompt field.
- **Gallery images:** browse gallery assets and import originals as references. Gallery use requires file permission and access to the image origin.
- **Applying a template:** you can adjust parameters afterwards. Generation starts when a task is submitted.
- **Results:** the desktop client saves generated images locally. The assistant can query status and read metadata and previews. MCP tools for arbitrary-directory export and format conversion are not available yet.
- **Batch progress:** query each item separately. Progress checks do not regenerate images; preserve the original task information when retrying to avoid duplicate submissions.

## FAQ

### Do I need Photoshop open?

Ordinary generation, template application and standalone batches using imported images do not require Photoshop. MCP layer splitting with local PSD output is still being integrated.

### Do I need my own model API key?

This integration uses the account and model services in the 51PSAI / PsAIKit desktop client. You do not need to add a model API key to the MCP configuration. Sign in through the desktop client first.

### Why is the MCP access setting missing?

Check whether your installed client version includes MCP support. Older versions may not have this setting; consult the release notes.

### What should I check if the connection fails?

Confirm that the desktop client is running, the MCP status is Listening, the bridge executable path is valid and the caller is paired. After moving or replacing the client, copy a fresh connection configuration and update your AI client.

### Why can't the assistant read my image or gallery asset?

Check the Assets and results permission and the allowed image directories or HTTPS origins. Click Save MCP settings after making changes, then retry the import.

### Why is a function unavailable?

Availability depends on your client version, account permissions and current function configuration. Ask the assistant to query the function list and parameter requirements first. Do not submit when required parameters are missing, dependencies are unmet or a function has not been integrated.

### Why can output dimensions differ from my request?

Size options and output behavior can vary by model. Check the returned image's actual width and height; use result metadata when exact dimensions matter.

### How do I disconnect an AI assistant?

Find its caller entry under MCP access and click Revoke. You can also turn off Enable MCP to stop the local service.

## Feedback and resources

- [Website and product information](https://51psai.cn/)
- [Step-by-step tutorials](https://51psai.cn/tutorials/)
- [Report an issue or request a feature](https://github.com/trying2025/51psai-mcp/issues): include your OS, desktop client version, AI client name and error message. Do not include account credentials.
- Maintainer release instructions: [RELEASING.md](RELEASING.md).
