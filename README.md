# CDCVAM

##Source code of the papers:

Centerline Detection on Partial Mesh Scans by Confidence Vote in Accumulation Map
Bertrand Kerautret, Adrien Kraënbühl, Isabelle Debled-Rennesson and Jacques-Olivier Lachaud
To appears at ICPR 2016



Linux/MacOs build: [![Build Status](https://travis-ci.com/kerautret/CDCVAM.svg?token=yURwTCTvpqppf6PxJuXv&branch=master)](https://travis-ci.com/kerautret/CDCVAM)

Windows build: [![Build status](https://ci.appveyor.com/api/projects/status/ta5k0fvxmgaj3y4u?svg=true)](https://ci.appveyor.com/project/kerautret/cdcvam)

## Dependencies

- CMake >= 2.6
- Boost >= 1.46 (modules: program_options)
- PCL for the **compAccFromSDP** tool
- [DGtal](https://github.com/DGtal-team/DGtal)
- [DGtalTools](https://github.com/DGtal-team/DGtalTools) (optional) : provide **3dImageViewer** and **3dSDPViewer** tools
- [DGtalTools-contrib](https://github.com/DGtal-team/DGtalTools-contrib.git) (optional) : provide **graphViewer** tool

Install dependencies CMake, Boost and PCL on Linux :
```
sudo apt-get install cmake libboost-dev libboost-program-options-dev libpcl-dev
```

## Installation

Option -DUSE_PCL=true is only required for the **compAccFromSDP** tool.

1. mkdir build && cd build
2. cmake .. -DUSE_PCL=true
3. make

## Use

### Accumulation and confidence maps

These two maps can be computed from a mesh with **compAccFromMesh**:

```
./bin/compAccFromMesh -i ../Samples/playmobiltree.off --autoScaleAcc rescaledAcc.vol --autoScaleConf rescaledConf.vol -r 9
```

This command uses the *playmobiltree.off* file as input mesh and gives the two maps *accumulation.longvol* and *confidence.longvol* as output files using a accumulation radius of 9 (-r option).
In addition this command gives in output the two files *rescaledAcc.vol* and *rescaledConf.vol* corresponding to the two maps rescaled in [0,255].

Then you can display the maps using the DGtal tool **3dImageViewer**:

```
3dImageViewer -i rescaledAcc.vol --thresholdImage -m 20
3dImageViewer -i rescaledConf.vol --thresholdImage -m 150
```

### Graph of the centerline

The tool **centerLineGeodesicGraph** computes the centerline of a tubular mesh.

```
./bin/centerLineGeodesicGraph -i ../Samples/playmobiltree.off -o playmobilTree --dilateDist 2 -g 3 -R 10 -t 0.5
```

This command uses the *playmobiltree.off* file as input mesh and gives the vertices and edges of the resulting centerline graph in two files : *playmobilTreeVertex.sdp* and *playmobilTreeEdges.sdp*.

The computed graph can be visualized using the **graphViewer** from the DGtalTools-contrib library:

```
graphViewer -m ../Samples/playmobiltree.off  -v playmobilTreeVertex.sdp -e playmobilTreeEdges.sdp --meshColor 200 200 200 125 --edgeColor 200 50 50 255 -b 0.5
```


