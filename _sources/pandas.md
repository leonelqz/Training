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

#### Sea Surface Temperature Anomaly

```{code-cell}
:tags: [remove-input]
import hvplot.xarray
import xarray as xr
import ee, os
from xee import EarthEngineBackendArray
def _slice_collection_silence(self, image_slice: slice) -> ee.Image:
  self._ee_init_check()
  start, stop, stride = image_slice.indices(self.shape[0])
  if self.store.fast_time_slicing and self.store.image_ids:
    imgs = self.store.image_ids[start:stop:stride]
  else:
    if self.store.fast_time_slicing:
      logging.warning(
            "fast_time_slicing is enabled but ImageCollection images don't have"
            ' IDs. Reverting to default behavior.'
        )
      # TODO(alxr, mahrsee): Find a way to make this case more efficient.
    list_range = stop - start
    imgs = self.store.image_collection.toList(list_range, offset=start).slice(
          0, list_range, stride
      )
  col = ee.ImageCollection(imgs)
  def reduce_bands(x, acc):
    return ee.Image(acc).addBands(x, [self.variable_name])
  aggregate_images_as_bands = ee.Image(col.iterate(reduce_bands, ee.Image()))
  # Remove the first "constant" band from the reduction.
  target_image = aggregate_images_as_bands.select(
      aggregate_images_as_bands.bandNames().slice(1)
    )
  return target_image
EarthEngineBackendArray._slice_collection = _slice_collection_silence


service_account = "gee-project@ee-leoqguz.iam.gserviceaccount.com"
json = "GEE_secret.json"
with open(json, "w") as file:
  file.write(os.environ["GEE"])
credentials = ee.ServiceAccountCredentials(service_account, json)
ee.Initialize(credentials)
os.remove(json)

#ds = xr.open_dataset("ssta_100_dias.nc")
#ds.hvplot(groupby='time', x="lon", y="lat", cmap="bwr", clim=(-1000, 1000))

ds = xr.open_dataset("ee://NOAA/CDR/OISST/V2_1", engine="ee", chunks="auto")
ds = ds.anom[-1].load()
ds.hvplot(x="lon", y="lat", cmap="bwr", clim=(-1000, 1000))

#ds_100 = ds.anom[-100:].load()
#ds_100.hvplot(groupby='time', x="lon", y="lat", cmap="bwr", clim=(-1000, 1000))
```
