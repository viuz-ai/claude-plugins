# Changelog

## 1.5.0 (2026-11-11)

- A plain `viuz_generate_post` call for a carousel (no `html_mode`) is made in the new designs while `HTML_DOOR_MCP_DEFAULT_ENABLED` is on (W5-c): typography only, one generation, no AI image, answer at once in GENERATING. `plan_item_id` links the post to the plan item (the answer says `plan_item_linked`), `source_url` is read like the repurpose door, `brief_id` brings the brief's words, `aspect` is `3:4` (default), `4:5` or `1:1`. Refused before anything is spent, with a sentence: `style`, `cta_kind`, `mid_cta`, `template_slug`, `source_closeness` and `aspect` 9:16 (`invalid_input`, reason `scene_only_key`). Photo pins keep a carousel in the previous editor; single images, stories and text posts are unchanged; `networks` only matters to a text post. Post-creation guidance no longer speaks of scene posts. Real-transport eval on Dev: `evals/mcp/reports/2026-11-11-w5c-generate-html-default-dev.md`.

## 1.4.3 (2026-11-10)

- Change the words of a page of a post made with `html_mode` (W3 M1), behind `MCP_HTML_PAGE_EDIT_ENABLED`: `viuz_get_page` reads the page slots, `viuz_edit_page` writes plain text with `base_revision`; `page_changed` = read again; wait for RENDERED; spends nothing. Real-transport eval on Prod 2026-11-10: edit + revert passed, refusals `page_changed`, `invalid_input` (`markup_in_value`), `not_html_post`, `not_found` all answered right.

## 1.4.2 (2026-10-27)

- Changing the design of a finished post (P3 gate 2, HP-R6): the swap is a NEW post with another `html_template_key`, one generation, after a yes; the keys are listed by `viuz_list_templates` under `html_designs` (behind `MCP_HTML_POSTS_ENABLED`). While a post is being made, wait the `retry_after_seconds` the create answer and `viuz_get_post` give instead of reading in a loop. Agent eval: `evals/mcp/reports/2026-10-27-html-posts-dev-gate-3.md`.

## 1.4.1 (2026-10-26)

- Make another like an OLD post (F7 A), behind `MCP_HTML_POSTS_ENABLED`: `viuz_generate_post` with `html_mode: "replicate"` accepts a `source_post_id` of a post made in the previous editor (scene or layered): it is remade in a new design from that post's text, photos do not come along, one generation. An Instagram import, a text post and a video are still refused (`source_not_replicable`). `viuz_get_post` adds `replicable`. When `viuz_edit_slide(s)` refuses an HTML post, the refusal names the replicate call. Agent eval not run yet.

## 1.4.0 (2026-10-26)

- The new designs over MCP (F5 P2), behind `MCP_HTML_POSTS_ENABLED`; the skill says "if listed" like the other flagged tools. `viuz_generate_post` takes `html_mode` (`template`, `hybrid`, `same_as_last`, `replicate` with `source_post_id`), `html_template_key` and `format`; it answers at once with GENERATING, never wait or retry after a timeout, read `viuz_get_post`. `viuz_get_post` shows `render_engine` and `html.pages` (slide id, revision, `has_edits`). New `viuz_regenerate_page` redoes one page with `expected_revision`; `page_has_edits` needs an explicit member yes before `discard_edits`. On a post with `render_engine` html, `viuz_edit_slide(s)` and the cover tools refuse. Real photos found for the subject may appear: never promise one. Agent eval not run yet (`evals/mcp/tasks/html-posts.md`).

## 1.3.8 (2026-10-23)

- Reading `viuz_get_insights` with `by: "formats"`: when `strongest_format.ruler` is `cell`, a format's `multiple` compares formats with each other on the posts that are 7 days or older in an account with at least 9 posts of that format; it is not a post's multiple, a format missing from the list gets no number, and two formats are never divided into a "times better" figure. `by: "posts"` counts the posts left out of the ranking by reason (`without_multiple`). Behind `INSIGHTS_DEVIATION_V2_ENABLED`; with the flag off the field is absent and the older reading stands.
- Reading `viuz_get_reel_script`: when `hook_type` is null, never name or suggest a hook type (Dev agent eval `evals/mcp/reports/2026-10-23-insights-deviation-real-transport-dev.md`).

## 1.3.7 (2026-10-22)

- Reading `viuz_get_insights`: the multiple is per format on the member's own account; quote it as returned, name the tier when present, and for a null multiple give the reason (`accumulating`, `below_floor`, `no_metric`) with no number. Behind `INSIGHTS_DEVIATION_V2_ENABLED`; with the flag off the tool returns no tier or reason and the older wording stands.
- Reading `viuz_get_reel_script`: `hook_tests` says what the opening tests and `hook_type_source` says who set the hook type (the model at creation, or the member). Never write or guess a hook type the tool did not return.

## 1.3.6 (2026-10-20)

- Renewing a deadline: when more than one saved note with a deadline could be the one the member means, name them and ask which before renewing; with exactly one clear match, renew it and say which.

## 1.3.5 (2026-10-20)

- Saving news with a deadline (Dev agent eval, `evals/mcp/reports/2026-10-20-save-brand-news-dev.md`): the offer to save goes at the end of the SAME message that delivers the task, and a missing date is asked in that same question; a member who extends or changes a deadline already saved gets `renew` by title (or `note_id`), never a second note. `renew` by title no longer needs the folder unless several notes share the title, and a `not_found` by title lists the brand's saved notes with a deadline (`candidates`: id, title, folder, date) so the agent can retry instead of saving a second note.

## 1.3.4 (2026-10-20)

- Directory links in `plugin.json`: repository, documentation (viuz.ai/mcp), support (support@viuz.ai) and terms. No skill change.

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
