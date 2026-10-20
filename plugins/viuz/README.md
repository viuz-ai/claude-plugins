# Viuz plugin

Connects Claude to your Viuz workspace and adds one skill, `viuz-copilot`, that teaches Claude how to plan the week, make carousels, single images, stories and Reel scripts in your brand's look, and schedule posts to Instagram, TikTok, Threads, X and LinkedIn.

## What it connects to and what it sends

- One MCP server: `https://api.viuz.ai/mcp`. You sign in to Viuz with OAuth in the browser; the plugin holds no API key and reads no file or credential from your machine.
- Claude sends Viuz only what a tool call carries: your requests, the brand content you work on, and the files you choose to upload. Your brand content stays in Viuz under its privacy policy: https://viuz.ai/privacy
- Nothing publishes without you. Spending a plan generation or an AI image asks first; direct publishing is a separate permission you grant in Viuz.

## Install

In Claude Code 2.1.275 or later:

```
/plugin install viuz --marketplace viuz-ai/claude-plugins
```

Then open `/mcp`, choose `viuz` and select Authenticate. More setup notes are in the repository README. Product: https://viuz.ai
