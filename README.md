# Split Poster Studio

A single-page poster generator. One photo fills both halves of the canvas: the subject is cut
out and floated on a flat colour above the split, and the same photo runs below it with the
subject's silhouette painted in that colour.

No build step, no server, no dependencies to install. Open `index.html` or push it to GitHub Pages.

**Live:** https://jcreatvz.github.io/split-poster-studio/

## Files

```
index.html                      the whole app
models/magic_touch.tflite       interactive segmenter — click any subject
models/selfie_segmenter.tflite  person segmenter — no click needed
```

## Getting the models

The app looks for its models in three places, in order, and uses the first that answers:

1. `./models/` — same origin, nothing external at runtime
2. `cdn.jsdelivr.net/gh/jcreatvz/split-poster-studio@main/models/` — the repo, via CDN
3. Google's public MediaPipe host

Options 1 and 2 both come from this repo, so download the two files once and commit them:

| Save as | Download from |
|---|---|
| `models/magic_touch.tflite` | `https://storage.googleapis.com/mediapipe-tasks/interactive_segmenter/ptm_512_hdt_ptm_woid.tflite` |
| `models/selfie_segmenter.tflite` | `https://storage.googleapis.com/mediapipe-models/image_segmenter/selfie_segmenter/float16/1/selfie_segmenter.tflite` |

Paste each URL into a browser tab, save the file, rename it, drop it in `models/`.

Skipping this still works — the app falls through to Google's host. Self-hosting just removes
a third-party runtime dependency and makes the app usable inside sandboxes that block that host.

## Deploying

Settings → Pages → Source: **Deploy from a branch** → `main` / `/ (root)` → Save.
Live in about a minute at the URL above.

## Using it

Drop in a photo, or paste one with Ctrl+V. Click your subject in the thumbnail to cut it out.

- **Drag the top half** to move the cutout; **drag the bottom half** to pan the photo
- **Scroll over a half** to resize what's in it
- The silhouette is locked to the photo, so it always sits where the subject really is
- If the cutout comes back inside-out, hit **Flip the mask**

Accent colours are pulled from the photo — three dominant, three of their opposites. The
colour-name line in the caption writes itself from whichever accent is active.

## Caption writer

Optional. Click **Key**, paste an Anthropic API key. It is kept in this browser's
`localStorage` and posted directly to `api.anthropic.com` — it never reaches the page host.
A key sitting in `localStorage` on a public page is only as private as the device, so treat it
as a personal-tool convenience rather than something to share around.

Everything else works with no key at all.

## Licences

App code: yours to license as you like.

Both segmentation models and the MediaPipe Tasks runtime are Apache-2.0, from Google.
If you commit the `.tflite` files you are redistributing them, so keep `models/NOTICE.md`
in place. See <https://ai.google.dev/edge/mediapipe/solutions/vision/interactive_segmenter>.
