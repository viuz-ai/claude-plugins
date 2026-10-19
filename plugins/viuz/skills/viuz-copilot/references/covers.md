# Covers: AI cover options for a post

Read this when the member wants a stronger first slide, or asks for a cover. Covers are optional. A post with a good photo does not need one.

## Cost first, always

Each option is one of the plan's monthly AI images. Say so in the member's terms ("3 options use 3 of your AI images this month"), recommend 1 or 2, and get a yes before the call. Send `confirm_spend: true` only after that yes. Options that fail are refunded.

## Pick the mode with the member

Ask one question, with your recommendation. Offer the modes this connection has: `mode` in `viuz_generate_cover`'s schema lists them.

- **scene** (default): a photo-like scene with calm room where Viuz sets the post's headline.
- **layered**: a person or object cut out in front of one giant word of the headline. Needs `post_id`, and only for feed posts (4:5 or 3:4).
- **mockup**: the post's own first-slide line printed on a real object in the scene (a poster, a sign). Needs `post_id`.
- **still_life** (if available: it sits behind a flag that is off in some deployments): one everyday object from the brand's field, photographed on the brand's colour, with one giant word Viuz writes for the post's idea (a new word, not taken from the headline), a short line and a handwritten aside. Needs `post_id`. The 3 options differ in framing (the third shows a hand). Recommend it for a tip, myth or mistake post when the member has no good photo. It is one choice among the four, never the default.

Optional starting points:

- **Their own photo as reference** (`reference_asset_ids`, up to 3): the same person or product in a new scene. Only library photos (`viuz_search_library`) or files uploaded into this post.
- **A References post's style** (`reference_post_id`): Viuz borrows only the look (light, composition, palette). Never the person, never its words. Never name the creator in the cover or the caption.

## The flow

1. `viuz_generate_cover` with `post_id` (or `subject`), `mode`, `variations` 1 to 3, an `art_direction` line if the member gave one, and one `idempotency_key`.
2. It may answer `running`. Call again with the SAME key and the SAME arguments until it is ready (about a minute). That never spends twice. A new key means new options and new cost.
3. Show the options and let the member choose.
4. `viuz_set_cover` with `post_id`, the chosen `cover_id`, `expected_updated_at` from `viuz_get_post`, and an `idempotency_key`. It costs nothing. A library photo's `asset_id` also works as a cover.
5. Nothing is published. Offer the next step (captions, schedule).

## Refusals, and what to say

- `no_photo_slot`: slide 1 has no photo slot to put the cover in. Explain that setting it will replace slide 1's layout with a full-photo cover that keeps its headline. Only after a yes, retry with `replace_layout: true`.
- `no_headline` or `cover_text_unusable`: the post's first slide has no line that fits a cover (or it is a placeholder). Ask the member for the line, or fix the slide first.
- `layered_needs_feed_size`: layered works on 4:5 or 3:4 posts only. Offer scene or mockup.
- `layered_unavailable`: the cutout step is down. Offer scene, do not retry in a loop.
- `plan_limit_exceeded`: no AI images left this month. Say so; offer a library photo as the cover instead. No retry.
- `idempotency_conflict`: that key was used for different arguments. Use a new key only if the member wants something new.

## Rules

- Never promise exact words painted by the image model. The only words promised are the mockup's line, which Viuz prints from the post. Other headline text is the post's own; to change it the member edits the post in Viuz.
- A sticky note or poster is one option, not a rule. Do not put one on every cover.
- Never copy a creator's cover. A reference is a look, not a template.
- People and short scene words are allowed. Invented numbers, prices, charts and screenshots are always kept out.

Member: "Make the cover more striking."
You: "I can make 2 options (uses 2 of your AI images this month). A giant word behind a photo of you, or a plain scene with room for the headline? I would try the giant word if you have a good photo in the library."
