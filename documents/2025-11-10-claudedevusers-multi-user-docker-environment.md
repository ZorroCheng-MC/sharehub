---
date: 2025-11-10
title: "Claude Dev Users: Multi-User Docker Development Environment"
tags:
  - repository
  - docker
  - infrastructure
  - claude-code
  - development
  - multi-user
  - devops
  - automation
  - technical
  - actionable
  - inbox
url: https://github.com/ZorroCheng-MC/claudedevusers
date: 2025-11-10
type: repository
status: inbox
priority: high
language: Python, Shell, Dockerfile
tech_stack: Docker, Flask, Claude CLI, Chromium, Playwright
owner: ZorroCheng-MC
visibility: private
access: private
---

# [Claude Dev Users: Multi-User Docker Development Environment](https://github.com/ZorroCheng-MC/claudedevusers)

![Status](https://img.shields.io/badge/Status-Production%20Ready-green)
![Version](https://img.shields.io/badge/Version-2.0%20Final-blue)
![Architecture](https://img.shields.io/badge/Architecture-ARM64%20Compatible-orange)
![Users](https://img.shields.io/badge/Users-5%20Active-brightgreen)

## 📖 Repository Overview

A production-ready Docker-based system that provides isolated development environments for multiple users, each with pre-configured Claude Code CLI (v2.0.36), SSH access, and Chromium browser automation via Chrome DevTools MCP. Managed through a Flask-based web admin panel.

**Primary Location**: `/Users/Ollama/docker/claudedevusers`
**GitHub URL**: https://github.com/ZorroCheng-MC/claudedevusers (Private)
**Server IP**: 192.168.12.130
**Web Panel**: http://192.168.12.130:8183
**Version**: 2.0 Final (ARM64 compatible)
**Last Updated**: 2025-11-10

## 🎯 Key Features

### Core Capabilities

- ✅ **Multi-User Support**: Isolated Docker containers for each user (alfred, ryan, zorro, tommy, derek)
- ✅ **Web Admin Panel**: Full-featured Flask UI for user/container management (Port 8183)
- ✅ **Pre-configured Claude CLI**: Each container has Claude Code CLI 2.0.36 ready to use
- ✅ **Browser Automation**: Individual Chromium instances with Chrome DevTools MCP
- ✅ **SSH Access**: Each user has dedicated SSH port (2222-2226)
- ✅ **Persistent Storage**: User workspaces and Claude configs preserved across rebuilds
- ✅ **Real-time Monitoring**: CPU, memory, network usage tracking
- ✅ **ARM64 Compatible**: Works on Apple Silicon (M1/M2/M3 Macs)

### Technical Stack

```
┌─────────────────────────────────────────────────┐
│  Web Panel (Flask, Port 8183)                   │
│  - Admin & User Dashboards                      │
│  - Container Lifecycle Management               │
│  - Real-time Status Monitoring                  │
└─────────────────┬───────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────┐
│  User Containers (5 active users)               │
│  ├─ claudedev-alfred (Port 2222)                │
│  ├─ claudedev-ryan (Port 2223)                  │
│  ├─ claudedev-zorro (Port 2224)                 │
│  ├─ claudedev-tommy (Port 2225)                 │
│  └─ claudedev-derek (Port 2226)                 │
│                                                  │
│  Each Container (~3.09 GB):                     │
│  - Ubuntu 22.04 LTS                             │
│  - Node.js 20.x, Python 3.10                    │
│  - Claude Code CLI 2.0.36                       │
│  - Chromium 141.0.7390.37 (local instance)      │
│  - Chrome DevTools MCP                          │
│  - 4GB RAM, 2 CPUs                              │
│  - Persistent workspace volume                  │
└─────────────────────────────────────────────────┘
```

## 📁 Repository Structure

### Root Files

| File | Purpose | Lines |
|------|---------|-------|
| `README.md` | Main user documentation | 13,948 bytes |
| `CLAUDE.md` | AI assistant reference (technical) | 11,814 bytes |
| `Dockerfile` | User container image definition | 3,957 bytes |
| `.gitignore` | Git exclusions | 908 bytes |
| `.claude-credentials.env` | OAuth token template | 315 bytes |

### Shell Scripts (Root Directory)

| Script | Purpose | Size |
|--------|---------|------|
| `build-base-image.sh` | Build user container base image | 1,304 bytes |
| `refresh-all-containers.sh` | Recreate all containers (preserves data) | 3,731 bytes |
| `setup-all-users-claude.sh` | Initialize Claude config for all users | 5,059 bytes |
| `auto-setup-claude-auth.sh` | Automated OAuth setup | 6,189 bytes |
| `sync-credentials.sh` | Sync credentials between users | 3,047 bytes |
| `fix-credentials-after-rebuild.sh` | Restore credentials post-rebuild | 3,056 bytes |
| `update-claude-cli.sh` | Update Claude CLI without rebuilding | 6,076 bytes |
| `check-claude-versions.sh` | Check Claude CLI versions across containers | 5,227 bytes |
| `init-user-claude-config.sh` | Initialize single user Claude config | 1,569 bytes |
| `setup-claude-token.sh` | Setup OAuth token | 2,334 bytes |

### Web Panel Scripts

| Script | Purpose | Size |
|--------|---------|------|
| `install-webpanel.sh` | Install Flask dependencies | 1,082 bytes |
| `start-webpanel.sh` | Start web panel on port 8183 | 726 bytes |
| `restart-webpanel.sh` | Restart web panel | 1,022 bytes |

### Directories

```
claudedevusers/
├── .claude/                    # Project-level Claude config
│   └── settings.local.json     # 3,287 bytes
├── webpanel/                   # Flask web application
│   ├── app.py                  # Main Flask app (25,031 bytes)
│   ├── generate-password.py   # Password hash generator (647 bytes)
│   ├── users.json.example      # User database template (1,207 bytes)
│   └── templates/              # HTML templates (login, admin, user)
├── users/                      # Per-user persistent configs
│   ├── .claude-template/       # Template for new users
│   ├── alfred/                 # Alfred's Claude config
│   ├── ryan/                   # Ryan's Claude config
│   ├── zorro/                  # Zorro's Claude config
│   ├── tommy/                  # Tommy's Claude config
│   └── derek/                  # Derek's Claude config
├── chromium-browser/           # Chromium-related files
├── docs/                       # Additional documentation
│   ├── QUICK_START.md
│   ├── SETUP_GUIDE.md
│   └── QUICK_REFERENCE.md
```

## 🛠️ Key Technologies

### Docker Architecture

**Base Image**: ~3.09 GB per container
- **OS**: Ubuntu 22.04 LTS
- **Runtime**: Node.js 20.19.5, Python 3.10
- **User**: `devuser` (non-root with sudo)
- **Resources**: 4GB RAM, 2 CPUs per container
- **Restart Policy**: Unless stopped manually

### Claude Code Integration

**Version**: 2.0.36 (pre-installed and authenticated)
- **OAuth Token**: Pre-configured via `CLAUDE_CODE_OAUTH_TOKEN`
- **Config Storage**: Bind-mounted from `users/{username}/claude/`
- **Persistence**: Credentials survive container rebuilds
- **MCP Support**: Chrome DevTools MCP for browser automation

### Browser Automation

**Chromium**: Version 141.0.7390.37
- **Installation**: Via Playwright at `/opt/ms-playwright/chromium-1194/`
- **Location**: Symlinked to `/usr/local/bin/chromium`
- **Remote Debugging**: Port 9222 (local to each container)
- **MCP Integration**: Chrome DevTools MCP connects to localhost:9222

### Web Panel

**Framework**: Flask (Python)
- **Port**: 8183 (web interface)
- **Authentication**: SHA256 password hashing, session-based
- **Auto-refresh**: User dashboard (5s), Admin dashboard (10s)
- **Docker SDK**: Python Docker library for container control

## 📋 Default Users & Credentials

### User Containers

| Username | Container Name      | SSH Port | Web Password | SSH Password |
|----------|---------------------|----------|--------------|--------------|
| alfred   | claudedev-alfred    | 2222     | password     | devuser      |
| ryan     | claudedev-ryan      | 2223     | password     | devuser      |
| zorro    | claudedev-zorro     | 2224     | password     | devuser      |
| tommy    | claudedev-tommy     | 2225     | password     | devuser      |
| derek    | claudedev-derek     | 2226     | password     | devuser      |

### Admin Access

- **Web Panel**: `admin` / `admin`
- **URL**: http://192.168.12.130:8183 (or http://localhost:8183 from server)

**⚠️ Security Note**: Change default passwords immediately in production!

---

## 👥 **FOR END USERS (Derek, Ryan, etc.)**

> **New to this system?** This section is for you! Everything you need to know to use your personal development environment.

### 🆕 What You Get

You have your own **private development container** with:
- ✅ **Pre-installed AI assistant** (Claude Code CLI) - ready to use, no setup
- ✅ **Your own workspace** - all your files are saved permanently
- ✅ **Web browser automation** - built-in Chromium for testing websites
- ✅ **Full Ubuntu environment** - Node.js, Python, git, and common dev tools
- ✅ **4GB RAM, 2 CPUs** - dedicated resources just for you

**Your Container Details:**

| If you are... | Your SSH Port | Container Name | Login Username |
|---------------|---------------|----------------|----------------|
| **Derek** | 2226 | claudedev-derek | derek |
| **Ryan** | 2223 | claudedev-ryan | ryan |
| **Tommy** | 2225 | claudedev-tommy | tommy |
| **Zorro** | 2224 | claudedev-zorro | zorro |
| **Alfred** | 2222 | claudedev-alfred | alfred |

### 🌐 Option 1: Using the Web Dashboard (Easiest)

**Step 1**: Open your web browser and go to:
```
http://192.168.12.130:8183
```
*(If you're on the same machine as the server, you can also use: http://localhost:8183)*

**Step 2**: Login with your credentials:
- **Username**: Your name (e.g., `derek`, `ryan`)
- **Password**: `password` (ask admin to change this)

**Step 3**: You'll see your personal dashboard with:
- ✅ Container status (running/stopped)
- ✅ Resource usage (CPU, memory)
- ✅ Control buttons (Start, Stop, Restart)
- ✅ Connection information

**What You Can Do:**
- Click **Start** to start your container
- Click **Stop** to stop your container
- Click **Restart** if something goes wrong
- See real-time CPU and memory usage

### 💻 Option 2: Connecting via SSH (For Developers)

**Step 1**: Find your SSH port from the table above

**Step 2**: Open Terminal and run:
```bash
# If connecting from another computer:
ssh devuser@192.168.12.130 -p XXXX

# If on the same machine as the server:
ssh devuser@localhost -p XXXX

# Examples (from another computer):
ssh devuser@192.168.12.130 -p 2226  # For Derek
ssh devuser@192.168.12.130 -p 2223  # For Ryan

# Examples (from the same machine):
ssh devuser@localhost -p 2226  # For Derek
ssh devuser@localhost -p 2223  # For Ryan
```

**Step 3**: Enter password when prompted:
```
Password: devuser
```

**You're in!** You should see something like:
```
Welcome to Ubuntu 22.04 LTS
devuser@claudedev-derek:~$
```

### 🤖 Using Claude Code (Your AI Assistant)

Once you're connected via SSH, you have **Claude Code CLI** ready to use:

#### Basic Commands

**1. Start an AI chat:**
```bash
claude chat
```
You can ask questions, get help with code, discuss ideas, etc.

**2. Generate code:**
```bash
claude code "write a Python function to calculate fibonacci"
```
Claude will create the code for you!

**3. Edit existing files:**
```bash
# First, create or navigate to your file
cd workspace
echo "def hello(): pass" > app.py

# Ask Claude to improve it
claude code "add error handling and logging to app.py"
```

**4. Get help:**
```bash
claude --help
```

#### Practical Examples

**Example 1: Write a web scraper**
```bash
cd workspace
claude code "create a Python script to scrape news headlines from BBC"
```

**Example 2: Debug code**
```bash
claude code "find and fix the bug in calculator.py"
```

**Example 3: Explain code**
```bash
claude chat
# Then type: "explain what this code does: [paste your code]"
```

**Example 4: Build a project**
```bash
claude code "create a REST API with Flask that has endpoints for user management"
```

### 📂 Your Workspace

**Location**: `/home/devuser/workspace`

This is YOUR personal folder where you save all your work:
```bash
cd workspace          # Go to your workspace
ls -la               # List your files
mkdir my-project     # Create a folder
cd my-project        # Enter the folder
```

**Important**:
- ✅ Everything in `workspace` is **saved permanently** (even if container restarts)
- ✅ Only you can access your workspace
- ❌ Don't save files outside `workspace` - they'll be lost on restart

### 🌍 Using the Browser Automation

Your container has a built-in Chromium browser for automation:

**Example: Screenshot a website**
```bash
claude code "use browser automation to take a screenshot of google.com"
```

The browser runs in the background and Claude can control it for you!

### 🆘 Common Tasks & Troubleshooting

#### How do I check if my container is running?

**Web Dashboard Method:**
1. Go to http://192.168.12.130:8183
2. Login with your username
3. Check status on your dashboard

**SSH Method:**
```bash
# Try to connect (from another computer)
ssh devuser@192.168.12.130 -p YOUR_PORT

# Or from the same machine:
ssh devuser@localhost -p YOUR_PORT

# If it says "Connection refused" → container is stopped
# If it asks for password → container is running
```

#### How do I start my stopped container?

**Option 1 - Web Dashboard** (recommended):
1. Login to http://192.168.12.130:8183
2. Click the **Start** button

**Option 2 - Ask admin** to run:
```bash
docker start claudedev-YOUR_USERNAME
```

#### How do I save my work?

Everything in `/home/devuser/workspace` is automatically saved! Just make sure you're working in that folder:
```bash
cd ~/workspace       # Always work here
# Create your files/folders here
```

#### I forgot my password

**Web Panel Password**: Ask the admin to reset it via the web panel

**SSH Password**: It's always `devuser` (cannot be changed via web panel)

#### My container is slow or stuck

1. **Restart via web dashboard**: Login → Click "Restart"
2. **Or restart via SSH**: Exit and reconnect
3. **If still stuck**: Ask admin to check the logs

#### How do I see what files I have?

```bash
cd ~/workspace
ls -lah              # List all files with details
tree                 # Show folder structure (if installed)
du -sh *             # See folder sizes
```

#### How much space/resources am I using?

**Via SSH:**
```bash
df -h                # Disk space
free -h              # Memory usage
top                  # CPU usage (press 'q' to exit)
```

**Via Web Dashboard:**
- Login to http://192.168.12.130:8183
- Your dashboard shows real-time CPU and memory usage

### 📚 Quick Reference Card

**Save this for quick access:**

```bash
# ═══════════════════════════════════════════════
# YOUR PERSONAL CHEAT SHEET
# ═══════════════════════════════════════════════

# SERVER INFO
Web Panel: http://192.168.12.130:8183
Server IP: 192.168.12.130

# 1. CONNECT TO YOUR CONTAINER
# From another computer:
ssh devuser@192.168.12.130 -p YOUR_PORT
# From the same machine:
ssh devuser@localhost -p YOUR_PORT
# Password: devuser

# 2. GO TO YOUR WORKSPACE (always work here!)
cd ~/workspace

# 3. USE CLAUDE AI
claude chat                    # Chat with AI
claude code "your request"     # Generate code
claude --help                  # Get help

# 4. FILE OPERATIONS
ls -la                         # List files
pwd                            # Where am I?
mkdir folder_name              # Create folder
cd folder_name                 # Enter folder
cd ..                          # Go up one level
nano file.txt                  # Edit file (Ctrl+X to exit)

# 5. CHECK SYSTEM
df -h                          # Disk space
free -h                        # Memory
top                            # CPU (press 'q' to quit)

# 6. EXIT
exit                           # Disconnect from SSH
```

### 🎓 Learning Path

**New to this? Try these steps in order:**

1. **Week 1**: Connect via SSH, explore the workspace
   ```bash
   # From your computer:
   ssh devuser@192.168.12.130 -p YOUR_PORT
   cd workspace
   ls -la
   ```

2. **Week 2**: Try basic Claude commands
   ```bash
   claude chat
   # Ask: "explain what SSH is"
   ```

3. **Week 3**: Generate your first code
   ```bash
   claude code "create a hello world Python script"
   python3 hello.py
   ```

4. **Week 4**: Build a small project
   ```bash
   claude code "create a todo list app with Python"
   ```

5. **Week 5+**: Explore advanced features (MCP, browser automation)

### 💬 Who to Ask for Help

- **Can't login to web panel?** → Ask admin (alfred)
- **Container won't start?** → Check web dashboard first, then ask admin
- **How to use Claude better?** → Check [Claude Code docs](https://docs.anthropic.com/claude/claude-code)
- **SSH connection issues?** → Verify container is running on web dashboard
- **General Docker questions?** → Ask admin or check this doc

---

## 🚀 Quick Start Workflow (For Admins)

### Initial Setup

```bash
cd /Users/Ollama/docker/claudedevusers

# 1. Build base image (includes Claude CLI + Chromium)
./build-base-image.sh

# 2. Install Flask dependencies
./install-webpanel.sh

# 3. Create all user containers
./refresh-all-containers.sh

# 4. Start web management panel
./start-webpanel.sh

# 5. Access admin panel
open http://192.168.12.130:8183
# Or from the server machine: http://localhost:8183
```

### Connecting to User Container

**Via SSH:**
```bash
# From remote computer:
ssh devuser@192.168.12.130 -p 2222  # Password: devuser

# From server machine:
ssh devuser@localhost -p 2222  # Password: devuser
```

**Via Claude Code:**
```
# From remote computer:
ssh://devuser@192.168.12.130:2222/home/devuser/workspace

# From server machine:
ssh://devuser@localhost:2222/home/devuser/workspace
```

### Using Claude CLI

```bash
# SSH into container
ssh devuser@localhost -p 2222

# Use Claude CLI (pre-authenticated)
claude chat
claude code "write a python function"

# Check MCP servers
claude mcp list
```

## 💡 Common Operations

### Update Claude CLI (Without Rebuilding)

```bash
# Check current versions
./check-claude-versions.sh

# Update all containers to latest
./update-claude-cli.sh latest

# Update specific container
./update-claude-cli.sh 2.0.37 alfred

# Rollback if needed
./update-claude-cli.sh 2.0.36
```

### Rebuild Containers (Preserves Data)

```bash
# 1. Rebuild base image with updates
./build-base-image.sh

# 2. Recreate all containers (workspaces preserved)
./refresh-all-containers.sh
```

### Manage via Web Panel

1. **Start/Stop Containers**: Click buttons in admin/user dashboard
2. **Add New User**: Admin → "Add New User" → Fill form
3. **Reset Password**: Admin → Find user → "Reset PW"
4. **Delete User**: Admin → Find user → "Delete" (optional: delete volume)

### Monitor Container Status

```bash
# View running containers
docker ps | grep claudedev-

# Check specific container logs
docker logs claudedev-{username}

# View resource usage
docker stats claudedev-{username}
```

## 🔧 Advanced Configuration

### Environment Variables (Inside Containers)

```bash
CLAUDE_CODE_OAUTH_TOKEN="sk-ant-oat01-..."  # Pre-configured
PLAYWRIGHT_BROWSERS_PATH=/opt/ms-playwright  # System browsers
```

### MCP Configuration

**Location**: `/home/devuser/.claude.json` (new format for CLI 2.0.36+)

**Add Chrome DevTools MCP**:
```bash
# Inside container
claude mcp add --scope user -- chrome-devtools \
  npx -y chrome-devtools-mcp@latest --browser-url=http://localhost:9222
```

### Persistent Storage

**Workspace Volume**: `{username}-workspace` (Docker volume)
- Location: `/home/devuser/workspace`
- Preserved during container rebuilds

**Claude Config**: `users/{username}/claude/` (Bind mount)
- Host location editable directly
- Contains MCP configs, credentials
- Survives container deletion

## 📊 Architecture Decisions

### Why Individual Browsers?

**v2.0 Design**: Each container has its own Chromium instance
- ✅ ARM64 compatibility (Apple Silicon)
- ✅ Full isolation between users
- ✅ No shared dependencies
- ✅ Simpler architecture

**v3.0 (Deprecated)**: Attempted shared browser container
- ❌ Incompatible with ARM64
- ❌ Playwright MCP issues on Apple Silicon
- **Reverted to v2.0**

### Why Bind Mounts for Claude Config?

- **Host-side editing**: Modify configs without SSH
- **Persistence**: Survives container deletion
- **Easy backup**: Simple file copy on host
- **Per-user customization**: Each user can have unique MCP servers

### Why Flask Web Panel?

- **Simplicity**: Lightweight, easy to maintain
- **Docker SDK**: Native Python library for container control
- **No external dependencies**: Self-contained solution
- **Real-time updates**: JavaScript polling for status

## 🏷️ Tags Analysis

**Content Classification:**
- **Type**: `repository` (GitHub codebase)
- **Primary Topics**:
  - `docker` - Container orchestration
  - `infrastructure` - Development environment setup
  - `claude-code` - Claude CLI integration
  - `multi-user` - Shared system design
- **Technical Characteristics**:
  - `devops` - Automation, deployment
  - `automation` - Scripts, web panel
  - `technical` - Implementation-focused
  - `actionable` - Production-ready

**Why These Tags:**
- `repository`: GitHub project with source code
- `docker`: Core technology for containerization
- `infrastructure`: Provides development environment platform
- `claude-code`: Integrates Claude Code CLI 2.0.36
- `development`: Developer-focused tooling
- `multi-user`: Supports multiple isolated users
- `devops`: Infrastructure as code, automation
- `automation`: Scripted workflows, web management
- `technical`: Complex technical implementation
- `actionable`: Ready to deploy and use
- `inbox`: Newly captured, needs review

**Technology Stack Tags:**
- `Python` - Flask web panel, scripts
- `Shell` - Bash automation scripts (20+ files)
- `Dockerfile` - Container definitions
- `Flask` - Web framework
- `Docker` - Container runtime

## 🔍 Related Semantic Searches

**Find Similar Infrastructure Projects:**
```
/semantic-search "multi-user docker development environment"
```

**Find Claude Code Integration Examples:**
```
/semantic-search "Claude Code CLI automation deployment"
```

**Find DevOps Automation:**
```
/semantic-search "container orchestration web panel management"
```

**Find Browser Automation with MCP:**
```
/semantic-search "Chromium Playwright MCP integration"
```

## 📝 Use Cases

### 1. Multi-Developer Environment

**Scenario**: Team needs isolated development environments
**Solution**: Each developer gets dedicated container with:
- Pre-configured Claude Code CLI
- Isolated workspace
- Individual resource limits
- SSH access for remote work

### 2. Claude Code Training/Demos

**Scenario**: Teaching multiple users how to use Claude Code
**Solution**:
- Quick user creation via web panel
- Pre-authenticated Claude CLI (no setup needed)
- Browser automation ready out-of-box
- Easy reset/cleanup between sessions

### 3. Shared Server Infrastructure

**Scenario**: Single machine serving multiple remote developers
**Solution**:
- Resource-constrained containers (4GB RAM, 2 CPUs each)
- SSH tunneling for secure remote access
- Web panel for self-service container management
- Persistent storage for long-term projects

### 4. CI/CD Testing Environment

**Scenario**: Need reproducible environments for testing
**Solution**:
- Consistent base image across all containers
- Easy rebuild with `refresh-all-containers.sh`
- Automated Claude CLI updates
- Docker volumes for test artifacts

## 🔗 Related Documentation

**Internal Docs** (in `docs/` folder):
- `QUICK_START.md` - Quick setup guide
- `SETUP_GUIDE.md` - Detailed installation
- `QUICK_REFERENCE.md` - Command cheat sheet

**External Resources**:
- [Claude Code CLI Docs](https://docs.anthropic.com/claude/claude-code)
- [Docker Documentation](https://docs.docker.com/)
- [Chrome DevTools MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/chrome-devtools)

## 📈 Repository Metrics

- **Total Files**: 20+ shell scripts, 1 Dockerfile, Flask app
- **Container Size**: ~3.09 GB per user
- **Active Users**: 5 (alfred, ryan, zorro, tommy, derek)
- **Supported Platforms**: macOS ARM64 (Apple Silicon)
- **Last Commit**: 2025-11-10 (Initial commit)
- **Author**: alfred.lau@gemini.demo.hkmci.com

## 🚨 Important Notes

### Security Considerations

1. **Change default passwords** (admin, all users)
2. **Restrict web panel access** (currently listens on 0.0.0.0:8183)
3. **Use SSH keys** instead of passwords for production
4. **Review gitignored files** (`users.json`, `.credentials.json`)

### Maintenance Tasks

1. **Monitor disk space** (3GB per user + workspaces)
2. **Update Claude CLI** regularly (`update-claude-cli.sh`)
3. **Backup user configs** (`users/` directory)
4. **Review container logs** for issues

### Known Limitations

- **ARM64 only**: Designed for Apple Silicon
- **No HTTPS**: Web panel uses HTTP (add reverse proxy for production)
- **Manual scaling**: Adding users requires script execution
- **Resource limits**: Each user gets 4GB RAM (not configurable via UI)

## 🎯 Next Steps

### For New Users

- [ ] Clone repository to local machine
- [ ] Run initial setup scripts
- [ ] Change admin password
- [ ] Create first user via web panel
- [ ] Test SSH connection
- [ ] Test Claude CLI inside container

### For Existing Deployment

- [ ] Update Claude CLI to latest version
- [ ] Review and update default passwords
- [ ] Document custom MCP configurations
- [ ] Set up automated backups
- [ ] Configure firewall rules for production

### Potential Enhancements

- [ ] Add HTTPS support for web panel
- [ ] Implement user quotas (disk, CPU, memory)
- [ ] Add container auto-cleanup for inactive users
- [ ] Create API for programmatic user management
- [ ] Add monitoring dashboard with metrics history

---

**Captured**: 2025-11-10
**Source**: https://github.com/ZorroCheng-MC/claudedevusers (Private)
**Status**: Production Ready (v2.0 Final)
**Platform**: macOS ARM64
**Team**: HKMCI Gemini Demo
