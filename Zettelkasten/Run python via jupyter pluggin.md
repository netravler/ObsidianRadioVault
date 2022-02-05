```jupyter
import sys

print(sys.version)
print(sys.version_info)
print("hi")

```


https://github.com/tillahoffmann/obsidian-jupyter

```jupyter

import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])
fig.write_html('first_figure.html', auto_open=True)

```