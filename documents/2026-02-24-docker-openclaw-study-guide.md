---
title: "Study Guide: Secure AI Agent Execution with Docker Sandboxes and OpenClaw"
tags:
  - study-guide
  - docker
  - AI
  - security
  - development
  - containers
  - processing
  - deep-dive
  - technical
source: https://www.docker.com/blog/run-openclaw-securely-in-docker-sandboxes/
date: 2026-02-24
type: study-guide
status: processing
difficulty: intermediate
estimated-time: 45-60 minutes
priority: high
read: false
---

# Study Guide: Secure AI Agent Execution with Docker Sandboxes and OpenClaw

## 📚 Overview

**Subject:** Running OpenClaw (open-source AI coding agent) securely in Docker Sandboxes with local AI models
**Source:** Docker Blog - Oleg Selajev
**Generated:** 2026-02-24

### 🎯 Learning Objectives

By the end of this study session, you will be able to:

- [ ] Understand Docker Sandboxes as an isolation primitive for AI agents
- [ ] Explain the security architecture of Docker Sandboxes (micro VMs, network proxy, credential injection)
- [ ] Set up OpenClaw with Docker Model Runner (local models, no API keys)
- [ ] Configure Docker Sandboxes network proxy to control agent connectivity
- [ ] Build a custom Docker Sandbox image with OpenClaw pre-installed
- [ ] Implement API key injection through network proxy for secure credential handling
- [ ] Distinguish between local model experimentation vs. cloud model usage patterns
- [ ] Troubleshoot Node.js/HTTP proxy issues in containerized environments

### ⏱️ Study Plan

- **Total Estimated Time:** 45-60 minutes
- **Difficulty Level:** Intermediate (requires Docker & containerization knowledge)
- **Prerequisites:** 
  - Docker Desktop installed
  - Basic understanding of Docker containers and micro VMs
  - Familiarity with AI agents/coding assistants
  - Node.js basics (helpful but not required)
- **Recommended Study Method:** 
  - Hands-on: Follow along with Docker Desktop
  - Theory-first: Understand architecture before implementation
  - Hybrid: Learn concepts, then attempt setup

## 📋 Content Structure

### Part 1: Core Concepts (15 minutes)
1. **What are Docker Sandboxes?**
   - Isolation primitive using micro VMs
   - Contrast with standard containers
   - Security boundaries: filesystem, network, environment

2. **OpenClaw Introduction**
   - Open-source AI coding agent
   - Node.js-based architecture
   - Integration with local/cloud models

3. **Security Architecture**
   - Network proxy enforcement
   - API key injection mechanism
   - Credential leak prevention

### Part 2: Quick Start Setup (15 minutes)

> **Source:** [Run OpenClaw Securely in Docker Sandboxes](https://www.docker.com/blog/run-openclaw-securely-in-docker-sandboxes/) — Docker Blog, Oleg Selajev
> **Docs:** [OpenClaw Docker Install](https://docs.openclaw.ai/install/docker)

#### Option A: Pre-built Image (Fastest)

```bash
# 1. Enable Docker Model Runner: Docker Desktop → Settings → Docker Model Runner → Enable

# 2. Pull a local model
docker model pull ai/qwen2.5:7B-Q4_K_M

# 3. Create sandbox from pre-built image
docker sandbox create --name openclaw -t olegselajev241/openclaw-dmr:latest shell .

# 4. Allow network access to localhost (for Model Runner)
docker sandbox network proxy openclaw --allow-host localhost

# 5. Run the sandbox
docker sandbox run openclaw

# 6. Inside sandbox — start OpenClaw
~/start-openclaw.sh
```

**Switch models on the fly:**
```bash
~/start-openclaw.sh list                        # list available models
~/start-openclaw.sh ai/gpt-oss:20B-UD-Q4_K_XL  # switch to a different model
```

#### Option B: Build Custom Image (From Scratch)

```bash
# 1. Create a blank sandbox
docker sandbox create --name my-openclaw shell .
docker sandbox network proxy my-openclaw --allow-host localhost
docker sandbox run my-openclaw

# 2. Inside sandbox — install Node.js 22 + OpenClaw
npm install -g n && n 22
hash -r
npm install -g openclaw@latest
openclaw setup
```

#### Networking Bridge Script

Node.js doesn't respect `HTTP_PROXY` env vars, so a bridge script forwards requests from `127.0.0.1:54321` through the sandbox proxy to Docker Model Runner at `localhost:12434`:

```javascript
// ~/bridge.js
const http = require("http");
const { URL } = require("url");

const PROXY = new URL(process.env.HTTP_PROXY || "http://host.docker.internal:3128");
const TARGET = "localhost:12434";

http.createServer((req, res) => {
  const proxyReq = http.request({
    hostname: PROXY.hostname,
    port: PROXY.port,
    path: "http://" + TARGET + req.url,
    method: req.method,
    headers: { ...req.headers, host: TARGET }
  }, proxyRes => {
    res.writeHead(proxyRes.statusCode, proxyRes.headers);
    proxyRes.pipe(res);
  });
  proxyReq.on("error", e => { res.writeHead(502); res.end(e.message); });
  req.pipe(proxyReq);
}).listen(54321, "127.0.0.1");
```

#### Gateway Configuration (openclaw.json)

```python
# Python script to configure OpenClaw with Docker Model Runner provider
import json
p = '$HOME/.openclaw/openclaw.json'
with open(p) as f: cfg = json.load(f)
cfg['models'] = cfg.get('models', {})
cfg['models']['mode'] = 'merge'
cfg['models']['providers'] = cfg['models'].get('providers', {})
cfg['models']['providers']['docker-model-runner'] = {
    'baseUrl': 'http://127.0.0.1:54321/engines/llama.cpp/v1',
    'apiKey': 'not-needed',
    'api': 'openai-completions',
    'models': [{
        'id': 'ai/qwen2.5:7B-Q4_K_M',
        'name': 'Qwen 2.5 7B (Docker Model Runner)',
        'reasoning': False, 'input': ['text'],
        'cost': {'input': 0, 'output': 0, 'cacheRead': 0, 'cacheWrite': 0},
        'contextWindow': 32768, 'maxTokens': 8192
    }]
}
cfg['agents'] = cfg.get('agents', {})
cfg['agents']['defaults'] = cfg['agents'].get('defaults', {})
cfg['agents']['defaults']['model'] = {'primary': 'docker-model-runner/ai/qwen2.5:7B-Q4_K_M'}
cfg['gateway'] = {'mode': 'local'}
with open(p, 'w') as f: json.dump(cfg, f, indent=2)
```

#### Save & Share Custom Image

```bash
# Save sandbox as reusable image
docker sandbox save my-openclaw my-openclaw-image:latest

# Push to Docker Hub
docker tag my-openclaw-image:latest yourname/my-openclaw:latest
docker push yourname/my-openclaw:latest

# Others can use it with:
docker sandbox create --name openclaw -t yourname/my-openclaw:latest shell .
```

### Part 3: Cloud Model Integration (10 minutes)
1. **Credential Injection**
   - `ANTHROPIC_API_KEY` / `OPENAI_API_KEY` environment variables
   - Network proxy auto-injects keys into outbound requests
   - Security: keys never exposed to the agent process itself

2. **Model Switching**
   - Local models: free, private, no API keys needed
   - Cloud models: for production / complex tasks
   - Same sandbox, different model endpoints

### Part 4: Technical Deep Dive (15-20 minutes)

**Architecture:** `Host ← Proxy (:3128) ← Sandbox ← Bridge (:54321) ← OpenClaw ← Model Runner (:12434)`

1. **Architecture Details**
   - Docker Model Runner binds to `localhost:12434`
   - Sandbox proxy at `host.docker.internal:3128`
   - Node.js HTTP proxy compatibility workaround via bridge script

2. **The Networking Bridge**
   - Node.js ignores `HTTP_PROXY` env vars (unlike curl/wget)
   - Bridge script (~20 lines) listens on `127.0.0.1:54321`
   - Forwards all requests through sandbox proxy to Model Runner

3. **Building Custom Images**
   - Base sandbox template: `shell`
   - Add Node.js 22, OpenClaw, bridge script
   - Save with `docker sandbox save` → share via Docker Hub

4. **Configuration & Startup**
   - Gateway config points provider to bridge at `:54321`
   - `start-openclaw.sh` orchestrates bridge + TUI launch
   - Model selection via CLI argument or OpenClaw settings

## 💡 Study Strategies

### For This Material:

**Conceptual Learning:**
- Create a mental model: Sandbox = VM → isolated environment
- Network proxy = gatekeeper between agent and outside world
- Bridge script = translation layer for Node.js HTTP

**Practical Learning:**
- Set up step-by-step with Docker Desktop (hands-on)
- Observe the Terminal UI interaction with model
- Test both local and cloud model switching

**Architecture Thinking:**
- Draw a diagram: Host ← Proxy ← Sandbox ← OpenClaw ← Bridge ← Model Runner
- Identify security boundaries at each layer
- Understand why each component is necessary

### Active Learning Techniques:

- **Spaced Repetition:** 
  - Day 1: Read about Docker Sandboxes
  - Day 3: Practice basic setup
  - Day 7: Build custom image
  - Day 14: Explain networking architecture to someone

- **Practice-Based:** 
  - [ ] Pull a Docker model and verify with `docker model ls`
  - [ ] Create a simple sandbox and run OpenClaw
  - [ ] Switch between local and cloud models
  - [ ] Inspect the bridge script and understand its purpose
  - [ ] Build a custom Docker image with your own tools

- **Teaching Others:** 
  - Explain to a colleague why sandboxes are more secure than standard containers
  - Describe the network proxy's role in credential injection
  - Walk someone through the quick start commands

## 🧠 Self-Assessment

### Knowledge Check (Week 1):

- [ ] **Q1:** What are the three main security features of Docker Sandboxes?
  - **Answer:** Micro VM isolation, network proxy control, environment variable injection
  
- [ ] **Q2:** What command enables Docker Model Runner?
  - **Answer:** Settings → Docker Model Runner → Enable (GUI), or config file edit
  
- [ ] **Q3:** Why does OpenClaw need a networking bridge?
  - **Answer:** Node.js doesn't respect HTTP_PROXY env vars; bridge forwards requests explicitly

- [ ] **Q4:** How are API keys protected in the sandbox?
  - **Answer:** Network proxy injects them; agent never sees/accesses the actual keys

- [ ] **Q5:** Can you switch models without recreating the sandbox?
  - **Answer:** Yes, via OpenClaw settings (local models) or environment variables (cloud)

### Knowledge Check (Final):

- [ ] **Practical Task 1:** Set up OpenClaw with a local model and screenshot the Terminal UI
- [ ] **Practical Task 2:** Document the setup process for a team member (write a mini-guide)
- [ ] **Practical Task 3:** Create a custom sandbox image with an additional CLI tool (e.g., Go, Rust)
- [ ] **Conceptual Task:** Draw and explain the network architecture (4-5 layers)
- [ ] **Integration Task:** Run OpenClaw with a cloud model and compare speed/cost vs. local

## 📊 Progress Tracking

**Completion Status:**
- [ ] Read/understand core concepts
- [ ] Complete quick start setup
- [ ] Answer Week 1 knowledge checks
- [ ] Perform at least 2 practical tasks
- [ ] Pass final self-assessment

**Estimated Progress:** 0%

**Checkpoints:**
- ✓ Day 1: Understand Docker Sandboxes concept
- ✓ Day 2-3: Hands-on setup (local model)
- ✓ Day 4-5: Cloud model integration & testing
- ✓ Day 6-7: Custom image building

**Next Milestone:** Complete Week 1 knowledge checks and setup a working OpenClaw instance

## 🔗 Reference Links

- [Run OpenClaw Securely in Docker Sandboxes](https://www.docker.com/blog/run-openclaw-securely-in-docker-sandboxes/) — Docker Blog (source article)
- [OpenClaw Docker Install Docs](https://docs.openclaw.ai/install/docker) — Official documentation
- [Simon Willison's OpenClaw Docker TIL](https://til.simonwillison.net/llms/openclaw-docker) — Condensed setup notes
- [OpenClaw on GitHub](https://github.com/openclaw/openclaw) — Source repository
- [Pre-built Docker Image](https://hub.docker.com/r/olegselajev241/openclaw-dmr) — `olegselajev241/openclaw-dmr:latest`

## 🔗 Related Notes

- `docker-containers-basics` — Foundational Docker concepts
- `ai-agents-overview` — General AI agent architecture
- `kubernetes-vs-docker` — When containers vs. VMs make sense
- `api-security-best-practices` — Credential handling patterns
- `node-js-http-proxy` — Advanced networking in Node.js environments
- `model-runner-setup` — Docker Model Runner installation guide

## 🏷️ Tags Analysis

**Content Analysis:**
- **Type**: `study-guide` (Structured learning material for skill development)
- **Topics**: 
  - `docker` (Container orchestration platform)
  - `AI` (Artificial intelligence agents & models)
  - `security` (Isolation, credential protection, sandboxing)
  - `development` (Setup, implementation, architecture)
  - `containers` (Docker-specific technology)
- **Difficulty**: `intermediate` (Requires prior Docker/containerization knowledge)
- **Characteristics**: 
  - `deep-dive` (Technical architecture explanation)
  - `technical` (Implementation-focused, hands-on labs)
- **Priority**: `high` (Relevant for secure AI agent deployment patterns)

**Why These Tags:**

This material covers **production-grade security** for AI agents (critical topic) + **emerging Docker technology** (sandboxes + model runner) + **hands-on implementation**. Medium-to-high priority for teams deploying AI coding agents.

**Suggested Obsidian Filters:**

```
type = study-guide AND tags contains "docker"
type = study-guide AND tags contains "security" AND difficulty = intermediate
tags contains "AI" AND tags contains "containers"
type = study-guide AND status = processing AND priority = high
```

## 🔍 Semantic Search Suggestions

- `/semantic-search "Docker Sandboxes AI agent isolation"`
- `/semantic-search "secure credential injection in containers"`
- `/semantic-search "Docker Model Runner local AI models"`
- `/semantic-search "network proxy containerized applications"`
- `/semantic-search "OpenClaw setup configuration"`

---

**Created:** 2026-02-24
**Status:** Processing - Active Study
**Last Updated:** 2026-02-24
**Next Action:** Begin with Part 1 conceptual study, then move to hands-on setup in Part 2
**Review Schedule:** Re-review on 2026-03-03 (Day 7) and 2026-03-10 (Day 14)
