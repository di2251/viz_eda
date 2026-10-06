02_viz
================
di2251
2026-10-06

``` r
library(tidyverse)
library(patchwork)
library(wesanderson)

library(p8105.datasets)
data("weather_df")
```

Revisit scatterplot

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = .5)
```

![](02_viz_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

### Labels

Provide informative axis labels, plot titles, and captions using
`labs()`

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    color = "Location",
    caption = "Data from the rnoaa package"
  )
```

![](02_viz_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

### Scales

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    color = "Location",
    caption = "Data from the rnoaa package") + 
  scale_x_continuous(
    breaks = c(-15, 0, 15),
    labels = c("-15º C", "0", "15")) +
  scale_y_continuous(
    trans = "sqrt",
    position = "right")
```

![](02_viz_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Color adjusting

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) + 
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    color = "Location",
    caption = "Data from the rnoaa package") + 
  scale_color_hue(h = c(100, 300))
```

![](02_viz_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Use `viridis` package color palette

``` r
ggp_temp_plot = 
  weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) + 
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    color = "Location",
    caption = "Data from the rnoaa package"
  ) + 
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE 
  )

ggp_temp_plot
```

![](02_viz_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Use `discrete = TRUE` because the `color` aesthetic is mapped to a
discrete variable

### Themes

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_classic()
```

![](02_viz_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
  theme(legend.position = "bottom")
```

    ## <theme> List of 1
    ##  $ legend.position: chr "bottom"
    ##  @ complete: logi FALSE
    ##  @ validate: logi TRUE

### Setting options

``` r
weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(aes(size = prcp), alpha = .5) +
  geom_smooth(se = FALSE) +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_classic() +
  theme(legend.position = "bottom") +
  labs(
    title = "Max temp over time",
    x = "Date",
    y = "Maximum temperature",
    caption = "max daily temp in three weather dtations in 2020 and 2021"
  ) +
  theme_minimal() +
  theme(legend.position = "bottom")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](02_viz_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Multiple panels with different plot types.

``` r
ggp_tmax_tmin = 
  weather_df |>
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point() +
  theme(legend.position = "none")

ggp_prcp_density = 
  weather_df |>
  filter(prcp > 0) |>
  ggplot(aes(x = prcp, fill = name)) +
  geom_density(alpha = .5) +
  theme(legend.position = "none")

ggp_seasonal = 
  weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point() +
  theme(legend.position = "bottom")

(ggp_tmax_tmin + ggp_prcp_density) / ggp_seasonal
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).
    ## Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
