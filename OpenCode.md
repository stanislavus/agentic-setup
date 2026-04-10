# Project: agentic-setup

Basic Agentic Setup — a curated configuration for AI-powered development.

## MCP Server Rules

### Tool Selection Priority

When multiple MCP servers can accomplish a task, prefer in this order:

1. **Built-in tools** (`view`, `write`, `edit`, `bash`, `grep`, `glob`, `ls`) — for direct file and shell operations
2. **Domain-specific MCP** — use the most specialized server for the task
3. **General-purpose fallback** — Filesystem, Server Memory, etc.

Do NOT call multiple MCP servers for the same task. Pick the best one.

---

### Context Mode (Context Window Protection)

- Use `ctx_batch_execute` for research: runs multiple commands, auto-indexes, and enables search.
- Use `ctx_search` for follow-up questions after indexing.
- Use `ctx_execute` / `ctx_execute_file` for data analysis, log processing, and computation in the sandbox.
- Do NOT use `ctx_execute` to create, modify, or overwrite files. Use `write` or `edit` instead.

### Context7 — Documentation Lookup

- When the user asks about a library, framework, SDK, API, CLI tool, or cloud service, use Context7 to fetch current documentation.
- Steps: `resolve-library-id` -> pick best match -> `query-docs` with library ID and question.
- Prefer Context7 over web search for library documentation. Training data may be outdated.
- Use Context7 for: API syntax, configuration, version migration, library-specific debugging, setup instructions.
- Do NOT use Context7 for: refactoring, writing scripts from scratch, debugging business logic, code review, general programming concepts.

### Serena — Semantic Code Intelligence

- Use Serena for intelligent, symbol-level code navigation instead of reading entire files with `view`.
- Workflow: `get_symbols_overview` -> `find_symbol` to locate -> read only the symbol bodies needed.
- Use `search_for_pattern` when symbol name or location is uncertain.
- Prefer Serena for: symbol navigation, codebase exploration, understanding call graphs, refactoring.
- Do NOT read entire files when Serena's symbolic tools suffice.

### Sequential Thinking

- Use Sequential Thinking for complex multi-step reasoning, architectural decisions, and debugging intricate logic chains.
- NOT needed for straightforward tasks (file edits, simple queries, formatting).

### Server Memory

- Use Server Memory to persist key facts, decisions, and context across sessions.
- Store: project conventions, team decisions, recurring patterns, important constraints.
- Do NOT store: code that exists in files, git history, ephemeral state.

### Filesystem

- Use Filesystem MCP for cross-project file access when needed.
- For files within the current workspace, prefer built-in tools (`view`, `write`, `edit`) over Filesystem MCP.

### Semgrep — Static Analysis

- Run Semgrep scans on code changes that involve: auth, user input processing, file system operations, database queries, or crypto.
- Use Semgrep for security-focused code reviews before committing sensitive code.
- Do NOT run Semgrep on every minor change (formatting, comments, config).

### Git MCP

- Use Git MCP for repository operations: status, diff, log, branch management.
- For complex git operations (merge, rebase), prefer running git CLI directly via `bash`.

### GitHub MCP

- Use GitHub MCP for: issues, pull requests, code search, repository management, and any GitHub API task.
- Use GitHub MCP (not git CLI) when interacting with the GitHub platform (PRs, issues, reviews).
- Use git CLI (not GitHub MCP) for local git operations (commit, branch, stash).

---

### PostgreSQL

- Use postgres MCP for direct database access: queries, schema inspection, migrations verification.
- Runs with `--access-mode=unrestricted`. Be cautious with write operations.
- Always use parameterized queries. Never concatenate user input into SQL.
- Before running DDL or destructive queries, confirm with the user.

### ClickHouse

- Use for analytical queries on columnar data — aggregations, time-series, large-scale analytics.
- Prefer ClickHouse over Postgres for: analytics dashboards, time-series, log aggregation.
- Prefer Postgres over ClickHouse for: transactional data, relational queries, CRUD operations.

### Supabase

- Use Supabase MCP for: database management, auth configuration, storage buckets, edge functions.
- Prefer Supabase over direct Postgres when working within a Supabase project (handles auth, policies).
- For raw SQL on a Supabase database, the postgres MCP may be more direct.

---

### Chrome DevTools

- Use for: live debugging of running web applications — DOM, network, console, performance, Lighthouse audits.
- Use for debugging existing pages. Use Playwright for automating interactions.

### Playwright

- Use for: browser automation, E2E testing flows, automated screenshots.
- Prefer Playwright over Chrome DevTools when: running test sequences, automating repetitive tasks.
- Prefer Chrome DevTools over Playwright when: inspecting a live debugging session, analyzing network traffic, running audits.

### WebMCP

- Use for: browsing websites, extracting content without a browser GUI.
- Prefer WebMCP over Playwright for simple content extraction.
- Prefer Playwright over WebMCP for multi-step interactions.

### Postman Suite

- **Postman** — full API platform: collections, environments, tests. Use for complete API workflows.
- **Postman Code** — generate client SDK code from API requests.
- **Postman Minimal** — lightweight one-off API calls.
- Selection: Minimal for quick tests -> Code for SDK generation -> full Postman for complex workflows.

### Apollo MCP

- Use for GraphQL schema introspection and query execution on a local Apollo server.
- Ensure the Apollo server is running at `http://127.0.0.1:8000` before use.

### API Specification (Apidog)

- Use to browse and query OpenAPI/Swagger specifications.
- Useful when: implementing API endpoints from specs, generating types from schemas, validating API contracts.

---

### Figma

- Use Figma MCP to access design files: layouts, components, styles, and design tokens.
- Do NOT guess design specifications. Always fetch from Figma when implementing designs.

### Pencil

- Pencil edits `.pen` design files. These files are encrypted — ONLY access them via Pencil MCP tools.
- Do NOT use `view`, `grep`, or any other tool to read `.pen` files. Always use Pencil MCP (`batch_get`, `batch_design`).
- Workflow: `get_editor_state` -> `open_document` -> `batch_get` to explore -> `batch_design` to modify.
- Max 25 operations per `batch_design` call.

---

### Tavily — Web Search

- Use for: research tasks, finding current information, gathering context.
- Prefer Tavily for: research queries, multi-source information gathering.
- Prefer Context7 for: specific library/framework documentation.

### Exa — AI Search

- Use for: high-quality web search and content extraction.
- Prefer Exa when: needing high-relevance results, extracting content from specific URLs.
- Prefer Tavily when: needing broader research with configurable depth and images.

### Search Tool Selection Guide

| Need | Tool |
|---|---|
| Library/framework docs | Context7 |
| Broad web research | Tavily |
| High-relevance search | Exa |
| Website content extraction | WebMCP or Exa |
| Codebase symbol search | Serena |
| Pattern search in code | Serena or `grep` |

---

### Docker & Podman

- Use Docker MCP by default for container management.
- Use Podman MCP only when: Docker daemon is unavailable, rootless containers are required, or user explicitly asks.
- Do NOT run both Docker and Podman MCP for the same task.

### Kubernetes (k8s)

- Use for: pod management, deployment status, service inspection, log retrieval, resource scaling.
- Before modifying cluster resources, always show the current state first.

### AWS Core & Azure

- Use AWS Core MCP for: S3, Lambda, SQS, SNS, DynamoDB, and general AWS API access.
- Use Azure MCP for: resource groups, VMs, storage accounts, and Azure infrastructure.
- Both require appropriate CLI authentication (`aws configure`, `az login`) before use.

### Trivy — Security Scanning

- Run Trivy scans on Docker images before deployment, Kubernetes manifests for misconfigurations, and IaC files for security issues.
- Use Trivy alongside Semgrep: Trivy for infrastructure/containers, Semgrep for application code.

### Sentry — Error Monitoring

- Use to investigate production errors, view stack traces, analyze performance issues, track release health.
- Do NOT use Sentry for local development errors — use Chrome DevTools or terminal output instead.

---

### General Best Practices

- Do NOT pass secrets or tokens directly in tool arguments. Use environment variable references.
- When starting a new session, check which MCP servers are relevant to the current task before using them.
