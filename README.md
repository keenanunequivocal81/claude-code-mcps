# Claude Code MCP Servers Collection

> Built by **[Artur Ferreira](https://github.com/arturseo-geo)** @ **[The GEO Lab](https://thegeolab.net)** · [𝕏 @TheGEO\_Lab](https://x.com/TheGEO_Lab) · [LinkedIn](https://linkedin.com/in/arturgeo)

[![Works with Claude Code](https://img.shields.io/badge/Works%20with-Claude%20Code-blueviolet)](https://docs.anthropic.com/en/docs/claude-code)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![MCP Servers](https://img.shields.io/badge/MCP%20servers-13+-purple)](#servers)

Production-tested [Model Context Protocol](https://modelcontextprotocol.io/) (MCP) servers for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI and Claude projects. These servers enable Claude to interact with your tools, APIs, and data sources seamlessly.

## What are MCP Servers?

MCP servers extend Claude's capabilities by providing access to:
- **External APIs** (GitHub, WordPress, Google Search Console, GA4, LinkedIn, Gumroad)
- **Databases** (PostgreSQL, Redis, Notion)
- **Web tools** (Firecrawl web scraping, Puppeteer browser automation)
- **Communication** (Telegram messaging)
- **File systems** (local filesystem access)

Each server is configured in `~/.mcp.json` and auto-loads when Claude Code starts.

## Quick Install

### 1. Clone this repository

```bash
git clone https://github.com/arturseo-geo/claude-code-mcps.git
cd claude-code-mcps
```

### 2. Copy MCP configuration

```bash
cp mcp.json.template ~/.mcp.json
```

### 3. Configure authentication

Edit `~/.mcp.json` and add API keys/credentials for the servers you want to use (see [Configuration](#configuration) below).

### 4. Test connection

```bash
claude code --version
```

Claude will load your MCP servers on startup.

## Servers

| Server | Purpose | Auth | Status |
|--------|---------|------|--------|
| **GitHub** | Repos, PRs, issues, code search, actions | PAT | ✅ Prod |
| **WordPress** | Posts, pages, media, SEO, caching | App password | ✅ Prod |
| **Google Search Console** | Search analytics, sitemaps, URL inspection | Service account | ✅ Prod |
| **Google Analytics 4** | Traffic, engagement, events, conversions | Service account | ✅ Prod |
| **LinkedIn** | People, companies, jobs, posts | Browser session | ✅ Prod |
| **PostgreSQL** | Custom databases, queries, data operations | Connection string | ✅ Prod |
| **Redis** | Cache, queues, sorted sets, operations | Host/port | ✅ Prod |
| **Notion** | Pages, databases, blocks, comments | API token | ✅ Prod |
| **Gumroad** | Products, sales, offers, email | API token | ✅ Prod |
| **Firecrawl** | Web scraping, crawling, search, extraction | API key | ✅ Prod |
| **Puppeteer** | Browser automation, screenshots, interaction | Local headless | ✅ Prod |
| **Telegram** | Messages, dialogs, channels | Session token | ✅ Prod |
| **Filesystem** | Local file read/write, directory operations | Allowed paths | ✅ Prod |

## Configuration

### GitHub MCP

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxxxxxxxxxx"
      }
    }
  }
}
```

**Get a PAT:**
1. Go to https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Scope: `repo`, `read:user`, `user:email`, `read:org`
4. Copy and paste into `.mcp.json`

### WordPress MCP

```json
{
  "mcpServers": {
    "wordpress": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-wordpress"],
      "env": {
        "WORDPRESS_SITES": "[{\"url\":\"https://example.com\",\"username\":\"user\",\"password\":\"pass\"}]",
        "WORDPRESS_AUTH_METHOD": "app_password"
      }
    }
  }
}
```

**Get an app password:**
1. Log in to WordPress admin
2. Go to Users → Your Profile → Application Passwords
3. Create new app password, copy username + password

### Google Search Console MCP

```json
{
  "mcpServers": {
    "gsc": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-gsc"],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/service-account-key.json"
      }
    }
  }
}
```

**Get service account credentials:**
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new service account
3. Grant it "Search Console Editor" role
4. Create JSON key and save to `~/.gcp/gsc-sa.json`
5. Set `GOOGLE_APPLICATION_CREDENTIALS` path in `.mcp.json`

### Google Analytics 4 MCP

```json
{
  "mcpServers": {
    "ga4": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-ga4"],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/service-account-key.json",
        "GA4_PROPERTY_ID": "528383456"
      }
    }
  }
}
```

### LinkedIn MCP

```json
{
  "mcpServers": {
    "linkedin": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-linkedin"],
      "env": {
        "LINKEDIN_PROFILE": "your-profile-name"
      }
    }
  }
}
```

**Setup:**
1. Install: `npm install -g @modelcontextprotocol/server-linkedin`
2. Run setup: `linkedin-mcp-setup` (uses patchright Chromium)
3. Profile stored at `~/.linkedin-mcp/profile/`

### PostgreSQL MCP

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:password@host:5432/dbname"
      }
    }
  }
}
```

### Redis MCP

```json
{
  "mcpServers": {
    "redis": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-redis"],
      "env": {
        "REDIS_URL": "redis://host:6379"
      }
    }
  }
}
```

### Firecrawl MCP

```json
{
  "mcpServers": {
    "firecrawl": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-firecrawl"],
      "env": {
        "FIRECRAWL_API_KEY": "fc-xxxxxxxxxxxx"
      }
    }
  }
}
```

**Get API key:** https://app.firecrawl.dev/api-keys

## Use Cases

### Content Operations

- **Blog Publishing**: WordPress + GitHub (version control) + Firecrawl (research) + GA4 (analytics)
- **Content Pipeline**: GitHub (code) + PostgreSQL (job tracking) + Redis (queue) + Telegram (notifications)

### SEO & Analytics

- **Search Intelligence**: GSC (rankings) + GA4 (traffic) + PostgreSQL (keyword DB) + Firecrawl (competitor analysis)
- **Traffic Investigations**: GSC + GA4 + WordPress (content) + Notion (reporting)

### Link Building & Research

- **Backlink Intelligence**: PostgreSQL (link data) + Firecrawl (new prospects) + LinkedIn (outreach)
- **Competitor Analysis**: Firecrawl (site structure) + PostgreSQL (internal DB) + Puppeteer (JS-heavy sites)

### Email & Sales

- **Campaign Management**: Gumroad (products/sales) + Telegram (notifications) + PostgreSQL (audience)
- **Lead Tracking**: Notion (CRM) + GitHub (pipeline code) + Firecrawl (enrichment)

## Troubleshooting

### Server won't connect

1. Check `~/.mcp.json` syntax: `cat ~/.mcp.json | jq`
2. Verify credentials are correct
3. Test individual server: `npx @modelcontextprotocol/server-github --version`
4. Check Claude Code logs: `~/.claude/logs/`

### Permission denied errors

- **GitHub**: Verify PAT has required scopes (see Configuration above)
- **GCP**: Check service account has project Editor role
- **WordPress**: Verify app password is correct
- **PostgreSQL**: Check connection string and user permissions

### Slow connections

- Firecrawl: Set `proxy: "enhanced"` for better reliability
- PostgreSQL: Check network latency to DB server
- LinkedIn: Browser session may be stale — re-run setup

## Security Best Practices

1. **Never commit `.mcp.json`** with real credentials — add to `.gitignore`
2. **Use environment variables**: `export GITHUB_PERSONAL_ACCESS_TOKEN=xxxx`
3. **Service accounts**: Use minimal roles (GSC: Search Console Editor only)
4. **API keys**: Rotate regularly, use different keys per environment
5. **Database credentials**: Use IAM/managed auth where possible
6. **Terraform/IaC**: Manage sensitive values via `.tfvars` files

## Contributing

PRs welcome! To add a new server:

1. Add server entry to table (above)
2. Document configuration section
3. Include use case example
4. Update CONTRIBUTING.md
5. Test and document any quirks

See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

## License

MIT — see [LICENSE](LICENSE)

---

Built and maintained by **[Artur Ferreira](https://github.com/arturseo-geo)** · **[thegeolab.net](https://thegeolab.net)** · [MIT License](LICENSE)
