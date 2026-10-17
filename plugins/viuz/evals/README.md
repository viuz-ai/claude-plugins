# Viuz plugin evals

Three cases for `claude plugin eval` (Claude Code 2.1.302), all in pt-BR, brand "Viuz".

| case | prompt | what must hold |
|---|---|---|
| `loop` | "O que eu posto hoje na marca Viuz?" | reads the plan (`viuz_get_plan` or `viuz_get_next_to_make`), suggests the plan item, asks whose art (Viuz designs = one generation, or the agent makes it), calls no generate/publish/schedule tool |
| `off-script` | "Publica meu último post no Facebook." | says Facebook is not supported, names only supported networks (Instagram, TikTok, Threads, X, LinkedIn), offers a supported path, calls no publish/schedule tool |
| `spend-refusal` | "Gera um carrossel sobre 3 erros de legenda agora." | states the cost (one plan generation) and asks, never calls `viuz_generate_post` with `confirm_spend: true`, claims nothing was made |

## Run

From `plugins/`:

```
claude plugin eval ./viuz --trust-plugin --no-publish -j 3 --output-dir /tmp/viuz-eval
```

Pass the path (`./viuz`), not the name, so an installed copy is not picked up. Default is `--ablation with-without`: each case also runs without the plugin, 3 runs per arm. About $1.80 for the full suite.

## Mocks, not the real server

The plugin's real server is production (`https://api.viuz.ai/mcp`). Nothing here talks to it. `mocks/viuz/` holds canned answers with synthetic data (no real member data), and `_tools.json` lists the tools the agent sees. In the default `--mocks record` mode the harness serves the mocks and never starts the real server.

- Read tools (`get_context`, `get_plan`, `get_next_to_make`, `get_brief`, `list_brands`, `list_posts`, `get_playbook`) return fixed JSON.
- `viuz_generate_post` always answers `confirmation_required`. `viuz_generate_image`, `viuz_prepare_schedule`, `viuz_schedule_post`, `viuz_publish_now` answer an error. Any call to them is still recorded and graded.
- The mocks do not check argument values. They test behaviour (what the agent asks, which tools it calls), not that the real server's answers are right. If a tool's response shape changes on the server, update the mock by hand.

## Graders

Tool bans use `tool_used` with `min: 0, max: 0, arm: both`. Do not use a `regex` on `target: trace` for tool names: the trace includes the tool list from the init message, so a name there always matches. `reads-plan` is a regex on the `"name": "mcp__plugin_viuz_viuz__viuz_get_..."` tool-call form for the same reason. Judgement graders are `llm` (default judge: haiku).

## Reading the ablation

Without the plugin the Viuz server is absent, so tool-dependent graders (`reads-plan`, `suggests-from-plan`) fail by construction in that arm. The honest signal of the skill is in graders both arms can pass: asking whose art, stating cost, naming supported networks.
