# Make a Reel

Viuz writes Reel scripts. It never renders video, and you must never say or imply that it does. The member films the Reel and uploads their own video.

## Write the script

`viuz_generate_reel_script` writes a script: hook, beats, payoff, call to action, captions, hashtags and a shot list. It spends one of the member's monthly Reel scripts.

1. Ask the member first. Tell them it uses one monthly Reel script.
2. Give exactly one source: `plan_item_id` (from `viuz_get_plan` or `viuz_get_next_to_make`; the script is linked to that item), `topic` (the member's own words) or `brand_idea_id`.
3. Call with `confirm_spend: true` and an `idempotency_key`. Without `confirm_spend` it answers `choice_required` with the quota and spends nothing.
4. Never repeat the call to change the script. To change it, use the edit tools below.

Read one later with `viuz_get_reel_script`; list them with `viuz_list_reel_scripts`.

## The member's own script

If the member wrote the script, `viuz_save_reel_script` saves it as written. It spends no monthly script. `viuz_review_reel_script` gives feedback on a script; it writes nothing.

## Change a script

1. `viuz_preview_reel_script_change` with the instruction. It returns before and after for every changed field and a preview token. It changes nothing.
2. Show the member every before and after exactly as returned and ask them to confirm.
3. Only after a clear yes, `viuz_apply_reel_script_change` with the same script, proposed values and preview token.
4. `viuz_undo_reel_script_change` puts it back within 24 hours, only when the member asks.

## After the member films

1. `viuz_prepare_upload` for the video (MP4 or MOV, up to 200 MB and 180 seconds; the plan must include video uploads). Run the returned curl command verbatim.
2. `viuz_complete_upload`. A video is processed in the background; while it says processing, call `viuz_complete_upload` again with the same arguments. It is safe to repeat.
3. `viuz_create_draft` with the video and `reel_script_id` so the draft is linked to the script. One video alone becomes an Instagram Reel. Optionally pass `cover_frame_ms` (a moment in the video) or `cover_asset_id` (an image from the library), never both.
4. Scheduling follows the Publishing section of SKILL.md.

## What not to promise

No video generation, no editing of the member's video, no captions burned into the video, no music added through this connection. If the member asks, say Viuz writes the script and the shot list and they record it.

## A good turn

Member: "Make a Reel from Thursday's plan item."
You: "That will use one of your monthly Reel scripts. Want me to go ahead? You will film it yourself; Viuz writes the hook, the beats and the shot list."
