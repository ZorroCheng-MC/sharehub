---
title: "Docker cagent vs Claude Subagents - Updated Comparison"
---

# Docker cagent vs Claude Subagents - Updated Comparison

**Created**: 2025-10-30
**Type**: Technical Comparison
**Status**: Updated Analysis

---

## Executive Summary

This is an updated comparison between Docker cagent and Claude Subagents, with corrections to the "Sharing & Distribution" and "API Access" sections based on further investigation.

**Key Corrections:**
1. **Sharing & Distribution**: Claude subagents CAN be shared through plugins/skills, not just GitHub repos
2. **API Access**: Claude Agent SDK provides programmatic API access, making it comparable to cagent's API mode

---

## Updated Comparison Table

| Aspect | Docker cagent | Claude Subagents | Winner |
|--------|--------------|------------------|--------|
| **Architecture** | Standalone CLI tool, multi-agent runtime with YAML configuration | Built into Claude Code CLI, operates within Claude ecosystem | Tie |
| **Deployment** | Runs as single process on local machine, can package agents as OCI images | Runs within Claude Code terminal sessions | cagent (more portable) |
| **Context Management** | Shared context within single cagent process, sub-agents inherit root context | Isolated contexts per subagent, prevents context pollution | **Claude** (better isolation) |
| **Model Flexibility** | Supports multiple providers: OpenAI, Anthropic, Google, Ollama, Azure, AWS Bedrock, Vertex AI | Limited to Anthropic's Claude models only | **cagent** (provider-agnostic) |
| **Configuration** | YAML files with version control, shareable via Docker Hub | Markdown files with YAML frontmatter, stored in `.claude/agents/` | Tie |
| **Tool Integration** | MCP protocol, filesystem, shell, think, todo, memory, custom toolsets | Claude's native tools (Read, Grep, Bash, etc.), MCP servers supported | Tie |
| **Parallel Execution** | All agents run within same process, coordination via sub_agents | True parallel execution, multiple subagents can run simultaneously | **Claude** (true parallelism) |
| **Learning Curve** | Requires understanding YAML structure, MCP protocol, model configuration | Simpler: Use /agents command, Claude helps create subagents interactively | **Claude** (easier) |
| **Cost** | Self-hosted with your own API keys, predictable costs | Requires Claude Code subscription + Anthropic API usage | **cagent** (more flexible) |
| **Orchestration** | Manual coordination through root agent and sub_agents configuration | Automatic orchestration: Claude decides when to use subagents | **Claude** (smarter routing) |
| **Use Cases** | General-purpose AI agents, custom workflows, research, any LLM task | Code-focused: debugging, testing, code review, development workflows | Depends on need |
| **Memory & State** | Persistent memory via file paths, todo lists, shared state | Memory tools available, context maintained per subagent | Tie |
| **Sharing & Distribution** ⚠️ UPDATED | **Docker Hub**: Push/pull agents as OCI images (`cagent push/pull`) | **Multiple channels**: <br>• Plugin system (skills can bundle subagents)<br>• GitHub repos<br>• Community at subagents.cc (60+ agents)<br>• Can be shared across Claude Code installations | **Tie** (different but equal) |
| **Team Coordination** | Define agent hierarchies with explicit delegation | Automatic task routing to appropriate subagents | **Claude** (less manual) |
| **Local Model Support** | Full support for Ollama and other local models | Limited local model support | **cagent** (local-first) |
| **API Access** ⚠️ UPDATED | HTTP REST API server (`cagent api`) for programmatic access | **Claude Agent SDK**: Programmatic API access to agents, similar to cagent API mode | **Tie** (both have API) |
| **Security** | Tool access defined per agent in YAML | Granular permission control, least-privilege per subagent | **Claude** (better security model) |
| **Performance** | Single process, resource usage depends on model provider | Multiple contexts = higher memory usage, but isolated | Depends on task |
| **Maturity** | Relatively new project | Part of established Claude Code platform | **Claude** |
| **Ecosystem** | Growing community, examples in GitHub repo | Large community, 60+ pre-built agents, extensive documentation | **Claude** (bigger ecosystem) |
| **Debugging** | Debug logging via `--debug` flag | Built-in debugging subagent available | **Claude** |
| **Customization** | Highly customizable: model parameters, instructions, toolsets | Limited to Claude model parameters and tool selection | **cagent** (more control) |

---

## Detailed Analysis of Corrections

### 1. Sharing & Distribution - CORRECTED

**Original Claim**: cagent wins with "built-in registry"

**Updated Analysis**: **TIE** - Both systems have robust distribution mechanisms

#### Docker cagent Distribution
- **OCI Images**: Package agents as Docker images
- **Docker Hub Integration**: `cagent push ./agent.yaml namespace/reponame`
- **Pull & Run**: `cagent run docker.io/username/my-agent:latest`
- **Versioning**: Use Docker tags for version control
- **Centralized**: Docker Hub as primary registry

#### Claude Subagents Distribution
- **Plugin/Skill System**: 
  - Subagents can be bundled within skills
  - Distributed through the Claude Code plugin ecosystem
  - Automatically available when skill is installed
- **GitHub Repositories**:
  - Agents stored as markdown files with frontmatter
  - Can be cloned/shared via Git
  - Version control through Git
- **Community Hub**:
  - subagents.cc hosts 60+ pre-built agents
  - Can be downloaded and used immediately
- **Cross-Installation Sharing**:
  - Agents in `.claude/agents/` can be copied between installations
  - Team sharing through shared directories/repos

**Verdict**: Both have first-class distribution mechanisms, just different approaches:
- **cagent**: Container-native, Docker ecosystem integration
- **Claude**: Plugin/skill ecosystem, Git-native, community hub

---

### 2. API Access - CORRECTED

**Original Claim**: cagent wins - "Terminal-based only, no API mode" for Claude

**Updated Analysis**: **TIE** - Both provide programmatic API access

#### Docker cagent API
```bash
# Start API server
cagent api

# HTTP REST API endpoints for agent interaction
# Programmatic access to agents
# Integration with external systems
```

#### Claude Agent SDK
- **Programmatic API**: Access to Claude agents through SDK
- **Same Capabilities**: Can invoke agents programmatically like cagent API
- **Integration**: Build applications that use Claude agents as services
- **Language Support**: SDK likely supports multiple programming languages

**Example Use Cases (Both)**:
- Webhook integrations
- Microservice architectures  
- CI/CD pipeline integration
- Custom workflows triggering agents
- Building applications on top of agents

**Verdict**: Both provide equivalent API access capabilities. The implementation differs (REST API vs SDK), but the functionality is comparable.

---

## Answer to Original Question

### Q: "Will each cagent start a container?"

**Answer**: **NO**

- **cagent is NOT containerized by default**: When you run `cagent run ./examples/pirate.yaml`, it executes as a regular process on your system
- **Agents are configurations**: Each agent is a YAML file specifying model, instructions, tools, and sub-agents
- **Single process runtime**: All agents (including sub-agents) run within one cagent process
- **Packaging vs Running**:
  - ✅ You CAN package agent configs as OCI images
  - ✅ You CAN push them to Docker Hub
  - ❌ But they DON'T run in separate containers
  - ℹ️ The OCI image contains the agent YAML config, not a running container

**Architecture**:
```
cagent process
├── Root Agent (from YAML)
├── Sub-Agent 1 (from YAML)
├── Sub-Agent 2 (from YAML)
└── Sub-Agent 3 (from YAML)
```

All agents share the same process space and coordinate through the cagent runtime.

---

## Revised Recommendations

### Choose Docker cagent if you:
- ✅ Need multi-provider model support (OpenAI, Google, Ollama, etc.)
- ✅ Want full control over agent configuration and deployment
- ✅ Need API server mode for integration with external systems
- ✅ Prefer Docker-based distribution and version control
- ✅ Want to run local models with Ollama
- ✅ Need cost control and flexibility in model selection
- ✅ Prefer container-native workflows

### Choose Claude Subagents if you:
- ✅ Are primarily doing software development work
- ✅ Want automatic orchestration and smart task routing
- ✅ Need isolated contexts to prevent pollution
- ✅ Prefer easier setup with interactive creation
- ✅ Want true parallel execution of multiple agents
- ✅ Value large community with 60+ pre-built agents
- ✅ Need better security controls with granular permissions
- ✅ Are already using Claude Code for development
- ✅ Want to leverage the plugin/skill ecosystem
- ✅ Prefer programmatic access via SDK

### Use Both:
- **cagent**: General AI orchestration, multi-model workflows, local models
- **Claude Subagents**: Development-focused tasks, code workflows, Claude Code integration

---

## Key Takeaways

1. **Distribution**: Both have mature distribution mechanisms
   - cagent = Docker Hub (OCI images)
   - Claude = Plugins/Skills + Community Hub + Git

2. **API Access**: Both provide programmatic access
   - cagent = REST API server
   - Claude = Agent SDK

3. **Container Architecture**: cagent does NOT run each agent in a container
   - All agents run in single process
   - OCI images are for distribution, not runtime isolation

4. **Ecosystem Maturity**: Claude has larger community and more pre-built agents

5. **Model Flexibility**: cagent wins for multi-provider support

6. **Developer Experience**: Claude wins for ease of use and smart orchestration

---

## Tags for Bases Filtering

**Type**: `reference`, `technical`
**Topics**: `AI`, `development`, `tools`, `automation`, `agents`
**Status**: `evergreen`
**Metadata**: `deep-dive`, `technical`, `actionable`, `comparison`

**Suggested Bases Queries**:
```
tags contains "AI" AND tags contains "agents"
type = reference AND tags contains "technical"
tags contains "deep-dive" AND status = "evergreen"
```

---

## Further Research

- [ ] Test Claude Agent SDK API capabilities
- [ ] Compare performance benchmarks between cagent and Claude subagents
- [ ] Explore hybrid architectures using both systems
- [ ] Document best practices for agent distribution in teams
- [ ] Investigate cost optimization strategies for multi-agent systems

---

**References**:
- Docker cagent: https://github.com/docker/cagent
- Claude Code Subagents: https://docs.anthropic.com/claude/docs/claude-code
- Community Agents: https://subagents.cc
- Context7 Library (used for research)

---

*Last Updated: 2025-10-30*
*Version: 2.0 (Corrected)*
