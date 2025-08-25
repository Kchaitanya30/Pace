# Relectance using Pace data
Awesome—here’s a clean, copy-paste **README.md** for your repo based on the notebook you uploaded.

---

# Satellite Imagery & Analysis with PACE OCI

This project demonstrates how to load, explore, and visualize **NASA PACE/OCI** Level-1C data in Python/Jupyter. You’ll open a NetCDF file, browse metadata, plot single-band images and RGB composites, inspect pixel spectra, project imagery on a map, and compute simple indices (e.g., **NDVI** for the land project).

> Notebook: `SatelliteImagery_PACE-OCI_Projects.ipynb`

---

## Features

* Open PACE/OCI NetCDF (`.nc`) files with **netCDF4**
* Inspect dataset groups/variables and wavelengths
* Plot per-band images and a 3-band **RGB composite**
* Extract and plot **spectra** from selected pixels
* **Map projection** with Cartopy (lat/lon)
* “Special Codes for Specific Projects”:

  * **Land**: compute **NDVI**
  * (Room for other projects—ocean, atmosphere, etc.)

---

## Quick Start

### 1) Get the data

Place a PACE/OCI Level-1C file in (or adjust path inside the notebook):

```
../data/PACE_OCI.20240918T171411.L1C.V3.5km.nc
nc
```

### 2) Environment setup (choose one)

#### Option A: pip (simple)

```bash
# Windows (Command Prompt or PowerShell)
py -m pip install --upgrade pip
pip install jupyterlab numpy matplotlib netCDF4 cartopy pyproj shapely
```

#### Option B: conda (recommended for geospatial libs)

```bash
conda create -n pacedata -c conda-forge python=3.12 \
  jupyterlab numpy matplotlib netcdf4 cartopy pyproj shapely -y
conda activate pacedata
```

> If you previously had TLS/SSL certificate issues on Windows (e.g., PostgreSQL set CA vars), clear overrides before installing:
>
> ```
> set REQUESTS_CA_BUNDLE=
> set CURL_CA_BUNDLE=
> set PIP_CERT=
> set SSL_CERT_FILE=
> ```

### 3) Launch Jupyter

```bash
# from your project folder
py -m jupyter lab
```

Open `SatelliteImagery_PACE-OCI_Projects.ipynb`.

---

## How the Notebook is Organized

1. **Imports / Setup**
   Uses: `numpy`, `matplotlib`, `netCDF4`, `cartopy` (+ `ccrs`), plus small utility functions:

   * `reflectance(rad, sza, f0)` — converts radiance to reflectance:

     $$
     \text{reflectance} = \frac{\pi \cdot \text{radiance}}{\cos(\text{sza}) \cdot F_0}
     $$
   * `color3_make(ch1, ch2, ch3)` — builds 3-band color composites.

2. **Loading an Image**
   Opens the NetCDF dataset and explores groups/variables, e.g.:

   * `/geolocation_data/longitude`, `/geolocation_data/latitude`
   * `/sensor_views_bands/intensity_wavelength`
   * radiance arrays and attributes

3. **Wavelengths & Radiances**
   Lists available wavelengths, selects bands, and sets display ranges.

4. **Plotting**

   * Single-band quicklooks
   * **RGB composite** (choose 3 bands → visualize)

5. **Pixel Spectra**
   Select pixel(s) by (x, y) and plot spectrum across wavelengths.

6. **Map Projection**
   Uses Cartopy with lat/lon to render georeferenced imagery (e.g., PlateCarree).

7. **Special Codes for Projects**

   * **Land Project**: **NDVI**:

     $$
     \mathrm{NDVI} = \frac{S_{b1} - S_{b2}}{S_{b1} + S_{b2}}
     $$

     (pick two bands appropriate for vegetation; values in $-1, 1$)

---

## Usage Tips

* **Pick bands** thoughtfully for RGB (e.g., a red-ish, green-ish, and blue-ish channel).
* If imagery looks blown out, **adjust radiance/reflectance scales** in the plotting cells.
* For **pixel spectra**, verify `(row, col)` order and tilt/indexing match your array shapes.
* For **map plots**, ensure your latitude/longitude arrays have the same spatial shape as your imagery.

---

## Troubleshooting

* **`ModuleNotFoundError` (netCDF4/cartopy)**
  Install into the **same kernel** you’re running:

  ```python
  import sys; print(sys.executable)
  # then:
  "!{sys.executable}" -m pip install netCDF4 cartopy pyproj shapely
  ```

  Or use the conda option above.

* **`jupyter: command not found`**
  Launch via Python to bypass PATH issues:

  ```bash
  py -m jupyter lab
  ```

* **Windows SSL/TLS certificate errors** (pip can’t download)
  Clear certificate overrides as shown in Quick Start.

* **Cartopy PROJ/GEOS errors on Windows**
  Prefer the **conda-forge** environment:

  ```bash
  conda create -n pacedata -c conda-forge python=3.12 cartopy pyproj shapely netcdf4 -y
  ```

---
📑 [View the full presentation (PDF)](/WildfilesinAmazon.pdf)

---

## Acknowledgments

* NASA **PACE/OCI** mission and data providers
* Open-source Python ecosystem: NumPy, Matplotlib, netCDF4, Cartopy

