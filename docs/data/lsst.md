# LSST

!!! warning
    Esta página está incompleta, pois está sendo construída

LSST datasets available at LIneA

## Archive

```
├── calexp
├── cosmo_dc2
├── dc2
├── dp0
└── images

5 directories, 0 files

du -khs *
17G     calexp
103G    cosmo_dc2
5.5T    dc2
345M    dp0
36G     images
```

## T1

```
tree -L 2 .
.
├── cosmo_dc2
│   └── EXTRAGALACTIC
├── dp0
│   └── lsst_dp0
├── dp0.2
│   ├── log_dp0.txt
│   ├── md5sum
│   ├── objectTable_tract_2897_DC2_2_2i_runs_DP0_2_v23_0_1_PREOPS-905_step3_1_20220317T233937Z.parq
|   ├── [...] 157 arquivos .parq ocultados
│   ├── objectTable_tract_5074_DC2_2_2i_runs_DP0_2_v23_0_1_PREOPS-905_step3_31_20220314T212509Z.parq
│   └── objectTable_tract.txt
├── dp0_skinny
│   └── DP0
├── dr1
├── dr2
├── gawa_project
│   ├── adriano.pieres
│   ├── input_data
│   ├── outputs
│   ├── raslan.oliveira
│   └── singulani
├── tmp
│   └── henrique.almeida
└── wazp_project
    ├── carlos
    ├── datasets
    ├── tiles_slices
    ├── wazp
    ├── wazp-errors.txt
    ├── wazp.log
    ├── wazp-outfile.txt
    ├── wazp_sdumont
    └── wazp_tiles

24 directories, 163 files

du -khs */*
354G    cosmo_dc2/EXTRAGALACTIC
12G     dp0.2/log_dp0.txt
24K     dp0.2/md5sum
6.6G    dp0.2/objectTable_tract_2897_DC2_2_2i_runs_DP0_2_v23_0_1_PREOPS-905_step3_1_20220317T233937Z.parq
[...]  157 arquivos .parq ocultados
5.9G    dp0.2/objectTable_tract_5074_DC2_2_2i_runs_DP0_2_v23_0_1_PREOPS-905_step3_31_20220314T212509Z.parq
32K     dp0.2/objectTable_tract.txt
2.5T    dp0/lsst_dp0
69G     dp0_skinny/DP0
```
