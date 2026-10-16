# Changelog

## 1.3.0 (2026-10-16)

- References: Viuz now builds a carousel or story from a public post link (`source_url`, `source_closeness`); the old "cannot build from a reference link" line is gone. Reference videos still are not a source.
- Plan: `viuz_generate_post` takes `plan_item_id` on every format, and a post made outside the plan is put on its week by default (`add_to_plan`, `objective`, undo receipt).

## 1.2.0 (2026-10-16)

- `viuz_get_slide` and `viuz_edit_slide` are named ("if available": both sit behind a flag that is off in some deployments); `plan_item_id` on every format, `add_to_plan` / `add_to_plan: false`, `source_url` with `source_closeness` on carousels and stories, and `confirm_spend` on `viuz_generate_post` are described the same way. Instagram's feed range is now 3:4 to 1.91:1 (3:4 publishes natively). A post changed through slide edits counts as mixed art in insights.

## 1.1.0 (2026-10-16)

- New reference `covers`: when a cover helps, the cost and the ask, the modes (scene, layered, mockup), the member's photo or a References post's look as a reference, polling with the same key, setting the cover, and the refusals. `viuz_save_cover_method` is not named (its flag is still off).

## 1.0.1 (2026-10-16)

- Every spend tool accepts `confirm_spend`; the skill says to send it only after the member agreed, and how to answer `confirmation_required`.

## 1.0.0 (2026-10-16)

- First release: the `viuz` plugin with the Viuz connector and the `viuz-copilot` skill (English, with a Brazilian Portuguese copy).
- Skill branches for a post: Viuz makes the art, the agent makes the art, or another tool's images are uploaded; library first; references; the plan and the objective; what spends; publishing rules; Reels as scripts only.

Bump `docs/mcp/skill/VERSION` and add a section here whenever the skill text changes.
