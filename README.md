# Viuz for Claude

Viuz is a social content system for creators and brands: it plans the week, designs carousels, single images and stories in the brand's own look, writes Reel scripts, and schedules posts to Instagram, TikTok, Threads, X and LinkedIn. This plugin connects Claude to your Viuz workspace and teaches Claude how to work with it: ask whose art it is before spending anything, use your photo library first, mix in images made elsewhere, and never publish without your say.

## Install

In Claude Code:

```
/plugin marketplace add viuz-ai/claude-plugins
/plugin install viuz@viuz
```

Then run `/reload-plugins`, open `/mcp`, choose `viuz` and select Authenticate. You sign in to Viuz in the browser with OAuth and approve the connection; no API key is needed. Check it worked by asking Claude to list your brands.

Turn on auto-update so the skill stays current: open the `/plugin` menu, find the `viuz` marketplace and enable auto-update for it. Third-party marketplaces have it off by default.

If you already added the Viuz connector by hand, remove it after installing the plugin. Otherwise every tool appears twice.

## What is inside

- `viuz`, a plugin with the Viuz connector (`https://api.viuz.ai/mcp`) and one skill, `viuz-copilot`, in English with a Brazilian Portuguese copy. Claude answers in your language.

## Plans

Some tools need the Studio plan, and some need a permission you grant when you connect. If Claude says a tool is missing, that is the reason; open Connections in Viuz to review what you allowed.

## Source and license

The skill text lives in the Viuz repository and this repository is generated from it on every release. MIT license. Product: https://viuz.ai
