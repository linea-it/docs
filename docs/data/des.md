# DES 

DES datasets available at LIneA

----

Levantamento inicial de informações sobre os datasets do DES disponíveis no ambiente

### Archive

#### DR2

There is no DR2 on Archive

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
