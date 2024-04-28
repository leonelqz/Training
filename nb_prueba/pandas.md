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

### Pruebo el code-cell

```{code-cell}
import pandas as pd
df = pd.DataFrame( {"A": range(100), "B": [k**2 for k in range(100)] } )
df
```

```{code-cell}
df.plot("A", "B", title="Plot de ejemplo")
```




##### Con ipython3
```{code-cell} ipython3
note = "Python syntax highlighting"
print(note)
```

##### Sin ipython3
```{code-cell}
note = "Python syntax highlighting"
print(note)
```