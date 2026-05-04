# Kilo MCP Servers Configuration

This repository documents the Model Context Protocol (MCP) servers configured for Kilo IDE to enhance AI-assisted development.

## Overview

Kilo IDE is configured with 8 MCP servers that provide various tools for code management, browser debugging, cloud services, and memory.

## Configured MCP Servers

### 1. GitHub Code Search (`gh_grep`)
- **Type**: Remote
- **URL**: https://mcp.grep.app
- **Purpose**: Search code across GitHub repositories
- **Usage**: Find code snippets, functions, and implementations

### 2. Chrome DevTools (`chrome-devtools`)
- **Type**: Local
- **Command**: `npx -y chrome-devtools-mcp@latest --executablePath "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"`
- **Purpose**: Browser automation and debugging
- **Features**:
  - Navigate pages
  - Take screenshots
  - Execute JavaScript
  - Monitor network requests
  - Fill forms and interact with elements

### 3. GitHub Management (`github`)
- **Type**: Local
- **Command**: `npx -y @modelcontextprotocol/server-github`
- **Environment**: `GITHUB_PERSONAL_ACCESS_TOKEN`
- **Purpose**: Full GitHub repository management
- **Features**:
  - Create/update files and repositories
  - Manage issues and pull requests
  - Search code and issues
  - Fork repositories
  - Create branches and commits

### 4. Cloudflare Workers (`cloudflare`)
- **Type**: Local
- **Command**: `npx -y @cloudflare/mcp-server-cloudflare run`
- **Purpose**: Manage Cloudflare resources
- **Features**:
  - Deploy Workers
  - Manage KV namespaces
  - Configure D1 databases
  - Set up R2 storage
  - Manage environment variables

### 5. Filesystem Access (`files`)
- **Type**: Local
- **Command**: `npx -y @modelcontextprotocol/server-filesystem "C:\Users\Administrator\Documents\Codex" "C:\tmp"`
- **Purpose**: Safe read/write access to project files
- **Features**:
  - Read files and directories
  - Write and update files
  - Create/delete files
  - Restricted to configured directories

### 6. HTML Extractor (`html-extractor`)
- **Type**: Local
- **Command**: `npx -y html-extractor-mcp`
- **Purpose**: Convert web pages to plain text
- **Features**:
  - Fetch URLs
  - Extract text content
  - Clean HTML tags
  - Preserve structure

### 7. Sequential Thinking (`sequential-thinking`)
- **Type**: Local
- **Command**: `npx -y @modelcontextprotocol/server-sequential-thinking`
- **Purpose**: Step-by-step complex reasoning
- **Features**:
  - Break down complex problems
  - Revise and refine thoughts
  - Branch into alternative reasoning paths
  - Generate and verify hypotheses

### 8. Cloud Memory (`mem0`)
- **Type**: Remote
- **URL**: https://mcp.mem0.ai/mcp/
- **Headers**: `Authorization: Token m0-...`
- **Purpose**: Persistent vector memory for AI assistants
- **Features**:
  - Semantic search across memories
  - Multi-tenant support
  - Entity relationship tracking
  - Cross-device synchronization
  - REST API and MCP protocol

## Memory Tools

### `add_memory`
Save text or conversation history for a user/agent.

### `search_memories`
Semantic search across memories with filters.

### `get_memories` / `get_memory`
List or retrieve specific memories.

### `update_memory` / `delete_memory`
Update or delete individual memories.

### `list_entities`
List users, agents, apps, or runs stored in Mem0.

## Configuration Location

The MCP servers are configured in:
```
~/.config/kilo/kilo.jsonc
```

## GitHub Tools (via `github` MCP)

The GitHub MCP server provides 26+ tools:

- **Repository Management**: Create/update repos, manage settings
- **File Operations**: Create/update/delete files in repositories
- **Branch Management**: Create/switch/delete branches
- **Issues**: Create, list, update, and manage issues
- **Pull Requests**: Create, manage, review, and merge PRs
- **Code Search**: Search code across repositories
- **Commits**: List and inspect commits
- **Collaboration**: Manage comments, reviews, and reactions

## Browser Tools (via `chrome-devtools` MCP)

The Chrome DevTools MCP server provides 27+ tools:

### Navigation
- `navigate` - Navigate to URL
- `back` / `forward` / `reload` - Browser history

### Interaction
- `click` - Click elements by selector or coordinates
- `type` - Type text into inputs
- `fill_form` - Fill multiple form fields
- `scroll` - Scroll page or elements
- `drag_drop` - Drag and drop elements

### Content Extraction
- `get_text` - Extract visible text
- `get_html` - Get element HTML
- `evaluate` - Execute JavaScript
- `query_selector` - Find elements by CSS selector

### Visual
- `screenshot` - Capture screenshots
- `get_page_info` - Get page metadata

### Network
- `network_enable` - Start capturing network requests
- `network_get_log` - Get captured requests
- `get_response_body` - Get response content

### Performance
- `performance_trace` - Record performance traces
- `lighthouse_audit` - Run Lighthouse audits

### Emulation
- `device_emulation` - Simulate devices
- `viewport` - Set viewport size
- `geolocation` - Set geolocation

## Development Workflow

### Typical AI-Assisted Development Flow

1. **Research**: Use `gh_grep` to find code examples
2. **Context**: Use `fetch` to read documentation
3. **Implementation**: Use `files` to read/write code
4. **Testing**: Use `chrome-devtools` to test in browser
5. **Deployment**: Use `cloudflare` to deploy Workers
6. **Memory**: Use `mem0` to remember project context

### Code Review with AI

1. AI searches for similar patterns with `gh_grep`
2. AI reads current code with `files`
3. AI uses `sequential-thinking` to analyze
4. AI suggests improvements
5. AI applies changes with `files`
6. AI tests with `chrome-devtools`

### Cross-Platform Memory

1. Conversations are saved to `mem0` cloud
2. Memories are accessible from any device
3. Semantic search finds relevant context
4. Entity relationships track project evolution

## Security Considerations

### Filesystem Access
The `files` MCP server is restricted to:
- `C:\Users\Administrator\Documents\Codex`
- `C:\tmp`

This prevents access to sensitive system files.

### Cloudflare Access
The Cloudflare MCP uses OAuth tokens with limited permissions:
- Workers: read/write
- KV: read/write
- D1: read/write
- R2: read/write
- Analytics: read-only

### GitHub Access
GitHub operations use personal access tokens with repository-specific permissions.

## Troubleshooting

### MCP Server Not Loading
1. Check Kilo configuration file syntax
2. Verify all commands are installed globally
3. Check environment variables for tokens
4. Restart Kilo IDE

### Memory Not Persisting
1. Verify `MEMORY_FILE_PATH` is writable
2. Check Mem0 API key is valid
3. Test Mem0 connection with `mem0` CLI

### Cloudflare Connection Issues
1. Run `wrangler whoami` to verify login
2. Check token permissions
3. Verify account has required resources

## Adding New MCP Servers

1. Install the MCP server package
2. Add configuration to `kilo.jsonc`
3. Specify type (local/remote)
4. Define command and arguments
5. Set environment variables if needed
6. Restart Kilo

## Best Practices

1. **Least Privilege**: Restrict filesystem and cloud access
2. **Token Management**: Use environment variables for secrets
3. **Memory Hygiene**: Regular review of stored memories
4. **Code Reviews**: Always review AI-generated code
5. **Testing**: Test AI modifications before committing

## Resources

- [Model Context Protocol](https://modelcontextprotocol.io)
- [MCP Servers Registry](https://github.com/modelcontextprotocol/servers)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)
- [Mem0 Platform](https://mem0.ai)
- [Cloudflare Workers](https://workers.cloudflare.com)
- [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)

## License

This configuration is provided as-is for Kilo IDE users.
Individual MCP servers have their own licenses.
