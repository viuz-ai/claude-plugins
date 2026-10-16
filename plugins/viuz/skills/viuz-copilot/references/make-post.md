# Make a post: carousel, single image, story, text

Read this when the member wants a post. The first job is to find out whose art it is. Never choose for them.

## Before any branch

1. `viuz_get_context` (once per task) for the brand, goal, accounts and what is left on the plan.
2. If the post belongs to the plan, `viuz_get_plan` or `viuz_get_next_to_make` for the item: its title, hook line, brief and outcome lane. Use them.
3. Look in the library first: `viuz_search_library` for photos and videos. Say in one line what you found.
4. Ask the one thing you cannot guess: the topic, if the member did not say.

## Branch A: Viuz makes the art

Spends one generation from the plan per post. Say so and get a yes.

1. `viuz_get_brief` with `art: "viuz"`, the `format`, the member's request and an `idempotency_key`. It writes the angle, hook, per-slide outline, captions per platform, hashtags, call to action and a suggested time. It costs no generation. Without `art` it only returns the art question (`choice_required`) and does nothing else.
2. Show the member the angle and hook in two lines. If they want changes, change the request, not the post.
3. `viuz_generate_post` with the `brief_id` (or a plain `request` and a `format`), one `idempotency_key`, and the photos the member chose. The result is GENERATING.
4. Poll `viuz_get_post` with the `post_id` until RENDERED or FAILED. Do not call `viuz_generate_post` again while it renders. If it FAILED, say so and ask before generating again.
5. Show the result link. Offer the next step: edit captions (`viuz_update_draft`), adjust slides (`viuz_edit_slides`) or prepare the schedule (see Publishing in SKILL.md).

Notes:

- Photo pins fill the first image slots in page order, and a single image cannot take pins. Say that when the member expects a photo on a particular slide; to put a photo on a particular slide, upload or choose it and then use `viuz_edit_slides` (see `references/outside-images.md`).
- Pass `campaign_id` only when the member named a campaign. If it answers that campaign support is unavailable, nothing was generated; tell them and ask.
- A layered look (the subject photo in front of or behind the headline) is available only if the tool lists a `style` field. Pass it only when the member asked for that look.
- `viuz_generate_image` makes one AI image into the library with no post around it. It spends one AI image, has no people and no text in it, and gives you an `asset_id` to use in a draft or a generation.

## Branch B: You make the art

No generation is spent.

1. `viuz_get_brief` with `art: "yours"`, so Viuz writes only the strategy and a production block (canvas size, page count, words per page, caption targets).
2. `viuz_get_design_kit` for palette, fonts, logo, handle and canvas sizes.
3. Make the images at those sizes. Instagram carousels run 4:5 to 1.91:1 and every page should share one ratio.
4. Upload and build the draft. See `references/outside-images.md`.

## Branch C: Another tool's images

The member already has images. Skip the brief unless they want captions written. Upload and build the draft. See `references/outside-images.md`.

## Stories

9:16 images, 1080x1920, one frame per image, Instagram only, no caption. Use `viuz_create_draft` with format story. Never make a story as a feed post. Stickers are added in the Instagram app after publishing.

## Text posts

For X, LinkedIn and Threads only. Either `viuz_generate_post` with format text (one generation, ask first), or write the words yourself and save them with `viuz_create_draft` format text with `caption_x` (280 weighted characters), `caption_threads` (500) and `caption_linkedin` (3000). An over-limit text is refused naming the network. To rewrite one network's text shorter or more direct, `viuz_suggest_caption` with its rewrite option, then apply with `viuz_update_draft`.

## After the post exists

- Captions and labels: `viuz_update_draft`.
- Slides: `viuz_edit_slides`.
- To publish, follow the Publishing section of SKILL.md. The default is `viuz_prepare_schedule`.
- To remove a wrong draft: `viuz_delete_draft` after the member confirms.

## A good turn

Member: "A carousel about why we raised prices, use the studio photos."
You: "Two quick things. Should Viuz design it (one generation) or do you want me to write it and make the art? And I found 14 studio photos in your library; should I use all of them or the best five?"
