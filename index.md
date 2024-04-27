---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  name: python3
---


### Linea 1...

```{code-cell} ipython3
import hvplot.pandas
from bokeh.sampledata.penguins import data as df

df.hvplot.scatter(x='bill_length_mm', y='bill_depth_mm', by='species')
```

