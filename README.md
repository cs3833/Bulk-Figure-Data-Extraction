# Batch Plot Digitizer

A browser tool for extracting numerical data from many plot images in a row, built for digitizing literature figures (for example cyclic voltammograms and polarization curves) in bulk.

**Use it here:** https://cs3833.github.io/Bulk-Figure-Data-Extraction/

It is a single HTML file. There is nothing to install and nothing is uploaded: images and results stay in your own browser.

## How it works

Every pixel that matches the curve's color inside the plot area becomes a data point, and pixel positions are converted to axis values with a linear calibration from two known ticks per axis. No data is thinned or interpolated.

Work through a folder of figures one at a time:

1. **Inner crop** – drag a box just inside the plot's axes. The plot frame is detected automatically and can be adjusted.
2. **Axes** – tick marks along the crop edges are detected and calibration points are placed on them. Type the value printed at each tick, or click another tick to move a point. Points snap to the tick's center. X and Y points can share the origin. An axis can use a **scale bar** instead of two ticks. Enter the x and y units here.
3. **Colors** – click the curve to pick its color (several shades allowed). Set the color tolerance and the minimum cluster size, so small specks of a matching color are ignored.
4. **Delete points** – drag boxes over stray points, on the figure or on a data plot.
5. **Review** – check the points over the figure, choose the dataset type (CV, Pol curve or Generic), and **Save & next**.

Datasets are saved under the image's file name, so `CV5-2.png` is saved as `CV5-2`.

### Edit image

For figures with overlapping curves, legends or labels, **Flag for editing** sends the figure to the image editor. Tools include crop, erase box, eraser, remove/keep/recolor a color, paint, and **trace**, which draws a smooth line through clicked points at a chosen width for curves that are hard to separate. Erased areas are left at 10% opacity by default. **Send to digitizer** replaces the figure with the edited image (the original file is never changed).

### Review data

The **Review data** tab shows each saved dataset over the original, unedited image (with any editor crop applied). The axis values and the recorded units are shown outside the image. You can adjust point color, point opacity and image opacity, edit the units, and mark each dataset **OK** or **Flag for edits**. Flagged datasets return to the Digitize queue as "needs review".

## Preparing figures

- Use images at their **original resolution**. Don't resize or screenshot them at a lower zoom.
- The dataset you want should be **a single color**. Use the image editor to isolate it if curves overlap.
- **Erase legends and labels** that share the curve's color.
- PNG, JPG, BMP, WebP and GIF are supported. TIFF is not (browsers can't open it), so convert those to PNG.
- Transparent backgrounds are treated as white.

## Output

**Export results (.zip)** produces:

| File | Contents |
|---|---|
| `data/<name>.csv` | One file per dataset: `X (units)`, `Y (units)` |
| `plots/<name>_plot.png` | A quick plot of each dataset |
| `extraction_summary.csv` | One row per dataset: original file, type, units, number of points, pixels per unit, line thickness and the matching x/y uncertainty (half the line width), calibration points, crop, colors, tolerance, delete boxes, review status |
| `auto_saved_data.csv` | All datasets side by side |
| `skipped_files.csv` | Images with no saved dataset |
| `session.json` | Settings for every figure, to reload a session later |
| `edited/` | Edited images |

Units are written in plain text (`u` for micro, `^2` for squared, for example `uA/cm^2`). Points are ordered along the curve by default, or by image rows. Values are rounded to 0.1 pixel by default, or kept at full precision.

## Storage and folders

- Work is saved in the browser automatically. A storage meter shows how much space is used, keeping room for the export.
- In **Chrome or Edge** (over https), **Open folder** writes results into an `extracted_<date>` folder next to your images after every figure. When browser storage fills up, everything so far is saved as a part and processing continues.
- In other browsers, use **Add images** and **Export**.
- Saved work belongs to one browser at one address. Export before switching browsers or computers, and use **Load session file** to continue.

## Running it yourself

Download `index.html` and open it in a browser, or host it with GitHub Pages (Settings → Pages → deploy from the `main` branch, root folder). The only external dependency is [JSZip](https://stuk.github.io/jszip/), loaded from cdnjs for building the zip export.

## License

See [LICENSE](LICENSE).
