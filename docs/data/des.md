# DES

!!! warning
    Esta página está incompleta, pois está sendo construída

DES datasets available at LIneA

----

Levantamento inicial de informações sobre os datasets do DES disponíveis no ambiente

### Archive

#### DR2

There is no DR2 on Archive

- __Paper or reference:__  https://ui.adsabs.harvard.edu/abs/2021ApJS..255...20A/abstract
- __Description (paper's abstract):__ We present the second public data release of the Dark Energy Survey, DES DR2, based on optical/near-infrared imaging by the Dark Energy Camera mounted on the 4 m Blanco telescope at Cerro Tololo Inter-American Observatory in Chile. DES DR2 consists of reduced single-epoch and coadded images, a source catalog derived from coadded images, and associated data products assembled from 6 yr of DES science operations. This release includes data from the DES wide-area survey covering ~5000 deg2 of the southern Galactic cap in five broad photometric bands, grizY. DES DR2 has a median delivered point-spread function FWHM of g = 1.11 arcsec, r = 0.95 arcsec, i = 0.88 arcsec, z = 0.83 arcsec, and Y = 0.90  arcsec, photometric uniformity with a standard deviation of <3 mmag with respect to Gaia DR2 G band, a photometric accuracy of ~11 mmag, and a median internal astrometric precision of ~27 mas. The median coadded catalog depth for a 1.95 arcsec diameter aperture at signal-to-noise ratio = 10 is g = 24.7, r = 24.4, i = 23.8, z = 23.1, and Y = 21.7 mag. DES DR2 includes ~691 million distinct astronomical objects detected in 10,169 coadded image tiles of size 0.534 deg2 produced from 76,217 single-epoch images. After a basic quality selection, benchmark galaxy and stellar samples contain 543 million and 145 million objects, respectively. These data are accessible through several interfaces, including interactive image visualization tools, web-based query clients, image cutout servers, and Jupyter notebooks. DES DR2 constitutes the largest photometric data set to date at the achieved depth and photometric precision.
- __Data access (catalogs and images):__  
easyaccess (internal collaboration)  
https://desportal.cosmology.illinois.edu/ (internal collaboration)  
https://des.ncsa.illinois.edu/desaccess/ (public)  
https://desportal2.cosmology.illinois.edu/ (public)  
https://datalab.noirlab.edu (public)  

#### Y6A1_COADD and Y6A1_GOLD (and SMALL versions)
```
tree -L 2 Y6A1_COADD/
Y6A1_COADD/
├── Y6A1_COADD
│   ├── cats
│   ├── depth_maps
│   └── masks
├── Y6A1_COADD_SMALL
│   ├── healpix
│   ├── masks
│   └── masks_ -> ../Y6A1_COADD/masks
├── Y6A1_GOLD
│   ├── cats
│   └── masks
└── Y6A1_GOLD_SMALL
    ├── healpix
    └── masks

14 directories, 0 files


du -khs Y6A1_COADD/*/*
3.6T    Y6A1_COADD/Y6A1_COADD/cats
2.4G    Y6A1_COADD/Y6A1_COADD/depth_maps
1.2T    Y6A1_COADD/Y6A1_COADD/masks
46G     Y6A1_COADD/Y6A1_COADD_SMALL/healpix
16K     Y6A1_COADD/Y6A1_COADD_SMALL/masks
0       Y6A1_COADD/Y6A1_COADD_SMALL/masks_
2.3T    Y6A1_COADD/Y6A1_GOLD/cats
368K    Y6A1_COADD/Y6A1_GOLD/masks
3.0G    Y6A1_COADD/Y6A1_GOLD_SMALL/healpix
4.0K    Y6A1_COADD/Y6A1_GOLD_SMALL/masks
```
- __Paper (or reference):__  
https://opensource.ncsa.illinois.edu/confluence/pages/viewpage.action?spaceKey=DESDM&title=Y6A1+Release+Notes  
https://cdcvs.fnal.gov/redmine/projects/des-y6/wiki/Y6_Gold_release  
- __Description:__ This is not a public catalog, only available for the collaboration. Y6A1 is the first version of DES-Y6 coadd catalog and image, covering all five bands. Y6A1_COADD here is the table Y6A1_COADD_OBJECT_SUMMARY available for the collaboration (also available on easyaccess), and the catalog here is the full catalog for Y6A1 coadd, with 186 columns and 691498505 objects. The catalog is available into two sets: balanced and healpix with only nside 32 available.  
There is a version of Y6A1_GOLD catalog, which is a catalog based on Y6A1_COADD, but with a few measurements added. The catalog here has 125 columns of the version Y6_GOLD_1_1 (in easyaccess the versions available are Y6_GOLD_1_1, Y6_GOLD_2_0, and Y6_GOLD_2_1), with 334 columns. The catalog Y6_GOLD_1_1 in easyaccess has 690153156 objects. The catalog is available into two sets: balanced and healpix with only nside 32 available.  
There is a minimal documentation about the depth maps (where they came from, etc).  
The masks are the complete list of files (*.pol, *.area, *.count, *.fits, *.maglims, *.red, *.time, *.weight) needed to run the systematic maps. Files are organized per tile and band (101689 tiles and five bands).  
- __Data access (catalogs and images):__  
easyaccess (internal collaboration)  

#### Y6A2_COADD, Y6A2_GOLD, Y6A2_SOF (and SMALL versions)
```
tree -L 2 Y6A2_COADD/

Y6A2_COADD/	
├── masks	
├── Y6A2_COADD	
│   ├── cats	
│   ├── cats_y6a2	
│   ├── fits	
│   ├── healpix	
│   ├── maps	
│   ├── masks	
│   ├── pngs	
│   └── stilts_converter_Y6A2-coadd_to_Y6A2-small.sh	
├── Y6A2_COADD_SMALL	
│   └── healpix	
├── Y6A2_GOLD	
│   ├── cats	
│   ├── install	
│   ├── Y6A2_GOLD_LIMIT.27000000_OFFSET.0.fits	
│   └── Y6_Gold_2_0.csv	
├── Y6A2_GOLD_SMALL	
│   ├── cats	
│   ├── healpix	
│   └── install	
├── Y6A2_SOF_V2	
│   ├── cats	
│   ├── healpix	
│   ├── install	
│   └── Y6A2_SOF_V2.csv	
└── Y6_Gold_2_0	
└── Y6_Gold_2_0	

du -khs Y6A2_COADD/*/*
274M    Y6A2_COADD/Y6A2_COADD/cats
0       Y6A2_COADD/Y6A2_COADD/cats_y6a2
0       Y6A2_COADD/Y6A2_COADD/fits
898G    Y6A2_COADD/Y6A2_COADD/healpix
0       Y6A2_COADD/Y6A2_COADD/maps
9.1G    Y6A2_COADD/Y6A2_COADD/masks
0       Y6A2_COADD/Y6A2_COADD/pngs
4.0K    Y6A2_COADD/Y6A2_COADD_SMALL/healpix
4.0K    Y6A2_COADD/Y6A2_COADD/stilts_converter_Y6A2-coadd_to_Y6A2-small.sh
1.7T    Y6A2_COADD/Y6A2_GOLD/cats
36K     Y6A2_COADD/Y6A2_GOLD/install
0       Y6A2_COADD/Y6A2_GOLD_SMALL/cats
3.2G    Y6A2_COADD/Y6A2_GOLD_SMALL/healpix
196K    Y6A2_COADD/Y6A2_GOLD_SMALL/install
15G     Y6A2_COADD/Y6A2_GOLD/Y6A2_GOLD_LIMIT.27000000_OFFSET.0.fits
132K    Y6A2_COADD/Y6A2_GOLD/Y6_Gold_2_0.csv
904G    Y6A2_COADD/Y6A2_SOF_V2/cats
0       Y6A2_COADD/Y6A2_SOF_V2/healpix
14M     Y6A2_COADD/Y6A2_SOF_V2/install
132K    Y6A2_COADD/Y6A2_SOF_V2/Y6A2_SOF_V2.csv
0       Y6A2_COADD/Y6_Gold_2_0/Y6_Gold_2_0
```
- __Paper (or reference):__ https://opensource.ncsa.illinois.edu/confluence/pages/viewpage.action?spaceKey=DESDM&title=Y6A1+Release+Notes  
- __Description:__ This is not a public catalog, only available for the collaboration. Y6A2 is the reprocessing of only 73 tiles after detect an issue in align DECam images (see the Y6A1 description for reference). See the list of tiles in https://desar2.cosmology.illinois.edu/DESFiles/desarchive/OPS/multiepoch/Y6A2/r5137/. Files in Y6A2_COADD/Y6A2_COADD/healpix/32/*.fits here is the table Y6A2_COADD_OBJECT_SUMMARY available for the collaboration, with 186 columns and 691483608 objects. The catalog is available into two sets: balanced and healpix with only nside 32 available.  
The masks are the set list of files (*.pol, *.area, *.count, *.fits, *.maglims, *.red, *.time, *.weight) needed to run the systematic maps for only the tiles that changed in Y6A1 to Y6A2.
Y6A2_GOLD is the catalog Y6_GOLD_2_0 version available on easyaccess. The files are split by tile in folder Y6A2_GOLD/cats. The files copied here have the same amount of 333 columns and 691483608 objects.

- __Data access (catalogs and images):__  
easyaccess (internal collaboration)  

**T1**

```
tree -L 2 .
.
├── desar2.cosmology.illinois.edu
│   └── DESFiles
├── dr2
│   ├── fits
│   └── ptifs
├── images
│   └── y6a1_hips
├── process
│   ├── production
│   ├── testing
│   └── testnagios
├── Y6A2_COADD
│   ├── Y6A2_GOLD
│   └── Y6A2_GOLD_SMALL
├── y6a2_gold
│   └── cats
├── Y6A2_GOLD
│   └── cats
├── Y6A2_GOLD_FITS
│   └── BALANCED
└── Y6_SUBSET_HIPS
    ├── AladinBeta.jar
    ├── Hipsgen-cat.jar
    ├── imagens
    └── outputs

23 directories, 2 files

Getting information about datasets volume size
```
