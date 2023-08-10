# Unexpected Behavior with `terra::split` Function

**Issue Overview:**

I am encountering unexpected behavior while attempting to split a polygon into two separate polygons using the `terra::split` function. In the following example, I am using the Bilag.shp dataset from [this repository](https://github.com/derek-corcoran-barrios/SplitError) and trying to split a specific arm of a fjord using a line. The split operation does not behave as intended and extends further than expected.

**Reprex:**

``` r
# Load necessary libraries
library(terra)
#;-) Warning: package 'terra' was built under R version 4.2.3
#;-) terra 1.7.39
library(ggplot2)
#;-) Warning: package 'ggplot2' was built under R version 4.2.3
library(tidyterra)
#;-) Warning: package 'tidyterra' was built under R version 4.2.3
#;-) 
#;-) Attaching package: 'tidyterra'
#;-) The following object is masked from 'package:stats':
#;-) 
#;-)     filter

# Load the dataset and create the splitting line
PartialBilag <- terra::vect("Bilag.shp")

latitudes <- c((55 + 56.245 / 60), (55 + 56.719 / 60))
longitudes <- c((11 + 54.364 / 60), (11 + 54.279 / 60))

Border_Roskilde <- data.frame(lon = longitudes, lat = latitudes) |>
  terra::vect(geom = c("lon", "lat"), crs = "+proj=longlat +datum=WGS84") |>
  terra::project("EPSG:25832") |>
  terra::as.lines()

# Visualize the data
ggplot() +
  geom_spatvector(data = PartialBilag, fill = "blue") +
  geom_spatvector(data = Border_Roskilde, color = "red") +
  theme_bw()
```

![](https://i.imgur.com/WDWyyoE.png)<!-- -->

## Problematic Behavior:

The issue arises when I attempt to use the terra::split function to split the polygon using the defined line:

``` r
Splited_Bilag <- terra::split(PartialBilag, Border_Roskilde)
Splited_Bilag$Polygon <- c("A", "B")

ggplot() +
  geom_spatvector(data = Splited_Bilag, aes(fill = Polygon)) +
  theme_bw()
```

![](https://i.imgur.com/2kNUIn6.png)<!-- -->

*Observation:*

As you can see, the split operation extends the line southwards, covering a larger area than intended. The expected behavior was for the split to occur precisely along the defined line, rather than extending further south.

*Desired Functionality:*

In my understanding, the terra::split function should only utilize the specified line for the splitting operation, without extending the split southwards. I would appreciate clarification on whether this is the intended functionality or if there’s a workaround to achieve the desired outcome.

Thank you for your attention and assistance in resolving this matter.

Best regards,

Derek

<details style="margin-bottom:10px;">
<summary>
Standard output and standard error
</summary>

``` sh
-- nothing to show --
```

</details>
<details style="margin-bottom:10px;">
<summary>
Session info
</summary>

``` r
sessioninfo::session_info()
#;-) ─ Session info ───────────────────────────────────────────────────────────────
#;-)  setting  value
#;-)  version  R version 4.2.2 (2022-10-31 ucrt)
#;-)  os       Windows Server x64 (build 20348)
#;-)  system   x86_64, mingw32
#;-)  ui       RTerm
#;-)  language en
#;-)  collate  Danish_Denmark.utf8
#;-)  ctype    Danish_Denmark.utf8
#;-)  tz       Europe/Paris
#;-)  date     2023-08-10
#;-)  pandoc   2.19.2 @ C:/Program Files/RStudio/resources/app/bin/quarto/bin/tools/ (via rmarkdown)
#;-) 
#;-) ─ Packages ───────────────────────────────────────────────────────────────────
#;-)  package     * version date (UTC) lib source
#;-)  class         7.3-20  2022-01-16 [2] CRAN (R 4.2.2)
#;-)  classInt      0.4-9   2023-02-28 [1] CRAN (R 4.2.3)
#;-)  cli           3.6.1   2023-03-23 [1] CRAN (R 4.2.2)
#;-)  codetools     0.2-18  2020-11-04 [2] CRAN (R 4.2.2)
#;-)  colorspace    2.1-0   2023-01-23 [1] CRAN (R 4.2.2)
#;-)  curl          5.0.0   2023-01-12 [1] CRAN (R 4.2.2)
#;-)  DBI           1.1.3   2022-06-18 [1] CRAN (R 4.2.2)
#;-)  digest        0.6.31  2022-12-11 [1] CRAN (R 4.2.2)
#;-)  dplyr         1.1.2   2023-04-20 [1] CRAN (R 4.2.3)
#;-)  e1071         1.7-13  2023-02-01 [1] CRAN (R 4.2.2)
#;-)  evaluate      0.21    2023-05-05 [1] CRAN (R 4.2.2)
#;-)  fansi         1.0.4   2023-01-22 [1] CRAN (R 4.2.2)
#;-)  farver        2.1.1   2022-07-06 [1] CRAN (R 4.2.2)
#;-)  fastmap       1.1.1   2023-02-24 [1] CRAN (R 4.2.3)
#;-)  fs            1.6.2   2023-04-25 [1] CRAN (R 4.2.3)
#;-)  generics      0.1.3   2022-07-05 [1] CRAN (R 4.2.2)
#;-)  ggplot2     * 3.4.2   2023-04-03 [1] CRAN (R 4.2.3)
#;-)  glue          1.6.2   2022-02-24 [1] CRAN (R 4.2.2)
#;-)  gtable        0.3.3   2023-03-21 [1] CRAN (R 4.2.3)
#;-)  highr         0.10    2022-12-22 [1] CRAN (R 4.2.2)
#;-)  htmltools     0.5.5   2023-03-23 [1] CRAN (R 4.2.3)
#;-)  httr          1.4.5   2023-02-24 [1] CRAN (R 4.2.3)
#;-)  KernSmooth    2.23-20 2021-05-03 [2] CRAN (R 4.2.2)
#;-)  knitr         1.42    2023-01-25 [1] CRAN (R 4.2.2)
#;-)  lifecycle     1.0.3   2022-10-07 [1] CRAN (R 4.2.2)
#;-)  magrittr      2.0.3   2022-03-30 [1] CRAN (R 4.2.2)
#;-)  mime          0.12    2021-09-28 [1] CRAN (R 4.2.0)
#;-)  munsell       0.5.0   2018-06-12 [1] CRAN (R 4.2.2)
#;-)  pillar        1.9.0   2023-03-22 [1] CRAN (R 4.2.3)
#;-)  pkgconfig     2.0.3   2019-09-22 [1] CRAN (R 4.2.2)
#;-)  proxy         0.4-27  2022-06-09 [1] CRAN (R 4.2.2)
#;-)  purrr         1.0.1   2023-01-10 [1] CRAN (R 4.2.2)
#;-)  R.cache       0.16.0  2022-07-21 [1] CRAN (R 4.2.3)
#;-)  R.methodsS3   1.8.2   2022-06-13 [1] CRAN (R 4.2.2)
#;-)  R.oo          1.25.0  2022-06-12 [1] CRAN (R 4.2.2)
#;-)  R.utils       2.12.2  2022-11-11 [1] CRAN (R 4.2.3)
#;-)  R6            2.5.1   2021-08-19 [1] CRAN (R 4.2.2)
#;-)  Rcpp          1.0.10  2023-01-22 [1] CRAN (R 4.2.2)
#;-)  reprex        2.0.2   2022-08-17 [1] CRAN (R 4.2.2)
#;-)  rlang         1.1.1   2023-04-28 [1] CRAN (R 4.2.3)
#;-)  rmarkdown     2.21    2023-03-26 [1] CRAN (R 4.2.3)
#;-)  rstudioapi    0.14    2022-08-22 [1] CRAN (R 4.2.2)
#;-)  scales        1.2.1   2022-08-20 [1] CRAN (R 4.2.2)
#;-)  sessioninfo   1.2.2   2021-12-06 [1] CRAN (R 4.2.3)
#;-)  sf            1.0-12  2023-03-19 [1] CRAN (R 4.2.3)
#;-)  styler        1.10.1  2023-06-05 [1] CRAN (R 4.2.3)
#;-)  terra       * 1.7-39  2023-06-23 [1] CRAN (R 4.2.3)
#;-)  tibble        3.2.1   2023-03-20 [1] CRAN (R 4.2.3)
#;-)  tidyr         1.3.0   2023-01-24 [1] CRAN (R 4.2.2)
#;-)  tidyselect    1.2.0   2022-10-10 [1] CRAN (R 4.2.2)
#;-)  tidyterra   * 0.4.0   2023-03-17 [1] CRAN (R 4.2.3)
#;-)  units         0.8-2   2023-04-27 [1] CRAN (R 4.2.3)
#;-)  utf8          1.2.3   2023-01-31 [1] CRAN (R 4.2.3)
#;-)  vctrs         0.6.2   2023-04-19 [1] CRAN (R 4.2.3)
#;-)  withr         2.5.0   2022-03-03 [1] CRAN (R 4.2.2)
#;-)  xfun          0.39    2023-04-20 [1] CRAN (R 4.2.3)
#;-)  xml2          1.3.4   2023-04-27 [1] CRAN (R 4.2.3)
#;-)  yaml          2.3.7   2023-01-23 [1] CRAN (R 4.2.2)
#;-) 
#;-)  [1] C:/Users/au687614/AppData/Local/R/win-library/4.2
#;-)  [2] C:/Program Files/R/R-4.2.2/library
#;-) 
#;-) ──────────────────────────────────────────────────────────────────────────────
```

</details>
