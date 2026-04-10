# agentic-setup

Basic Agentic Setup — a curated configuration for AI-powered development with Cursor.

## Structure

```
.cursor/
├── mcp.json          # MCP server configurations
├── hooks.json        # Context Mode hooks (pre/post tool use)
└── rules/
    ├── mcp-general.mdc           # General MCP usage & tool selection priorities
    ├── mcp-code.mdc              # Code intelligence, docs, version control, security
    ├── mcp-data-infra.mdc        # Databases, containers, cloud, infrastructure
    ├── mcp-browser-api.mdc       # Browser automation, debugging, API testing
    └── mcp-design-research.mdc   # Design tools, search, error monitoring
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

## Cursor Rules

Rules are stored in `.cursor/rules/` and are always applied during sessions. They define:

- **Tool selection priorities** — which MCP server to prefer when multiple can handle a task
- **Domain-specific workflows** — step-by-step guidance for each server
- **Safety constraints** — confirmation requirements for destructive operations
- **Search tool guide** — when to use Context7 vs Tavily vs Exa vs Serena

## Environment Variables

The following environment variables are required by MCP servers:

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

Context Mode hooks are configured in `.cursor/hooks.json`:

- **preToolUse** — intercepts Shell, Read, Grep, WebFetch, and Context Mode tools to index output
- **postToolUse** — indexes tool results after execution
- **stop** — processes remaining data when the session ends

Use `ctx stats` to see context savings, `ctx doctor` to diagnose issues, or `ctx purge` to reset the knowledge base.
