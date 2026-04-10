# agentic-setup

Basic Agentic Setup — a curated configuration for AI-powered development with Cursor and Claude Code.

## Structure

```
.cursor/
├── mcp.json          # MCP server configurations (Cursor)
├── hooks.json        # Context Mode hooks (Cursor)
└── rules/
    ├── mcp-general.mdc           # General MCP usage & tool selection priorities
    ├── mcp-code.mdc              # Code intelligence, docs, version control, security
    ├── mcp-data-infra.mdc        # Databases, containers, cloud, infrastructure
    ├── mcp-browser-api.mdc       # Browser automation, debugging, API testing
    └── mcp-design-research.mdc   # Design tools, search, error monitoring

.claude/
└── settings.json     # Hooks configuration (Claude Code)

.opencode/
└── commands/         # Custom commands (OpenCode)

.opencode.json        # MCP server configurations (OpenCode)
CLAUDE.md             # All rules combined (Claude Code)
OpenCode.md           # All rules combined (OpenCode)
```

## MCP Servers

### Code Intelligence & Documentation

| Server | Purpose |
|---|---|
| **Serena** | Semantic code intelligence — symbol navigation, search, refactoring |
| **Context7** | Fetches up-to-date library/framework documentation |
| **Semgrep** | Static analysis for security vulnerabilities and code quality |
| **Git** | Local git operations (status, diff, log, branches) |
| **GitHub** | GitHub API — issues, PRs, code search, repo management |
| **Sequential Thinking** | Step-by-step reasoning for complex problems |
| **Context Mode** | Context window protection — indexes output, processes in sandbox |
| **Server Memory** | Persistent knowledge graph across sessions |
| **Filesystem** | Read/write access to workspace files |

### Databases

| Server | Purpose |
|---|---|
| **Postgres** | Direct PostgreSQL access — queries, schema inspection |
| **ClickHouse** | OLAP analytics — aggregations, time-series, large-scale queries |
| **Supabase** | Backend management — database, auth, storage, edge functions |

### Browser & Testing

| Server | Purpose |
|---|---|
| **Chrome DevTools** | Live debugging — DOM, network, console, performance audits |
| **Playwright** | Browser automation — E2E testing, screenshots, form interaction |
| **WebMCP** | Lightweight web browsing and content extraction |

### API & GraphQL

| Server | Purpose |
|---|---|
| **Postman** | Full API platform — collections, environments, testing |
| **Postman Code** | Generate client SDK code from API requests |
| **Postman Minimal** | Lightweight one-off API calls |
| **Apollo MCP** | Local Apollo GraphQL server — schema introspection, queries |
| **API specification** | Apidog — browse and query OpenAPI/Swagger specs |

### Design

| Server | Purpose |
|---|---|
| **Figma** | Access Figma designs — layouts, components, styles, tokens |
| **Pencil** | Create and edit `.pen` design files (encrypted, Pencil-only access) |

### Search & Research

| Server | Purpose |
|---|---|
| **Tavily** | AI-optimized deep web search with image support |
| **Exa** | High-relevance AI search engine |

### Infrastructure & Cloud

| Server | Purpose |
|---|---|
| **Docker** | Container management — build, run, stop, list |
| **Podman** | Rootless container alternative to Docker |
| **k8s** | Kubernetes — pods, deployments, services, logs |
| **AWS Core** | AWS services — S3, Lambda, SQS, SNS, DynamoDB |
| **Azure** | Azure resource management and cloud infrastructure |
| **Trivy** | Security scanner for containers, K8s, and IaC |

### Monitoring

| Server | Purpose |
|---|---|
| **Sentry** | Error tracking, performance monitoring, release health |

## Rules

### Cursor

Rules are stored in `.cursor/rules/` as `.mdc` files and always applied. They define tool selection priorities, domain-specific workflows, safety constraints, and a search tool guide.

### Claude Code

All rules are consolidated in `CLAUDE.md` at the project root. Claude Code reads this file automatically at the start of each session.

### OpenCode

All rules are consolidated in `OpenCode.md` at the project root. OpenCode reads this file as a project memory file at the start of each session. Custom commands can be added as `.md` files in `.opencode/commands/`.

> **Note:** OpenCode has been archived and continued under the name [Crush](https://github.com/charmbracelet/crush) by the Charm team.

## Environment Variables

| Variable | Server |
|---|---|
| `CONTEXT7_API_KEY` | Context7 |
| `GITHUB_PERSONAL_ACCESS_TOKEN` | GitHub |
| `FIGMA_ACCESS_TOKEN` | Figma |
| `POSTGRES_USERNAME`, `POSTGRES_PASSWORD`, `POSTGRES_DATABASE` | Postgres |
| `CLICKHOUSE_TOKEN` | ClickHouse |
| `TAVILY_API_KEY` | Tavily |
| `POSTMAN_API_KEY` | Postman |
| `APIDOG_API_BASE_URL` | API specification |

## Hooks

Context Mode hooks protect the context window by indexing tool output:

- **Cursor** — configured in `.cursor/hooks.json`
- **Claude Code** — configured in `.claude/settings.json` under `hooks`

Both configurations run the same Context Mode hook commands to auto-index output before and after tool use, and on session stop.

OpenCode does not currently support hooks. Use Context Mode MCP tools directly (`ctx_batch_execute`, `ctx_search`) for context window protection.

Use `ctx stats` to see context savings, `ctx doctor` to diagnose issues, or `ctx purge` to reset the knowledge base.
