# Getting Started with Plotly in Python

Getting Started with Plotly for Python.

  
New to Plotly?

Plotly is a [free and open-source](https://plotly.com/python/is-plotly-free/) graphing library for Python. We recommend you read our [Getting Started guide](https://plotly.com/python/getting-started/) for the latest installation or upgrade instructions, then move on to our [Plotly Fundamentals tutorials](https://plotly.com/python/plotly-fundamentals/) or dive straight in to some [Basic Charts tutorials](https://plotly.com/python/basic-charts/).

### Overview

The [`plotly` Python library](https://plotly.com/python/) is an interactive, [open-source](https://plotly.com/python/is-plotly-free) plotting library that supports over 40 unique chart types covering a wide range of statistical, financial, geographic, scientific, and 3-dimensional use-cases.

Built on top of the Plotly JavaScript library ([plotly.js](https://plotly.com/javascript/)), `plotly` enables Python users to create beautiful interactive web-based visualizations that can be displayed in Jupyter notebooks, saved to standalone HTML files, or served as part of pure Python-built web applications using Dash. The `plotly` Python library is sometimes referred to as "plotly.py" to differentiate it from the JavaScript library.

Thanks to deep integration with our [Kaleido](https://medium.com/plotly/introducing-kaleido-b03c4b7b1d81) image export utility, `plotly` also provides great support for non-web contexts including desktop editors (e.g. QtConsole, Spyder, PyCharm) and static document publishing (e.g. exporting notebooks to PDF with high-quality vector images).

This Getting Started guide explains how to install `plotly` and related optional pages. Once you've installed, you can use our documentation in three main ways:

1.  You jump right in to **examples** of how to make [basic charts](https://plotly.com/python/basic-charts/), [statistical charts](https://plotly.com/python/statistical-charts/), [scientific charts](https://plotly.com/python/scientific-charts/), [financial charts](https://plotly.com/python/financial-charts/), [maps](https://plotly.com/python/maps/), and [3-dimensional charts](https://plotly.com/python/3d-charts/).
2.  If you prefer to learn about the **fundamentals** of the library first, you can read about [the structure of figures](https://plotly.com/python/figure-structure/), [how to create and update figures](https://plotly.com/python/creating-and-updating-figures/), [how to display figures](https://plotly.com/python/renderers/), [how to theme figures with templates](https://plotly.com/python/templates/), [how to export figures to various formats](https://plotly.com/python/static-image-export/) and about [Plotly Express, the high-level API](https://plotly.com/python/plotly-express/) for doing all of the above.
3.  You can check out our exhaustive **reference** guides: the [Python API reference](https://plotly.com/python-api-reference) or the [Figure Reference](https://plotly.com/python/reference)

For information on using Python to build web applications containing plotly figures, see the [_Dash User Guide_](https://dash.plotly.com/).

We also encourage you to join the [Plotly Community Forum](http://community.plotly.com/) if you want help with anything related to `plotly`.

### Installation

`plotly` may be installed using `pip`:

$ pip install plotly==5.5.0

or `conda`:

$ conda install -c plotly plotly=5.5.0

This package contains everything you need to write figures to standalone HTML files.

> Note: **No internet connection, account, or payment is required to use plotly.py.** Prior to version 4, this library could operate in either an "online" or "offline" mode. The documentation tended to emphasize the online mode, where graphs get published to the Chart Studio web service. In version 4, all "online" functionality was removed from the `plotly` package and is now available as the separate, optional, `chart-studio` package (See below). **plotly.py version 4 is "offline" only, and does not include any functionality for uploading figures or data to cloud services.**

import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])
fig.write_html('first_figure.html', auto_open=True)

### Plotly charts in Dash

[Dash](https://plotly.com/dash/) is the best way to build analytical apps in Python using Plotly figures. To run the app below, run `pip install dash`, click "Download" to get the code and run `python app.py`.

Get started with [the official Dash docs](https://dash.plotly.com/installation) and **learn how to effortlessly [style](https://plotly.com/dash/design-kit/) & [deploy](https://plotly.com/dash/app-manager/) apps like this with [Dash Enterprise](https://plotly.com/dash/).**

#### JupyterLab Support

For use in [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/), install the `jupyterlab` and `ipywidgets` packages using `pip`:

$ pip install "jupyterlab>=3" "ipywidgets>=7.6"

or `conda`:

$ conda install "jupyterlab>=3" "ipywidgets>=7.6"

You'll need `jupyter-dash` to add widgets such as sliders, dropdowns, and buttons to Plotly charts in JupyterLab.

Install [`jupyter-dash`](https://github.com/plotly/jupyter-dash) using `pip`:

$ pip install jupyter-dash

or `conda`:

$ conda install -c conda-forge -c plotly jupyter-dash

These packages contain everything you need to run JupyterLab...

$ jupyter lab

and display plotly figures inline using the `plotly_mimetype` renderer...

import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])
fig.show()

abc00.511.522.53

xy

[](https://plotly.com/)

or using `FigureWidget` objects.

import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])

import plotly.graph_objects as go
fig_widget = go.FigureWidget(fig)
fig_widget

The instructions above apply to JupyterLab 3.x. **For JupyterLab 2 or earlier**, run the following commands to install the required JupyterLab extensions (note that this will require [`node`](https://nodejs.org/) to be installed):

# JupyterLab 2.x renderer support jupyter labextension install jupyterlab-plotly@5.5.0 @jupyter-widgets/jupyterlab-manager

Please check out our [Troubleshooting guide](https://plotly.com/python/troubleshooting/) if you run into any problems with JupyterLab, particularly if you are using multiple python environments inside Jupyter.

See [_Displaying Figures in Python_](https://plotly.com/python/renderers/) for more information on the renderers framework, and see [_Plotly FigureWidget Overview_](https://plotly.com/python/figurewidget/) for more information on using `FigureWidget`.

#### Jupyter Notebook Support

For use in the classic [Jupyter Notebook](https://jupyter.org/), install the `notebook` and `ipywidgets` packages using `pip`:

$ pip install "notebook>=5.3" "ipywidgets>=7.5"

or `conda`:

$ conda install "notebook>=5.3" "ipywidgets>=7.5"

These packages contain everything you need to run a Jupyter notebook...

$ jupyter notebook

and display plotly figures inline using the notebook renderer...

import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])
fig.show()

abc00.511.522.53

xy

[](https://plotly.com/)

or using `FigureWidget` objects.

import plotly.express as px
fig = px.bar(x=["a", "b", "c"], y=[1, 3, 2])

import plotly.graph_objects as go
fig_widget = go.FigureWidget(fig)
fig_widget

See [_Displaying Figures in Python_](https://plotly.com/python/renderers/) for more information on the renderers framework, and see [_Plotly FigureWidget Overview_](https://plotly.com/python/figurewidget/) for more information on using `FigureWidget`.

### Static Image Export

plotly.py supports [static image export](https://plotly.com/python/static-image-export/), using the either the [`kaleido`](https://github.com/plotly/Kaleido) package (recommended, supported as of `plotly` version 4.9) or the [orca](https://github.com/plotly/orca) command line utility (legacy as of `plotly` version 4.9).

#### Kaleido

The [`kaleido`](https://github.com/plotly/Kaleido) package has no dependencies and can be installed using pip...

$ pip install -U kaleido

or conda.

$ conda install -c plotly python-kaleido

#### Orca

While Kaleido is now the recommended image export approach because it is easier to install and more widely compatible, [static image export](https://plotly.com/python/static-image-export/) can also be supported by the legacy [orca](https://github.com/plotly/orca) command line utility and the [`psutil`](https://github.com/giampaolo/psutil) Python package.

These dependencies can both be installed using conda:

conda install -c plotly plotly-orca==1.3.1 psutil

Or, `psutil` can be installed using pip...

pip install psutil

and orca can be installed according to the instructions in the [orca README](https://github.com/plotly/orca).

#### Extended Geo Support

Some plotly.py features rely on fairly large geographic shape files. The county choropleth figure factory is one such example. These shape files are distributed as a separate `plotly-geo` package. This package can be installed using pip...

$ pip install plotly-geo==1.0.0

or conda.

$ conda install -c plotly plotly-geo=1.0.0

See [_USA County Choropleth Maps in Python_](https://plotly.com/python/county-choropleth/) for more information on the county choropleth figure factory.

#### Chart Studio Support

The `chart-studio` package can be used to upload plotly figures to Plotly's Chart Studio Cloud or On-Prem services. This package can be installed using pip...

$ pip install chart-studio==1.1.0

or conda.

$ conda install -c plotly chart-studio=1.1.0

> **Note:** This package is optional, and if it is not installed it is not possible for figures to be uploaded to the Chart Studio cloud service.

### Where to next?

Once you've installed, you can use our documentation in three main ways:

1.  You jump right in to **examples** of how to make [basic charts](https://plotly.com/python/basic-charts/), [statistical charts](https://plotly.com/python/statistical-charts/), [scientific charts](https://plotly.com/python/scientific-charts/), [financial charts](https://plotly.com/python/financial-charts/), [maps](https://plotly.com/python/maps/), and [3-dimensional charts](https://plotly.com/python/3d-charts/).
2.  If you prefer to learn about the **fundamentals** of the library first, you can read about [the structure of figures](https://plotly.com/python/figure-structure/), [how to create and update figures](https://plotly.com/python/creating-and-updating-figures/), [how to display figures](https://plotly.com/python/renderers/), [how to theme figures with templates](https://plotly.com/python/templates/), [how to export figures to various formats](https://plotly.com/python/static-image-export/) and about [Plotly Express, the high-level API](https://plotly.com/python/plotly-express/) for doing all of the above.
3.  You can check out our exhaustive **reference** guides: the [Python API reference](https://plotly.com/python-api-reference) or the [Figure Reference](https://plotly.com/python/reference)

For information on using Python to build web applications containing plotly figures, see the [_Dash User Guide_](https://dash.plotly.com/).

We also encourage you to join the [Plotly Community Forum](http://community.plotly.com/) if you want help with anything related to `plotly`.

### What About Dash?

[Dash](https://dash.plot.ly/) is an open-source framework for building analytical applications, with no Javascript required, and it is tightly integrated with the Plotly graphing library.

Learn about how to install Dash at [https://dash.plot.ly/installation](https://dash.plot.ly/installation).

Everywhere in this page that you see `fig.show()`, you can display the same figure in a Dash application by passing it to the `figure` argument of the [`Graph` component](https://dash.plot.ly/dash-core-components/graph) from the built-in `dash_core_components` package like this:

import plotly.graph_objects as go # or plotly.express as px
fig = go.Figure() # or any Plotly Express function e.g. px.bar(...)
# fig.add_trace( ... )
# fig.update_layout( ... )

import dash
import dash_core_components as dcc
import dash_html_components as html

app = dash.Dash()
app.layout = html.Div([
    dcc.Graph(figure=fig)
])

app.run_server(debug=True, use_reloader=False)  # Turn off reloader if inside Jupyter