# Changelog

## 1.3.3 (2026-10-20)

- Packaging for the Claude directory: a listing icon (`.claude-plugin/icon.png`), `privacyPolicyUrl` in `plugin.json`, and a README inside `plugins/viuz/` that says what the plugin sends. The eval cases moved out of the shipped plugin into the Viuz repository (`evals/claude-plugin/`).
- Saving: news with a deadline (a launch, a price, an event, a promotion). If `viuz_save_context` lists `valid_until` and `renew`, offer once per conversation to save it AFTER the task, with the date in the same question, only on an explicit yes, never for a fact that does not expire or for revenue or a named client; `renew` extends a deadline; say when a note is `only_when_picked`. Behind `MCP_CONTEXT_VALID_UNTIL_ENABLED`; the skill says "if listed" like the other flagged tools.

## 1.3.2 (2026-10-19)

- Covers: the still life mode is named (one everyday object of the brand's field on the brand's colour, a giant word Viuz writes, not taken from the headline, a line and a handwritten aside; needs `post_id`; if available), with when to recommend it. It is one of four modes, never the default.

## 1.3.1 (2026-10-16)

- Publishing names the five supported networks and says Facebook, YouTube and others are not supported, never "connect it" (the plugin eval's off-script case caught the agent offering to connect Facebook).

## 1.3.0 (2026-10-16)

- References: Viuz now builds a carousel or story from a public post link (`source_url`, `source_closeness`); the old "cannot build from a reference link" line is gone. Reference videos still are not a source.
- Plan: `viuz_generate_post` takes `plan_item_id` on every format, and a post made outside the plan is put on its week by default (`add_to_plan`, `objective`, undo receipt).

## 1.2.0 (2026-10-16)

- `viuz_get_slide` and `viuz_edit_slide` are named ("if available": both sit behind a flag that is off in some deployments); `plan_item_id` on every format, `add_to_plan` / `add_to_plan: false`, `source_url` with `source_closeness` on carousels and stories, and `confirm_spend` on `viuz_generate_post` are described the same way. Instagram's feed range is now 3:4 to 1.91:1 (3:4 publishes natively). A post changed through slide edits counts as mixed art in insights.

## 1.1.1 (2026-10-16)

- `make-post`: Instagram carousels run 3:4 to 1.91:1 (a 1080x1440 carousel was measured publishing uncropped on 2026-10-14); a single image or a video slide stays at 4:5.

## 1.1.0 (2026-10-16)

- New reference `covers`: when a cover helps, the cost and the ask, the modes (scene, layered, mockup), the member's photo or a References post's look as a reference, polling with the same key, setting the cover, and the refusals. `viuz_save_cover_method` is not named (its flag is still off).

## 1.0.1 (2026-10-16)

- Every spend tool accepts `confirm_spend`; the skill says to send it only after the member agreed, and how to answer `confirmation_required`.

## 1.0.0 (2026-10-16)

- First release: the `viuz` plugin with the Viuz connector and the `viuz-copilot` skill (English, with a Brazilian Portuguese copy).
- Skill branches for a post: Viuz makes the art, the agent makes the art, or another tool's images are uploaded; library first; references; the plan and the objective; what spends; publishing rules; Reels as scripts only.

Bump `docs/mcp/skill/VERSION` and add a section here whenever the skill text changes.
