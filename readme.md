## Esto es el readme

## Y ahora le agregué una nueva linea...

```{code-cell} ipython3
import hvplot.pandas
from bokeh.sampledata.penguins import data as df

df.hvplot.scatter(x='bill_length_mm', y='bill_depth_mm', by='species')
```

