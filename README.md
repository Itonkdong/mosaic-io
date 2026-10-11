# Mosaic.IO

A Python desktop app that turns pictures into photo mosaics by replacing image regions with small tile images matched by average color. It includes a Tkinter interface, basic color-distance matching, and faster matching with SciPy's KDTree, plus optional random sampling and parallel processing.

## Setup

Use **Python 3.12 or newer** with Tkinter. Run the commands below from the project root.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install numpy opencv-python scipy
New-Item -ItemType Directory -Force -Path tiles, databases, dest, logs | Out-Null
```

Put readable tile images directly in `tiles/` (no subfolders). The processor resizes them and caches tile data and average colors in `databases/`. Clear the cache when changing the tile collection, or construct `MosaicProcessor` with `override_database=True` to rebuild it.

## Run

```powershell
python MosaicIO.py
```

Select a picture, choose a tile size and border option, and enter a title. Click **Create Mosaic** to save a JPEG to `dest/<title>.jpg`, then **Show Mosaic** to preview it. The destination folder is created automatically if needed, and save failures are shown in the app. Smaller tiles preserve more detail but take longer to process. **Fill borders** expands the result to fit complete tiles; **Keep same size** preserves dimensions and leaves incomplete edge regions black.

To create and save a mosaic from Python, run this example from a script in the project root:

```python
from ImageProcessor import ImageProcessor
from MosaicProcessor import MosaicProcessor

processor = MosaicProcessor(ImageProcessor())
processor.create_mosaic(
    "test_images/single/bear.jpg",  # Replace with your picture's path.
    tile_size=(16, 16),  # (height, width) in pixels
    improved=True,
    write=True,
    title="my-mosaic",
)
```

The result is saved to `dest/my-mosaic.jpg`. The first run for a tile size builds its cache and may take longer.

## Project files

- `MosaicIO.py`: desktop interface.
- `MosaicProcessor.py`: mosaic creation and experiment helpers; running it directly executes the configured `normal_run()` example.
- `ImageProcessor.py`: image display, color calculations, and cache utilities.
- `tiles/`, `tiles1/`, `tiles2/`: tile collections; the app uses `tiles/` by default.
- `test_images/`: sample input pictures.
- `databases/`, `dest/`, `logs/`, `test_results/`, `experiments/`: caches, generated mosaics, and experiment output.
- `MosaicProcessor-1.py` and `backup/`: earlier implementations.

Generated output, Python caches, virtual environments, and editor settings are ignored by Git. Ignore rules do not remove files already tracked in the repository.

Compatible with Windows only.
