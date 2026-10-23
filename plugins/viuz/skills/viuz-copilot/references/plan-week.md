# The Weekly Plan

The plan is the member's strategy turned into weeks of items. Each item has a title, often a suggested hook line and a short brief, a day or time, channels, an outcome lane (reach, followers or sales) and a progress state (planned, made, scheduled, published).

## Read it

- `viuz_get_plan`: strategy, the weeks, and each item. Quote an item's reason, brief and hook line as given. A hook line is a suggestion the member may rewrite. Never add numbers that are not there. A proposed item or a draft week has not been reviewed: call it proposed.
- `viuz_get_next_to_make`: what to make next. Offer this when the member says "what should I post?".
- `viuz_get_brand_strategy` for pillars and goals; `viuz_get_best_times` for the best times to post; `viuz_get_insights` and `viuz_get_plan_review` for measured results, when the member asks how things went.
- Reading `viuz_get_insights` with `by: "formats"`: when `strongest_format.ruler` is `cell`, each format's `multiple` is that format's median views over the median of the same qualifying posts (7 days or older, an account with at least 9 of that format), so it compares formats with each other; it is not a post's `multiple`, and a format missing from the list has too few mature posts, so say it needs more posts, never give it a number. Quote each format's `multiple` as returned and never divide one format's views or multiple by another's ("Reels do 1.7x more than carousels" is a number the tool did not return).
- Reading `viuz_get_insights` posts and formats: a post's `multiple` is views over the median of the member's OTHER posts of the same format on the same account, so say "twice the usual for a Reel", never "twice the usual" alone, and quote it exactly as returned (it is already cut to one decimal; never round it up or recompute it). When `tier` is present, "standout" and "notable" are the words; a "typical" post is not a win. When `multiple` is null, read `multiple_reason`: `accumulating` means the post is under 7 days old and still collecting views, so say it is too early; `below_floor` means the member has too few posts of that format to compare (`multiple_n` says how many it has), so say more posts of that format are needed; `no_metric` means the network reported no views. Give the reason and no number. Under `by: "posts"`, posts with no multiple are left out of `top` and `bottom`; `without_multiple` counts them by reason, so say how many are still too new or lack enough posts of their format. Never state a multiple, a tier or a "better than usual" the tool did not return, and never compare multiples across formats as if they shared a base.

An empty plan is not an error. Say so and offer to start a week.

## Make something for an item

- Carousel, single image or story: follow `references/make-post.md`. When you save the draft with `viuz_create_draft`, pass the item's `plan_item_id` so the plan shows it as made.
- `viuz_generate_post` accepts `plan_item_id` on every format (text, carousel, single image, story) and links the post to the item. It spends one generation: ask first.
- Reel: `viuz_generate_reel_script` with `plan_item_id`. See `references/make-reel.md`.
- A post made without `plan_item_id` is put on its week by default (`add_to_plan`; pass `objective` after asking the member). The answer says where and gives a receipt for `viuz_undo_plan_change`. `attach_post` and `detach_post` link or unlink a post the member already made. A post the member published straight from the Instagram app appears in `viuz_list_posts` and `viuz_get_post` with `origin: instagram_native` and its `permalink`: it is read-only (edit, schedule and publish tools refuse it with `native_post_read_only`), but `attach_post` works on it and its numbers can be read.

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

Every plan item has an outcome lane. A post made outside the plan has none until it is added to the plan (`add_to_plan` does that by default, with the `objective` you pass). Ask the member what it should do (reach, followers or sales) when it matters for the hook and the call to action, and write for that. If they want it on the plan, add it with preview then apply, then link the draft with `plan_item_id`.

## Ideas and inspiration

`viuz_list_ideas` reads the idea bank; `viuz_save_idea` saves one idea the member gave you; `viuz_delete_idea` dismisses one after they confirm. `viuz_list_campaigns` lists campaigns when campaign support is on.

## A good turn

Member: "What should I post this week?"
You: read the plan, then answer in three lines: the next item to make, why it is there (its own reason), and one question: "Want me to start it with Viuz designing the art, or do you want to make the images?"
