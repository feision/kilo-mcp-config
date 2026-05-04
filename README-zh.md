# Kilo MCP 服务器配置文档

本文档记录了为 Kilo IDE 配置的 Model Context Protocol (MCP) 服务器，用于增强 AI 辅助开发能力。

## 概述

Kilo IDE 配置了 8 个 MCP 服务器，提供代码管理、浏览器调试、云服务操作和记忆功能。

## 配置的 MCP 服务器

### 1. GitHub 代码搜索 (`gh_grep`)
- **类型**: 远程
- **URL**: https://mcp.grep.app
- **用途**: 搜索 GitHub 仓库中的代码
- **功能**: 查找代码片段、函数和实现

### 2. Chrome DevTools (`chrome-devtools`)
- **类型**: 本地
- **命令**: `npx -y chrome-devtools-mcp@latest --executablePath "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"`
- **用途**: 浏览器自动化和调试
- **功能**:
  - 导航页面
  - 截图
  - 执行 JavaScript
  - 监控网络请求
  - 填写表单和交互元素

### 3. GitHub 管理 (`github`)
- **类型**: 本地
- **命令**: `npx -y @modelcontextprotocol/server-github`
- **环境变量**: `GITHUB_PERSONAL_ACCESS_TOKEN`
- **用途**: 完整的 GitHub 仓库管理
- **功能**:
  - 创建/更新文件和仓库
  - 管理 Issue 和 Pull Request
  - 搜索代码和 Issue
  - Fork 仓库
  - 创建分支和提交

### 4. Cloudflare Workers (`cloudflare`)
- **类型**: 本地
- **命令**: `npx -y @cloudflare/mcp-server-cloudflare run`
- **用途**: 管理 Cloudflare 资源
- **功能**:
  - 部署 Workers
  - 管理 KV 命名空间
  - 配置 D1 数据库
  - 设置 R2 存储
  - 管理环境变量

### 5. 文件系统访问 (`files`)
- **类型**: 本地
- **命令**: `npx -y @modelcontextprotocol/server-filesystem "C:\Users\Administrator\Documents\Codex" "C:\tmp"`
- **用途**: 安全的项目文件读写
- **功能**:
  - 读取文件和目录
  - 写入和更新文件
  - 创建/删除文件
  - 限制在配置目录内

### 6. HTML 提取器 (`html-extractor`)
- **类型**: 本地
- **命令**: `npx -y html-extractor-mcp`
- **用途**: 将网页转换为纯文本
- **功能**:
  - 获取 URL 内容
  - 提取文本
  - 清理 HTML 标签
  - 保留结构

### 7. 顺序思考 (`sequential-thinking`)
- **类型**: 本地
- **命令**: `npx -y @modelcontextprotocol/server-sequential-thinking`
- **用途**: 复杂问题的分步推理
- **功能**:
  - 分解复杂问题
  - 修订和细化思路
  - 分支到替代推理路径
  - 生成和验证假设

### 8. 云端记忆 (`mem0`)
- **类型**: 远程
- **URL**: https://mcp.mem0.ai/mcp
- **头部**: `Authorization: Token m0-...`
- **用途**: AI 助手的持久化向量记忆
- **功能**:
  - 语义搜索记忆
  - 多租户支持
  - 实体关系跟踪
  - 跨设备同步
  - REST API 和 MCP 协议

## 记忆工具

### `add_memory`
为用户/代理保存文本或对话历史。

### `search_memories`
使用过滤器进行语义搜索。

### `get_memories` / `get_memory`
列出或检索特定记忆。

### `update_memory` / `delete_memory`
更新或删除单个记忆。

### `list_entities`
列出存储在 Mem0 中的用户、代理、应用或运行。

## 配置文件位置

```
~/.config/kilo/kilo.jsonc
```

## GitHub 工具（通过 `github` MCP）

GitHub MCP 服务器提供 26+ 个工具：

- **仓库管理**: 创建/更新仓库，管理设置
- **文件操作**: 在仓库中创建/更新/删除文件
- **分支管理**: 创建/切换/删除分支
- **Issues**: 创建、列出、更新和管理 Issues
- **Pull Requests**: 创建、管理、审查和合并 PR
- **代码搜索**: 跨仓库搜索代码
- **Commits**: 列出和检查提交
- **协作**: 管理评论、审查和反应

## 浏览器工具（通过 `chrome-devtools` MCP）

Chrome DevTools MCP 服务器提供 27+ 个工具：

### 导航
- `navigate` - 导航到 URL
- `back` / `forward` / `reload` - 浏览器历史

### 交互
- `click` - 通过选择器或坐标点击元素
- `type` - 在输入框中输入文本
- `fill_form` - 填写多个表单字段
- `scroll` - 滚动页面或元素
- `drag_drop` - 拖放元素

### 内容提取
- `get_text` - 提取可见文本
- `get_html` - 获取元素 HTML
- `evaluate` - 执行 JavaScript
- `query_selector` - 通过 CSS 选择器查找元素

### 视觉
- `screenshot` - 截图
- `get_page_info` - 获取页面元数据

### 网络
- `network_enable` - 开始捕获网络请求
- `network_get_log` - 获取捕获的请求
- `get_response_body` - 获取响应内容

### 性能
- `performance_trace` - 记录性能跟踪
- `lighthouse_audit` - 运行 Lighthouse 审计

### 模拟
- `device_emulation` - 模拟设备
- `viewport` - 设置视口大小
- `geolocation` - 设置地理位置

## 开发工作流程

### 典型的 AI 辅助开发流程

1. **研究**: 使用 `gh_grep` 查找代码示例
2. **上下文**: 使用 `fetch` 阅读文档
3. **实现**: 使用 `files` 读写代码
4. **测试**: 使用 `chrome-devtools` 在浏览器中测试
5. **部署**: 使用 `cloudflare` 部署 Workers
6. **记忆**: 使用 `mem0` 记住项目上下文

### 使用 AI 进行代码审查

1. AI 使用 `gh_grep` 搜索类似模式
2. AI 使用 `files` 阅读当前代码
3. AI 使用 `sequential-thinking` 进行分析
4. AI 提出改进建议
5. AI 使用 `files` 应用更改
6. AI 使用 `chrome-devtools` 进行测试

### 跨平台记忆

1. 对话保存到 `mem0` 云端
2. 记忆可在任何设备上访问
3. 语义搜索找到相关上下文
4. 实体关系跟踪项目演变

## 安全注意事项

### 文件系统访问
`files` MCP 服务器限制在以下目录：
- `C:\Users\Administrator\Documents\Codex`
- `C:\tmp`

这可以防止访问敏感系统文件。

### Cloudflare 访问
Cloudflare MCP 使用 OAuth 令牌，权限有限：
- Workers: 读写
- KV: 读写
- D1: 读写
- R2: 读写
- Analytics: 只读

### GitHub 访问
GitHub 操作使用个人访问令牌，具有仓库特定的权限。

## 故障排查

### MCP 服务器未加载
1. 检查 Kilo 配置文件语法
2. 验证所有命令是否已全局安装
3. 检查令牌的的环境变量
4. 重启 Kilo IDE

### 记忆未持久化
1. 验证 `MEMORY_FILE_PATH` 是否可写
2. 检查 Mem0 API 密钥是否有效
3. 使用 `mem0` CLI 测试连接

### Cloudflare 连接问题
1. 运行 `wrangler whoami` 验证登录状态
2. 检查令牌权限
3. 验证账户是否有所需资源

## 添加新的 MCP 服务器

1. 安装 MCP 服务器包
2. 将配置添加到 `kilo.jsonc`
3. 指定类型（本地/远程）
4. 定义命令和参数
5. 如有需要，设置环境变量
6. 重启 Kilo

## 最佳实践

1. **最小权限原则**: 限制文件和云访问权限
2. **令牌管理**: 使用环境变量存储密钥
3. **记忆清理**: 定期审查存储的记忆
4. **代码审查**: 始终审查 AI 生成的代码
5. **测试**: 提交前测试 AI 的修改

## 资源

- [Model Context Protocol](https://modelcontextprotocol.io)
- [MCP Servers Registry](https://github.com/modelcontextprotocol/servers)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)
- [Mem0 Platform](https://mem0.ai)
- [Cloudflare Workers](https://workers.cloudflare.com)
- [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)

## 许可证

此配置文档按原样提供，供 Kilo IDE 用户使用。
各个 MCP 服务器有各自的许可证。
