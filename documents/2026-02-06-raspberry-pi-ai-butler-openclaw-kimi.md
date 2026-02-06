---
title: "Your Own AI Butler for $75? OpenClaw+Kimi K-2.5 on Raspberry Pi [Full Tutorial]"
tags: [video, AI, automation, raspberry-pi, tutorial, technical, inbox, development]
url: https://www.youtube.com/watch?v=KjxYpRkPT48
cover: https://i.ytimg.com/vi/KjxYpRkPT48/maxresdefault.jpg
date: 2026-02-06
video_date: 2025
type: video
status: inbox
read: false
priority: high
duration: ~25min
channel: N/A
---

# Your Own AI Butler for $75? OpenClaw+Kimi K-2.5 on Raspberry Pi [Full Tutorial]

[![Watch on YouTube](https://i.ytimg.com/vi/KjxYpRkPT48/maxresdefault.jpg)](https://www.youtube.com/watch?v=KjxYpRkPT48)

## 📖 Description

A comprehensive tutorial showing how to run OpenClaw (previously ClaudeBot) on a Raspberry Pi 5 with the Moonshot Kimi K-2.5 AI model. The video demonstrates the complete setup process from flashing the OS to configuring integrations with Telegram, API keys, and skills. The presenter emphasizes how Raspberry Pi is an ideal platform for tinkerers due to its ability to connect various sensors (temperature, cameras, pressure) to enhance the AI assistant's capabilities.

**Channel**: N/A
**URL**: https://www.youtube.com/watch?v=KjxYpRkPT48

## 🎯 Learning Objectives

By the end of this video, you will understand:
- How to set up a Raspberry Pi 5 as an AI assistant platform
- How to install and configure OpenClaw (formerly ClaudeBot)
- How to integrate Moonshot's Kimi K-2.5 AI model
- How to connect Telegram as a messaging interface
- How to configure various skills and API integrations (OpenAI Whisper, ElevenLabs TTS)
- How to set up automation tasks like news aggregation and job search
- Why Raspberry Pi is ideal for agentic AI applications

## 📋 Curriculum/Contents

### Hardware Setup
- **Raspberry Pi 5 introduction** - Credit card sized computer, $75 budget
- **Power requirements** - 5V @ 5A (25W) power brick recommended
- **SD card preparation** - Using Raspberry Pi Imager software

### OS Installation & Network Configuration
- **Downloading and installing Raspberry Pi OS** - 64-bit version
- **Pre-configuring Wi-Fi credentials** - For headless setup
- **Enabling SSH server** - Remote access setup
- **First boot and IP address discovery** - Using `ip addr` command
- **Two methods of SSH access** - hostname.local vs IP address

### OpenClaw Installation
- **One-liner installation** - Quick setup from official website
- **Dependency installation** - Node.js and npm packages
- **Onboarding process** - Initial configuration wizard
- **Safety warnings** - Sandbox environment recommendations

### API Configuration
- **Moonshot Kimi K-2.5 setup** - Creating and configuring API key
- **Model selection** - Choosing the latest kimi-k-2.5 model
- **API balance management** - Adding credits ($10 recharge shown)
- **OpenAI API integration** - For Whisper transcription
- **ElevenLabs TTS setup** - Voice response capability

### Telegram Bot Setup
- **BotFather interaction** - Creating new Telegram bot
- **Bot naming and token generation** - "JanskyTheBot" example
- **Token configuration** - Connecting OpenClaw to Telegram

### Skills & Features Configuration
- **Homebrew installation** - Package manager for additional tools
- **Package manager selection** - npm vs pnpm considerations
- **Skill selection process**:
  - OpenAI Whisper (voice transcription)
  - ElevenLabs (text-to-speech)
  - Command logger
  - Session memory

### Advanced Automation Examples
- **Morning news aggregation** - Cron job for tech, astronomy, world news
- **Job search automation** - Research paper tracking and job recommendations
- **Weather updates** - Daily briefings
- **Custom workflows** - Personalized automation setup

### Future Enhancements Teased
- **Sensor integration possibilities** - Temperature, pressure, camera sensors
- **Enhanced awareness** - Making the bot more "alive" with hardware inputs

## 📝 Notes & Key Takeaways

### Main Insights

1. **Raspberry Pi as AI Platform**: The $75 Raspberry Pi 5 offers a cost-effective alternative to Mac minis or VPS hosting for running persistent AI agents. Its compact size and sensor connectivity make it ideal for tinkerers.

2. **OpenClaw Ecosystem**: Formerly known as ClaudeBot, OpenClaw is a souped-up agentic AI assistant that runs perpetually on hardware. The platform has gained significant traction with users buying dedicated hardware.

3. **Kimi K-2.5 Performance**: The presenter reports positive experiences with Moonshot's latest model for job searches, research tracking, and trend following, with API costs around $14-24 for extended use.

4. **Headless Setup Advantages**: Pre-configuring Wi-Fi and SSH during OS imaging enables completely headless operation - no monitor, keyboard, or mouse needed after initial SD card preparation.

5. **Skills-Based Architecture**: OpenClaw uses a modular "skills" system allowing users to enable/disable capabilities like voice transcription (Whisper), TTS (ElevenLabs), command logging, and session memory.

6. **Automation Power**: The platform enables setting up cron jobs for daily tasks like news aggregation, job searching, research paper discovery, and weather updates - creating a true "AI butler" experience.

7. **Multi-Model Integration**: While running Kimi K-2.5 as the primary model, the system can leverage OpenAI's Whisper for transcription and ElevenLabs for voice synthesis, showing flexible architecture.

### Actionable Points

- **Hardware**: Purchase Raspberry Pi 5 kit with 25W power supply (5V/5A)
- **Storage**: Get a quality SD card for OS installation
- **Accounts needed**:
  - Moonshot AI account with API credits (~$10 to start)
  - Telegram account for bot interface
  - Optional: OpenAI account for Whisper, ElevenLabs for TTS
- **Network**: Ensure stable Wi-Fi for reliable operation
- **Customization**: Start with basic skills, add sensors later for enhanced capabilities
- **Cost breakdown**: ~$75 hardware + ~$10-20/month API usage for moderate use

### Personal Reflections

*Add your own notes here after watching*

## ⭐ Rating & Review

After completion:
- **Quality (1-5):** 4/5
- **Relevance (1-5):** 5/5
- **Would recommend:** Yes
- **Best for:** Technical enthusiasts, developers interested in AI agents, makers who want hands-on AI integration, those seeking cost-effective alternatives to cloud-hosted assistants

## 🏷️ Auto-Generated Tags

**Content Analysis:**
- **Type**: `video` (YouTube tutorial)
- **Topics**: AI automation, Raspberry Pi hardware, OpenClaw/ClaudeBot setup, API integration, edge computing
- **Complexity**: Intermediate to Advanced - requires comfort with terminal, SSH, API configuration
- **Priority**: High - timely content on trending AI agent platform with practical implementation

**Why These Tags:**
- `AI` - Core focus on AI assistant technology
- `automation` - Emphasis on automated workflows and cron jobs
- `raspberry-pi` - Hardware platform for the entire tutorial
- `tutorial` - Step-by-step instructional content
- `technical` - Requires technical knowledge and hands-on implementation
- `development` - Software setup and configuration for developers
- `inbox` - New content requiring review

**Suggested Bases Filters:**
- Find similar content: `type = video AND tags contains "AI" AND tags contains "raspberry-pi"`
- Find high-priority learning: `priority = high AND status = inbox`
- Technical tutorials: `tags contains "tutorial" AND tags contains "technical"`

## 🔗 Related Topics & Further Research

**Related Searches:**
- OpenClaw official documentation and GitHub repository
- Moonshot AI Kimi K-2.5 API documentation
- Raspberry Pi 5 sensor integration guides
- Telegram Bot API documentation
- Alternative AI models compatible with OpenClaw
- Comparison: Raspberry Pi vs VPS vs Mac Mini for AI hosting
- Privacy considerations for self-hosted AI assistants
- Advanced automation patterns with agentic AI

**Keywords for further exploration:**
- Edge AI deployment
- Agentic workflows
- Self-hosted AI assistants
- Raspberry Pi sensor projects
- API cost optimization
- Voice-enabled AI assistants

---

**Captured**: 2026-02-06
**Source**: https://www.youtube.com/watch?v=KjxYpRkPT48
**Channel**: N/A

**Connection to Other Notes:**
This tutorial provides practical implementation details for running AI agents on edge devices, complementing broader knowledge about AI automation, self-hosting, and privacy-focused computing. Consider connecting to notes about OpenAI API usage, Telegram bot development, Raspberry Pi projects, and cost-effective AI deployment strategies.
