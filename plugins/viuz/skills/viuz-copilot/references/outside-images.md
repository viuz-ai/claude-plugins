# Outside images: upload, then place

Use this when the images were made by you (from the design kit), by another tool, or by the member. The same upload path serves all three. Viuz checks every file (malware scan, moderation, size, format), strips its metadata and stores it.

## New draft from your own images

1. If you are making the art, call `viuz_get_design_kit` first: palette, fonts, logo, handle and canvas sizes. Do not print `@yourhandle`.
2. `viuz_prepare_upload` with the file list. It returns one short-lived `upload_url` per file and a ready curl command in each entry's `curl` field. Run that curl command verbatim, from the folder that holds the files. Do not retype the URL or headers: the signature covers the exact bytes and a hand-copied URL fails with 400.
3. `viuz_complete_upload` with the object keys. Each file returns an `asset_id` or its own error; one bad file does not stop the others. JPEG, PNG or WebP up to 25 MB; MP4 or MOV up to 200 MB and 180 seconds; up to 10 files.
4. `viuz_create_draft` with the asset ids in the order wanted, plus captions if you have them, plus `plan_item_id` if the post belongs to a plan item, plus `brief_id` if the images came from a Viuz brief. The draft is not scheduled and not published.

Formats: several files become a carousel, one video becomes a Reel, and `format: "story"` turns 9:16 images into story frames. Read the warnings in the result and tell the member about any image Instagram would crop.

## Photos already in the library

Skip the upload. `viuz_search_library` finds them, then pass `library_asset_ids` (1 to 10) to `viuz_create_draft`. A photo you saved with `viuz_save_stock_photo` works the same way, and so does an image from `viuz_generate_image`.

## Mix outside images into a Viuz-designed post

1. Generate the post with `viuz_generate_post` and wait for RENDERED (poll `viuz_get_post`).
2. Read the slide ids and `updated_at` from `viuz_get_post`.
3. `viuz_prepare_upload` with `post_id` of that post, then `viuz_complete_upload`. This uploads the files into the existing unpublished post.
4. `viuz_edit_slides` with the COMPLETE slide list after the edit, in order: `{slide_id}` keeps a slide, `{asset_id}` adds a new slide from the uploaded file, and any existing slide you leave out is removed. To replace one slide, put the new `asset_id` where that slide was and keep every other id unchanged. Pass `expected_updated_at`; if the member changed the post meanwhile the call is refused with `conflict`: read the post again and redo the edit.
5. Never start a new draft to change one slide.

Be honest about what a swap does: the new image replaces the whole slide, including its designed text and layout. Viuz's own designed slides re-render for a moment (status RENDERING) after an edit. Changing only the words or one photo inside a designed slide is done with `viuz_get_slide` then `viuz_edit_slide`, if both are in your list (see SKILL.md, Fixing mistakes); if they are not, you cannot do it through this connection, so tell the member to open the post in Viuz, or generate again with a better request after asking them.

`viuz_edit_slides` works on a post that has not published, including a scheduled one more than 30 minutes ahead. It is refused (`not_editable`) when the post is publishing, published, still rendering, scheduled within 30 minutes, or scheduled for only some of its slides.

## Rules

- Do not upload the same file twice to fix a slow answer. `viuz_complete_upload` is safe to repeat with the same arguments.
- A video must be ready (processing says ready) before it goes into a draft or a slide edit.
- Images from another tool carry that tool's rules and licences. Do not claim the image is licensed or original on the member's behalf.

## A good turn

Member: "Replace the cover with this picture and keep the rest."
You: read the post, upload the picture into it, send the full slide list with the new asset in slide 1's place, then say: "Done. The old cover and its text are gone; the other slides are untouched. It is re-rendering for a moment."
