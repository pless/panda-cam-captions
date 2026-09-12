# Panda Cam Captions

Historical frames from the Smithsonian's National Zoo giant-panda cameras, each paired with a caption written by zoo staff. 682 pairs, 2009–2026, of which 165 are flagged as fixed-camera frames (the rest are keeper photographs and other zoo media that share the same captions and are kept for comparison).

**Browse it:** https://pless.github.io/panda-cam-captions/ (filter by source, year, cam flag; search captions).

**Download everything:** the [latest release](../../releases/latest) has a single zip of `images/`, `pairs.jsonl`, `pairs.csv` and this README. Or clone the repo (about 210 MB).

## Files

- `pairs.jsonl` — one JSON object per pair (the canonical file).
- `pairs.csv` — the same, flattened, for spreadsheets and pandas.
- `images/` — the frames. YouTube-derived frames are 1280×720 video thumbnails; news-post images are the zoo's uploads at their original size.
- `docs/` — the static browser (GitHub Pages).

## Fields

| field | meaning |
|---|---|
| `image` | path under `images/` |
| `caption` | the human-written text |
| `caption_kind` | `title` (YouTube video title), `alt_text` (image alt text in a zoo news post), `figcaption` (dated caption on an embedded cam clip) |
| `source` | `nzp_youtube`, `nzp_posts`, `nzp_news` |
| `description` | YouTube video description, when present: a longer keeper-written paragraph for the same clip |
| `date` | upload date, post date, or the date in the figcaption |
| `url` | where the pair came from (video page or news post) |
| `video_id`, `duration` | YouTube id and length in seconds, for YouTube pairs |
| `cam_score` | SigLIP2 zero-shot margin, "fixed webcam frame" prompts minus "professional photo / interview / title card" prompts |
| `is_cam` | `cam_score ≥ 0.028` and no title or credit rule fired. **A heuristic, not a hand label.** Contact-sheet inspection put the boundary near 0.028; expect a handful of errors either side. |

## Where it came from

- The zoo's YouTube channel (259 giant-panda videos out of 1,051): title, description, upload date, and the thumbnail frame.
- Zoo news posts (382 panda posts, 2015–2026): each post's own images with their alt text.
- Zoo news figure captions on embedded cam clips (rare: 8).

The 2013–2015 "Giant Panda Update" posts survive on the zoo site as text only, and the 2020–2021 weekly cub updates have been removed, so those eras are represented by YouTube clips rather than post images.

## Load it

```python
import json, pandas as pd
pairs = [json.loads(l) for l in open("pairs.jsonl")]
df = pd.read_csv("pairs.csv")
cam = df[df.is_cam]           # 165 fixed-camera frames
```

## Terms and attribution

All images and captions are © Smithsonian's National Zoo and Conservation Biology Institute and are reproduced here for research and educational use under the Smithsonian's [Terms of Use](https://www.si.edu/termsofuse), which permit personal, educational and other non-commercial use with attribution. They are **not public domain** and **not licensed for commercial use**; commercial use requires the zoo's permission. Please credit "Smithsonian's National Zoo and Conservation Biology Institute" and link to the source page in `url` when you reuse a frame. The Smithsonian watermark on frames must not be removed.

The metadata, scripts and this compilation are released under CC BY 4.0.

## Citation

If you use this in a paper, please cite this repository and credit the Smithsonian's National Zoo. Built as part of work on specialist text-to-image retrieval (Robbins, Stylianou, Pless).
