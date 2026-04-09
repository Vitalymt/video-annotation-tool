# Video Annotation Tool

A self-contained, browser-based tool for frame-accurate video segment annotation. Open `index.html` in any modern browser — no server, no build step, no dependencies to install.

Designed for workflows where an operator watches a video, marks problem segments, assigns labels, and exports structured metadata for downstream processing.

---

## Features

- Load video from a direct URL (S3 presigned links, CDN, etc.) or a local file
- Frame-accurate playback control: step one frame at a time in either direction
- Keyboard-driven segment marking: set start and end points without leaving the keyboard
- Segment overlay rendered as color bands on the timeline
- Label each segment with a predefined tag
- Export all annotations as structured JSON
- Fully self-contained: one HTML file, no external runtime dependencies

---

## Usage

Double-click `index.html` to open it in your default browser, or drag it into Chrome, Edge, or Firefox.

### Loading a video

The URL tab accepts any publicly accessible or presigned MP4/WebM link. Click "Загрузить" to load it. The second tab lets you pick a local file directly from disk.

### Annotating segments

1. Play the video and navigate to the start of a segment.
2. Press `[` to mark the start frame.
3. Navigate to the end of the segment.
4. Press `]` to mark the end frame.
5. Select a tag from the dropdown.
6. Click "Добавить фрагмент" to save the segment.

Each segment appears in the list below the player. Click the arrow button to jump to a segment's start frame, or the X button to remove it.

### Keyboard shortcuts

| Key       | Action             |
|-----------|--------------------|
| `Space`   | Play / Pause       |
| `[`       | Set segment start  |
| `]`       | Set segment end    |
| `←`       | Back 1 frame       |
| `→`       | Forward 1 frame    |

### Exporting

Click "Экспорт JSON" to download `manifest.json` containing all annotated segments:

```json
[
  {
    "start_frame": 840,
    "end_frame": 1250,
    "tag": "wrong_detection",
    "start_time_sec": 33.6,
    "end_time_sec": 50.0
  }
]
```

---

## Customization

**Tags** — edit the `<select id="tag-select">` options inside `index.html`. The `value` attribute is written to the JSON output; the visible label is shown in the UI.

**Frame rate** — change the `const FPS = 25` constant at the top of the script block. Frame numbers in the export are calculated from this value.

**Accent color** — edit `--accent` and `--accent-hover` in the `:root { ... }` CSS block at the top of the file.

---

## Technical notes

- Single HTML file. All logic is vanilla JavaScript; no frameworks or build tools.
- Video playback uses the browser's native `<video>` element. H.264/MP4 works across all major browsers. AV1 and WebM support varies by browser.
- Frame stepping: `video.currentTime = frame / FPS`. Accurate for constant frame rate sources. For variable frame rate sources, frame numbers are approximations.
- Segment bands on the timeline are absolutely positioned `<div>` elements redrawn on every add/remove operation.
- JSON export uses `URL.createObjectURL` and a temporary `<a>` element to trigger the download — no server required.

---

## License

MIT
