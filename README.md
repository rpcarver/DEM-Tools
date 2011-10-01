# DEM-Tools #

Python code for working with Digital Elevation Models and tile rendering.

License in "LICENSE" file.

## Contents ##

**Hillup**: package for handling DEM modules and intermediate storage of slope
and aspect in tiled GeoTIFF data format. The **Hillup.data module** simplifies access to digital elevation models from the SRTM and NED data sets. The module is a TileStache provider that automatically downloads elevation data for requested regions. It also renders that data into multiband TIFF files that have precalculated slope and azimuth data for the selected region. The **Hillup.tiles** module uses TileStache and PIL to turn the slope-and-aspect TIFF files generated by hillfarm-seed.py and turn them in to image tiles suitable for use in an online slippy map.

**hillup-seed.py** is a script for pre-generating slope-and-aspect TIFF files for a selected region. It is used to seed the data directories for rendering.

## Installation ##

`python setup.py install`

DEM-Tools relies on a large stack of open source software. Version dependencies are not exact, but just notes on what is known to work.

* Python (2.7)
* gdal (1.8.1) and its dependencies
* PIL (1.1.7)
* ModestMaps (1.2)
* TileStache (1.19.0)

On Debian or Ubuntu, all requirements can be installed via Apt and Python's easy_install/pip.

On MacOS, all requirements can be installed via HomeBrew and Python's easy_install/pip. Install Python first and pay close attention to HomeBrew's caveats about `/usr/local/share/python` in your PATH. There is a **known bug** ([issue #1](https://github.com/migurski/DEM-Tools/issues/1)) on MacOS where the multiband slope-and-aspect TIFFs generated by hillup-seed.py are corrupt and unusuable. For now we generate those input tiles on Linux: rendering works on MacOS.

## Usage ##

1. Clone the git repository.
2. Run `python hillup-seed.py 10`. That will download necessary DEM data and then populate the `out` directory with slope-and-azimuth TIFFs for a small region near San Francisco at zoom level 10. If that works, you can then generate a larger set of TIFFs via a line like
`python hillup-seed.py -b 41 -121 42 -120 4 5 6 7 8 9 10 11 12 13 14 15`
3. install `render/tile.cgi` as a CGI script in your favorite web server. You can then test it by loading a URL like http://localhost/tiles/hills/10/163/395.png where `localhost/tiles/hills` matches the installation path and `10/163/395.png` is the slippy math pap to a tile (in this case, near San Francisco at 37.84, -122.50).

`hillup-seed.py` downloads and generates many gigabytes of data in the `data/out` and `data/source` directories for large scale renders. Provision accordingly.
