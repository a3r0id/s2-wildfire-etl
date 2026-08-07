# Wildfire Footprinting With Sentinel-2

Detect active-fire hotspots from [Sentinel-2 L2A](https://docs.sentinel-hub.com/api/latest/data/sentinel-2-l2a/) imagery and export map-ready features or integrate detection workflows.

Hotspot logic is adapted from **QuickFire V1.0.0** by [Pierre Markuse](https://x.com/Pierre_Markuse) ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)), originally published as an evalscript in the [Copernicus Browser](https://dataspace.copernicus.eu/browser/).

*2026 Old Trails Fire - Spokane, Washington, United States - QuickFire - *
<img width="1000" height="387" alt="image" src="https://github.com/user-attachments/assets/49036045-7eb8-4af3-953a-12648d936bc0" />

*2026 Old Trails Fire - Spokane, Washington, United States - Detections - *
<img width="2018" height="833" alt="image" src="https://github.com/user-attachments/assets/a278dd77-c196-4d9a-ae1c-9ca503788e08" />

<img width="1155" height="515" alt="image" src="https://github.com/user-attachments/assets/8e35c798-55ff-41a8-834c-9efb3269e0fc" />


## How it works

1. Search and load Sentinel-2 L2A COGs from [Element 84](https://element84.com/)’s public [STAC](https://stacspec.org/) API (`earth-search`)
2. Scale reflectance and pick the clearest solar-day scene
3. Run a vectorized QuickFire port (`odc-stac` / `xarray`) for RGB visualization and hotspot levels `0–4`
4. Polygonize hotspots and write outputs under `outputs/`

The demo AOI in [`s2-wildfire.ipynb`](s2-wildfire.ipynb) is the Old Trails Fire near Spokane, WA (`2026-08-01`).

## Limitations

This needs near-ideal conditions. Results degrade with:

- Cloud or smoke cover
- Bright or optically thick smoke nearby

It tends to work best on wind-driven fires with high BTUs and little/offset smoke.

## Setup

```bash
python -m pip install -r requirements.txt
cp example.env .env   # Windows: copy example.env .env
```

Edit `.env` (`BBOX`, `DATETIME`, optional `OUT_DIR`), then run [`s2-wildfire.ipynb`](s2-wildfire.ipynb) top to bottom.

## Outputs

| Path | Description |
|------|-------------|
| `outputs/shapefile/` | Hotspot polygons (`.shp`) |
| `outputs/geojson/` | Same polygons as GeoJSON (WGS84) |
| `outputs/scl/` | Scene classification layer GeoTIFF |
| `outputs/hotspot_mask/` | Hotspot level mask GeoTIFF (`0–4`) |

## License

This project’s code is licensed under the [MIT License](LICENSE).

The QuickFire algorithm concept is © Pierre Markuse and used under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribution must be retained when redistributing adaptations of that material.
