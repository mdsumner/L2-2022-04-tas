# L2-2022-04-tas
a set of swaths from ocean colour L2 around tasmania


# 1. Run the code in dl.R 

(relies on earth data login as per https://oceancolor.gsfc.nasa.gov/data/download_methods/)

# 2. Install gdal for R and some helpers

```R
## install.packages("pak")
pak::pkg_install("vapour")
pak::pkg_install("palr")
pak::pkg_install("scales")
pak::pkg_install(file.path("hypertidy", c("dsn", "ximage", "whatarelief"))
```

# 3. Run the warper to get all the shiz

```R
library(vapour)

sds <- dsn::sds(list.files(".", pattern = ".*\\.nc$"), "/geophysical_data/chlor_a", "NetCDF")
d <- gdal_raster_data(sds,  target_ext = c(-1, 1, -1, 1) * 400000, 
target_res = c(3000, 3000), target_crs = "+proj=laea +lon_0=147 +lat_0=-42", resample = "average", 
   options = c("-multi", "-wo", "NUM_THREADS=ALL_CPUS"))
   
```

# 4. Get colours, make image

```R
library(ximage)
library(palr)
im <- d
pal <- chl_pal(palette = TRUE)
pal$cols <- scales::alpha(pal$cols, 0.7)
im[[1]] <- image_pal(d[[1]], col = pal$cols, breaks = pal$breaks)
ximage(im)
lines(whatarelief::coastline(extent = attr(d, "extent"), projection = attr(d, "projection")))
```

These are the getfiles: 

```
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220430T055401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220430T041801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220430T041200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220430T023600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220429T043600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220429T043000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220429T030000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220429T025401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220428T045400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220428T031801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220428T031200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220427T051800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220427T051201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220427T033600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220427T033000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220426T053000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220426T035400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220426T034801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220425T054800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220425T041201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220425T040601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220424T060601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220424T043001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220424T042400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220424T025400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220424T024800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220423T044800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220423T031200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220423T030601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220422T051200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220422T050600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220422T033001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220422T032400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220421T053000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220421T052401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220421T034801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220421T034200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220420T054201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220420T040600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220420T040001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220419T060000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220419T042400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220419T041801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220419T024200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220418T044201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220418T043600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220418T030600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220418T030000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220417T050000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220417T032400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220417T031801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220416T052400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220416T051800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220416T034201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220416T033601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220415T053601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220415T040001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220415T035400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220414T055401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220414T041800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220414T041200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220414T023601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220413T043600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220413T043001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220413T030001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220413T025400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220412T045401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220412T031800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220412T031200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220411T051801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220411T051200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220411T033600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220411T033001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220410T053000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220410T035401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220410T034801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220409T054801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220409T041201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220409T040600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220408T060601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220408T043000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220408T042400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220408T025400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220408T024801.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220407T044800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220407T031201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220407T030600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220406T051200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220406T050601.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220406T033001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220406T032400.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220405T053001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220405T052401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220405T034800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220405T034201.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220404T054200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220404T040600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220404T040001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220403T060000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220403T042401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220403T041800.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220403T024200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220402T044200.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220402T043600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220402T030600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220402T030001.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220401T050600.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220401T050000.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220401T032401.L2.OC.nc
https://oceandata.sci.gsfc.nasa.gov/ob/getfile/SNPP_VIIRS.20220401T031801.L2.OC.nc
```

figure out the earthdata credentials and do it, 110 files ~9Gb total
