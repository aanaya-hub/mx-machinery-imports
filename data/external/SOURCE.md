# External data — provenance

These files are **not** part of the synthetic import dataset. They are public reference
geography, bundled so the figures in the notebook can be rebuilt offline and identically
on any machine.

## `ne_110m_land.geojson`

| | |
| --- | --- |
| **What** | World land polygons at 1:110 million scale — 127 features |
| **Source** | [Natural Earth](https://www.naturalearthdata.com/) vector data, via the [natural-earth-vector](https://github.com/nvkelso/natural-earth-vector) repository |
| **File** | `geojson/ne_110m_land.geojson`, downloaded 2026-09-24 |
| **Licence** | **Public domain.** Natural Earth states: *"No permission is needed to use Natural Earth. Crediting the authors is unnecessary."* |
| **Attribution given anyway** | "Coastlines: Natural Earth" on every figure that uses it, and in the project README |
| **Size** | 138 KB |

### Why it is committed rather than downloaded at run time

Two reasons, both learned the hard way in this notebook:

1. **A figure that needs the network is not reproducible.** The earlier attempt at a map used
   `contextily` to fetch map tiles, which fails offline — and when it fails, it still produces
   a figure, so the failure looks like a design choice rather than a missing layer.
2. **Map tiles cannot express this route.** The China→Mexico lanes cross the Pacific, and
   standard web-map tiles stop at the dateline. Plotting the route on tiles is not possible
   without a Pacific-centred projection, which the tile servers do not serve.

A 138 KB public-domain file removes both problems permanently.

### How it is used

`data/external/ne_110m_land.geojson` is read by the geography cell in
`notebooks/01-data-repair-and-eda.ipynb`. Polygon longitudes are shifted into a
Pacific-centred window (negative longitudes become `lon + 360`) so the whole route fits on one
map, and features that straddle the dateline are dropped rather than drawn as a band across the
whole figure.
