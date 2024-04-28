---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

### Sea Surface Temperature Anomaly

```{code-cell}
:tags: [remove-input]
import hvplot.xarray
import xarray as xr
import ee
import warnings
warnings.filterwarnings("ignore")

#ruta_json = r"C:\Users\Leo\Dropbox\ee-leoqguz-4754b53f9bc6.json"
ruta_json = "ee-leoqguz-4754b53f9bc6.json"
service_account = "gee-project@ee-leoqguz.iam.gserviceaccount.com"
credentials = ee.ServiceAccountCredentials(service_account, ruta_json)
ee.Initialize(credentials)

#ds = xr.open_dataset("ssta_100_dias.nc")
#ds.hvplot(groupby='time', x="lon", y="lat", cmap="bwr", clim=(-1000, 1000))

ds = xr.open_dataset("ee://NOAA/CDR/OISST/V2_1", engine="ee", chunks="auto")
ds.anom[-1].hvplot(x="lon", y="lat", cmap="bwr", clim=(-1000, 1000))

#ds_100 = ds.anom[-100:].load()
#ds_100.hvplot(groupby='time', x="lon", y="lat", cmap="bwr", clim=(-1000, 1000))
```
