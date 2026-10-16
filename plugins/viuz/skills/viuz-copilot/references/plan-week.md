# The Weekly Plan

The plan is the member's strategy turned into weeks of items. Each item has a title, often a suggested hook line and a short brief, a day or time, channels, an outcome lane (reach, followers or sales) and a progress state (planned, made, scheduled, published).

## Read it

- `viuz_get_plan`: strategy, the weeks, and each item. Quote an item's reason, brief and hook line as given. A hook line is a suggestion the member may rewrite. Never add numbers that are not there. A proposed item or a draft week has not been reviewed: call it proposed.
- `viuz_get_next_to_make`: what to make next. Offer this when the member says "what should I post?".
- `viuz_get_brand_strategy` for pillars and goals; `viuz_get_best_times` for the best times to post; `viuz_get_insights` and `viuz_get_plan_review` for measured results, when the member asks how things went.

An empty plan is not an error. Say so and offer to start a week.

## Make something for an item

- Carousel, single image or story: follow `references/make-post.md`. When you save the draft with `viuz_create_draft`, pass the item's `plan_item_id` so the plan shows it as made.
- Text post: `viuz_generate_post` with format text accepts `plan_item_id`.
- Reel: `viuz_generate_reel_script` with `plan_item_id`. See `references/make-reel.md`.
- `viuz_generate_post` does not link a plan item for carousels, singles or stories. Do not say it did.

## Change the plan

Plan changes go in three steps, so the member sees them first:

1. `viuz_preview_plan_change` with a command. It computes the exact result and changes nothing. Commands include add_item, remove_item, move_item, approve_item, replace_item, set_item_channels, set_capacity, set_priorities, pause_lane, pillar and fact changes, and create_next_cycle.
2. Show the member what will change, in plain words.
3. After a clear yes, `viuz_apply_plan_change` with the signed preview, the expected revision and an `idempotency_key`. A stale revision is refused; read the plan again.

`viuz_undo_plan_change` reverses a plan change within its window, only when the member asks. `viuz_get_plan_receipt` shows what an applied change did.

To empty a week, apply a batch of remove_item commands: that can be undone.

## Weeks

- `viuz_create_plan_week` starts an empty week when there is none to plan in (a first week, or after a gap). A first week Viuz suggested at onboarding must be reviewed by the member in Viuz first; relay the link you are given.
- `viuz_delete_plan_week` deletes a whole week and its items. It cannot be undone and is refused if anything in the week was made, scheduled or published. Confirm with the member first, and set `confirm` true only after they agreed to delete that week.
- `create_next_cycle` copies a week forward. Give every new item a day inside the new week; never copy last week's dates.

## An objective for a post outside the plan

Every plan item has an outcome lane. A post made outside the plan has none stored today. Ask the member what it should do (reach, followers or sales) when it matters for the hook and the call to action, and write for that. If they want it on the plan, add it with preview then apply, then link the draft with `plan_item_id`.

## Ideas and inspiration

`viuz_list_ideas` reads the idea bank; `viuz_save_idea` saves one idea the member gave you; `viuz_delete_idea` dismisses one after they confirm. `viuz_list_campaigns` lists campaigns when campaign support is on.

## A good turn

Member: "What should I post this week?"
You: read the plan, then answer in three lines: the next item to make, why it is there (its own reason), and one question: "Want me to start it with Viuz designing the art, or do you want to make the images?"
