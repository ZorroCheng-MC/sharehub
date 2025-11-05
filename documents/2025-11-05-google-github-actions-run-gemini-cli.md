---
title: "run-gemini-cli - GitHub Action for Gemini AI Integration"
tags: [repository, AI, automation, development, tools, inbox, technical, actionable]
url: https://github.com/google-github-actions/run-gemini-cli
date: 2025-11-05
type: repository
status: inbox
priority: high
language: JavaScript/TypeScript
stars: 1500
license: Apache-2.0
---
/pu
# run-gemini-cli - GitHub Action for Gemini AI Integration

## 📦 Repository Overview

**run-gemini-cli** is an official GitHub Action from Google that integrates the Gemini AI model into development workflows via the Gemini CLI. It serves as both an autonomous agent for routine coding tasks and an on-demand AI collaborator directly within GitHub repositories.

**Key Capabilities:**
- 🤖 Automated PR reviews using Gemini AI
- 🏷️ Intelligent issue triage
- 💬 Conversational AI assistance via `@gemini-cli` mentions
- 🔧 Extensible with tools (GitHub CLI integration)
- 📝 Customizable via `GEMINI.md` context files

## 🏗️ Architecture

**Core Components:**
1. **Gemini Dispatch Workflow** - Central router for AI requests
2. **PR Review Workflow** - Automatic pull request analysis
3. **Issue Triage Workflow** - Automated issue categorization
4. **Assistant Workflow** - General-purpose conversational AI

**Technology Stack:**
- Platform: GitHub Actions
- AI Model: Google Gemini (via API or Vertex AI)
- CLI: Gemini CLI (installable tool)
- Language: JavaScript/TypeScript
- Authentication: API Key, Workload Identity Federation, or GitHub App

## 📁 Key Files & Components

**Essential Files:**
- `action.yml` - GitHub Action definition and inputs/outputs
- `GEMINI.md` - Project-specific AI context and instructions
- `examples/workflows/` - Pre-built workflow templates
  - `gemini-dispatch.yml` - Request routing
  - `issue-triage/` - Issue automation
  - `pr-review/` - PR review automation
  - `gemini-assistant/` - General AI assistance

**Configuration:**
- `.gemini/settings.json` - Project-level Gemini CLI settings
- `.gitignore` - Must exclude `.gemini/` and credentials

## 💡 Key Features & Use Cases

### Automation Modes

**1. Event-Triggered Automation**
```yaml
# Automatic PR review on PR open
on:
  pull_request:
    types: [opened]
```

**2. On-Demand Collaboration**
```bash
# In PR or issue comments:
@gemini-cli /review          # Review this PR
@gemini-cli /triage          # Triage this issue
@gemini-cli explain this code change
@gemini-cli write unit tests for this component
```

**3. Scheduled Tasks**
```yaml
# Nightly code analysis
on:
  schedule:
    - cron: '0 0 * * *'
```

### Primary Use Cases

✅ **Pull Request Reviews**
- Automatic code quality analysis
- Security vulnerability detection
- Best practice suggestions
- Architecture feedback

✅ **Issue Triage**
- Automatic categorization and labeling
- Priority assignment
- Duplicate detection
- Related issue linking

✅ **Code Analysis**
- Bug identification
- Performance optimization suggestions
- Code explanation and documentation
- Refactoring recommendations

✅ **Development Assistance**
- Unit test generation
- Code completion suggestions
- Debugging help
- API documentation lookup

## 🚀 Quick Start

### Prerequisites
1. Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey) (free tier available)
2. GitHub repository with Actions enabled

### Setup (5 minutes)

**Step 1: Add API Key as Secret**
```bash
# Repository Settings > Secrets > Actions > New secret
# Name: GEMINI_API_KEY
# Value: [your-api-key]
```

**Step 2: Update .gitignore**
```gitignore
# gemini-cli settings
.gemini/

# GitHub App credentials
gha-creds-*.json
```

**Step 3: Install Workflows**

**Option A (Recommended):** Use Gemini CLI setup command
```bash
gemini
/setup-github
```

**Option B:** Manually copy workflows from `examples/workflows/` to `.github/workflows/`

**Step 4: Try It Out**
- Open a PR → Automatic review starts
- Comment `@gemini-cli /review` → Manual review trigger
- Comment `@gemini-cli help me debug this error` → AI assistance

## ⚙️ Configuration

### Action Inputs (Key Parameters)

| Input | Description | Default |
|-------|-------------|---------|
| `gemini_api_key` | Gemini API key | Required |
| `gemini_model` | Model to use | (latest) |
| `gemini_cli_version` | CLI version | `latest` |
| `prompt` | System prompt | "You are a helpful assistant" |
| `settings` | JSON settings object | - |
| `gemini_debug` | Enable debug logging | `false` |
| `use_vertex_ai` | Use Vertex AI instead | `false` |
| `extensions` | List of CLI extensions | - |

### Authentication Options

**Option 1: Gemini API Key** (Simplest)
- Free tier: Generous quotas
- Setup: Add `GEMINI_API_KEY` secret
- Best for: Personal projects, small teams

**Option 2: Workload Identity Federation** (Most Secure)
- Uses Google Cloud service account
- No long-lived credentials
- Best for: Production, enterprise use

**Option 3: Vertex AI**
- Enterprise Google Cloud integration
- Advanced features and quotas
- Best for: Large organizations with GCP

### Customization via GEMINI.md

Create a `GEMINI.md` file in repository root:
```markdown
# Project Context for Gemini

## Coding Conventions
- Use TypeScript strict mode
- Follow Airbnb style guide
- Prefer functional components in React

## Architecture Patterns
- Clean Architecture with dependency injection
- Repository pattern for data access

## Security Requirements
- Never log sensitive data
- Use environment variables for secrets
```

## 🔒 Security & Best Practices

### Repository Security
- ✅ Enable branch protection on main
- ✅ Require PR reviews before merge
- ✅ Use Workload Identity Federation (not API keys) in production
- ✅ Pin action versions: `uses: google-github-actions/run-gemini-cli@v1.2.3`
- ✅ Review action logs regularly

### Secret Management
- Store `GEMINI_API_KEY` as GitHub secret (not in code)
- Rotate API keys periodically
- Use separate keys for dev/staging/prod
- Consider GitHub App authentication for team use

### Workflow Configuration
- Set appropriate permissions:
  ```yaml
  permissions:
    contents: read
    pull-requests: write
    issues: write
  ```
- Enable OpenTelemetry for observability
- Monitor usage and costs

## 📊 Observability

**Telemetry Support:**
- Traces, metrics, and logs to Google Cloud
- Performance monitoring
- Debugging insights
- Usage analytics

**Setup:** Configure GCP project and enable observability in action inputs

## 🔌 Extensions

Extend Gemini CLI functionality:
- Install from GitHub repositories
- Custom tool integrations
- Domain-specific capabilities

**Example:**
```yaml
extensions:
  - github-org/custom-extension
  - another-org/specialized-tool
```

## 🎯 Comparison with Alternatives

| Feature | run-gemini-cli | GitHub Copilot | CodeRabbit |
|---------|----------------|----------------|------------|
| **Cost** | Free tier available | $10-20/mo | $12-49/mo |
| **Model** | Google Gemini | OpenAI Codex | Multiple |
| **PR Reviews** | ✅ Automated | ❌ Manual | ✅ Automated |
| **Issue Triage** | ✅ Yes | ❌ No | ✅ Yes |
| **Customization** | ✅ GEMINI.md | Limited | Limited |
| **Self-Hosted** | ❌ Cloud only | ❌ Cloud only | ❌ Cloud only |
| **Conversational** | ✅ @mentions | ❌ No | ✅ Comments |

## 💡 Key Insights

1. **GitHub-Native AI Integration**: Seamless AI assistance within existing GitHub workflow - no context switching required

2. **Dual Mode Operation**: Both autonomous (event-triggered) and interactive (on-demand) modes provide flexibility

3. **Extensible Architecture**: Tool-calling capabilities allow integration with GitHub CLI and other tools

4. **Cost-Effective**: Free tier of Gemini API makes this accessible for small teams and open-source projects

5. **Context-Aware**: GEMINI.md files provide project-specific instructions, improving AI response quality

6. **Production-Ready Security**: Workload Identity Federation support enables secure enterprise deployment

## 🔗 Related Concepts

### Related Tools & Technologies
- [[Gemini CLI]] - Underlying CLI tool
- [[GitHub Actions]] - Workflow automation platform
- [[GitHub Copilot]] - Alternative AI code assistant
- [[Vertex AI]] - Google Cloud AI platform
- [[Workload Identity Federation]] - Secure GCP authentication

### Similar Projects
- [[CodeRabbit]] - AI PR review tool
- [[Sweep AI]] - AI issue resolver
- [[Bito AI]] - AI code review
- [[Codeium]] - AI code completion

### Learning Resources
- [Gemini CLI Documentation](https://github.com/google-gemini/gemini-cli/)
- [GitHub Actions Best Practices](./docs/best-practices.md)
- [Authentication Guide](./docs/authentication.md)
- [Observability Setup](./docs/observability.md)
- [Extensions Guide](./docs/extensions.md)

## 🏷️ Tags Analysis

### Why These Tags?

**Content Type:**
- `repository` - GitHub repository analysis

**Topics (4 tags):**
- `AI` - Core focus on Gemini AI model integration and LLM capabilities
- `automation` - GitHub Actions workflow automation for CI/CD
- `development` - Developer tools and software engineering workflows
- `tools` - Software tool/GitHub Action for workflow enhancement

**Status:**
- `inbox` - Newly captured, ready for further exploration

**Metadata (2 tags):**
- `technical` - Requires technical setup, YAML configuration, API keys, authentication
- `actionable` - Provides clear setup guide, ready-to-use workflow templates, practical examples

### Bases Filtering

**Find this note:**
```
type = repository AND tags contains "AI" AND tags contains "automation"
```

**Similar content:**
```
tags contains "AI" AND tags contains "tools" AND status = inbox
```

**Actionable technical content:**
```
tags contains "technical" AND tags contains "actionable" AND type = repository
```

**AI development tools:**
```
tags contains "AI" AND tags contains "development" AND tags contains "automation"
```

## 📝 Next Steps

### Immediate Actions
- [ ] Try setup in a test repository
- [ ] Review example workflows in `examples/`
- [ ] Test `@gemini-cli` commands in PR
- [ ] Configure GEMINI.md with project context
- [ ] Compare with existing PR review tools

### Further Exploration
- [ ] Explore Gemini CLI extensions
- [ ] Set up Workload Identity Federation for production
- [ ] Enable observability in Google Cloud
- [ ] Benchmark PR review quality vs. manual reviews
- [ ] Investigate cost implications for large repos

### Integration Ideas
- Combine with existing CI/CD pipelines
- Create custom workflows for specific use cases
- Build domain-specific extensions
- Integrate with team communication tools (Slack notifications)
- Set up scheduled code quality audits

---

**Captured**: 2025-11-05  
**Source**: https://github.com/google-github-actions/run-gemini-cli  
**Repository Stats**: ⭐ 1.5k stars, 🍴 160 forks  
**License**: Apache-2.0  
**Maintained By**: Google GitHub Actions team
