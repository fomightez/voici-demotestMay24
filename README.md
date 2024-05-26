Presently, the folium example `folium_jupyterlite.ipynb`, isn't working in Voici. (Oddly it was working over on [my older demotest](https://github.com/fomightez/voici-demotestBASEDonOLDrepo) before I changed the name and triggered a rebuild, and so I don't know what changed.) I'm getting the error `TypeError: Can't create an SSLContext object without an ssl module`. I think I need to add SSL into things, according to [here](https://github.com/pyodide/pyodide/issues/529#issuecomment-1542971001) and [here](https://pyodide.org/en/stable/usage/wasm-constraints.html#optional-modules) but not quite sure where/how.  Putting `await pyodide.loadPackage("ssl")` at the top of the folium notebook may work? Not quite, I think but close. I at least worked it out in JupyterLite so far but cannot test folium there because folium works there already:  
I worked it out. According to stuff I worked out above and [here](https://pyodide.org/en/stable/usage/loading-packages.html#how-to-chose-between-micropip-install-and-pyodide-loadpackage), I can load SSL with:

```python 
import pyodide_js
await pyodide_js.loadPackage("ssl")
```

UGHH. I added that to the notbeook before the import of folium and still doesn't work, and is still saying `TypeError: Can't create an SSLContext object without an ssl module`?!?!?
Next I tried adding openssl from [here](https://beta.mamba.pm/channels/emscripten-forge?tab=packages&size=25&index=0&query=openssl), the packages which are referenced in the documentation at the botom [here](https://github.com/voila-dashboards/voici-demo?tab=readme-ov-file). But had to then remove the openssl line in `environment.yml` because otherwise Voici hangs infinitely on 'Startign up kernel...'  
Next I though maybe code [here](https://stackoverflow.com/questions/78495010/finding-similar-dna-sequence-in-a-specific-organism-with-biopythons-blast-modul/78497470#comment138398090_78497470) will help or allow me to troubleshoot more. It seemed to troubleshoot more because adding the `import ssl` fails out with `ModuleNotFoundError: No module named '_ssl'`. But I had run `await pyodide.loadPackage("ssl")`???

# Voici demo

[![lite-badge](https://jupyterlite.rtfd.io/en/latest/_static/badge.svg)](https://fomightez.github.io/voici-demotestMay24)

[Voici](https://github.com/voila-dashboards/voici) deployed as a static site to GitHub Pages, for demo purposes.

It uses [jupyterlite-xeus](https://github.com/jupyterlite/xeus) to build the Emscripten environment, including the **xeus-python** kernel and run dependencies.

## ✨ Try it in your browser ✨

https://fomightez.github.io/voici-demotestMay24

## An example page in dark theme

[dark theme example](https://fomightez.github.io/voici-demotestMay24/voici/render/test_providing_inside_nb.html?theme=dark)   
(Can do for any Voici page: add `?theme=dark` after the `.html`  

## 💡 How to make your own deployment

https://user-images.githubusercontent.com/21197331/223079815-0ea78df4-5173-4adc-a2e4-e10b9593a9f4.webm

Then your site will be published under https://{USERNAME}.github.io/{DEMO_REPO_NAME}

## 📦 How to install extra packages

You can pre-install extra packages by adding them to the ``environment.yml`` file.

For example, if you want to create a Voici deployment with NumPy and Matplotlib pre-installed, you would need to edit the ``environment.yml`` file as following:

```yml
name: voici
channels:
  - https://repo.mamba.pm/emscripten-forge
  - conda-forge
dependencies:
  - xeus-python
  - numpy
  - matplotlib
```

Only ``no-arch`` packages from ``conda-forge`` and packages from ``emscripten-forge`` can be installed.
- **How do I know if a package is ``no-arch`` on ``conda-forge``?** ``no-arch`` means that the package is OS-independent, usually pure-python packages are ``no-arch``. To check if your package is ``no-arch`` on ``conda-forge``, check if the "Platform" entry is "no-arch" in the https://beta.mamba.pm/channels/conda-forge?tab=packages page. If your package is not ``no-arch`` but is a pure Python package, then you should probably update the feedstock to turn your package into a ``no-arch`` one.
![](https://raw.githubusercontent.com/jupyterlite/xeus-python-demo/main/noarch.png)
- **How do I know if my package is on ``emscripten-forge``?** You can see the list of packages pubished on ``emscripten-forge`` [here](https://beta.mamba.pm/channels/emscripten-forge?tab=packages). In case your package is missing, or it's not up-to-date, feel free to open an issue or a PR on https://github.com/emscripten-forge/recipes.
