# Projection Mapper

A desktop tool for **projection mapping**: warp images and videos onto real-world surfaces (walls, boxes, set pieces) by dragging the four corners of each layer until it lines up with the physical surface, then project full screen.

## What it does

- **Multi-surface mapping**: add several images or videos; each becomes its own quad you can position independently. Select surfaces by clicking or with Prev/Next Surface.
- **Real-time perspective warp**: dragging a corner handle recomputes a homography (`cv2.getPerspectiveTransform`) and the warped frame re-renders live (16 ms repaint timer). Each layer is clipped to its quad, so overlapping surfaces stay clean.
- **Video playback**: video files loop automatically. Supports PNG, JPG, BMP, MP4, MOV, AVI and MKV.
- **Presets**: save and load quad layouts as JSON, so a calibrated setup can be restored.
- **Show-ready controls**: fullscreen output, hideable toolbar (`H`), mesh/handle overlay toggle, right-click or `Delete` to remove a layer.

## Tech stack

Python · PySide6 (Qt 6) · OpenCV · NumPy

## How it works

1. Each media file is wrapped in a `Projection`: a `VideoSource` plus a 4-point target quad.
2. A Qt timer triggers a repaint every 16 ms. For each projection, the current frame is warped from its source rectangle onto the target quad with `cv2.warpPerspective`, converted to a `QImage` and painted inside a clip path.
3. Handles and mesh are drawn last so they always stay on top for alignment.

## Getting started

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python src/main.py
```

Then **Add Media**, drag the white corner handles onto your surface, and use **Toggle Fullscreen** on the projector display.

## Project structure

```
src/
  main.py          main window, toolbar, preset save/load, shortcuts
  canvas.py        render loop, perspective warp, handle dragging, selection
  projections.py   Projection model (media + target quad)
  video_source.py  image/video loading with looping playback
  utils.py         OpenCV → QImage conversion, quad ordering
presets/           example saved layouts (JSON)
media/             put your own images/videos here (not tracked)
```

## License

MIT
