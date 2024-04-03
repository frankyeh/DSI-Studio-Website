# Export metrics data from FIB files

> use --action=`exp` to export data from a FIB file

## Examples

*Export dti_fa,ad,md from a FIB file
```
dsi_studio --action=exp --source=subject1.fib.gz --export=dti_fa,ad,md
```

*Export qa,iso from all FIB files
```
dsi_studio --action=exp --source=*.fib.gz --export=qa,iso
```


## Core Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| source |  | Specify the path for the FIB file|
| export |  | Specify the metrics to be exported (for fib.gz). e.g., qa,iso,dti_fa,ad,md |

# Export tract data from TRK or TT files

> use --action=`exp` to export data from a TRK or TT file

## Examples

*Export TRK's tract data into a TT file
```
dsi_studio --action=exp --source=af.trk.gz --output=af.tt.gz
```

*Export all TT's tract data into TRK files
```
dsi_studio --action=exp --source=*.tt.gz --output=*.trk.gz
```

## Core Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| source |  | Specify the file name path for a trk.gz or tt.gz |
| output |  | Specify the output. The file name has to be *.tt.gz or trk.gz |

