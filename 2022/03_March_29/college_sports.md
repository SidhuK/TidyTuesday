March 29 2022
================
Karat Sidhu
2022-03-29

### Loading Libraries

``` r
library(tidyverse)
```
``` r
library(ggtext)
```

### Loading Dataset

``` r
sports <-
  readr::read_csv(
    'https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-03-29/sports.csv'
  )
```

### Filtering data

#### Removing everything except D1- and non profit 4-yr schools

``` r
sports_cleaned <- sports %>%
  filter(classification_name %in% c("NCAA Division I-FCS", "NCAA Division I-FBS")) %>%
  filter(sector_name %in%
           "Public, 4-year or above" | is.na(sector_name)) %>%
  filter(sports %in% c("Basketball", "Football"))
```

#### List of SEC Colleges and Filtering them

``` r
sec_colleges <-
  c(
    "Auburn University",
    "The University of Alabama",
    "University of Arkansas",
    "University of Florida",
    "University of Georgia",
    "University of Kentucky",
    "Louisiana State University and Agricultural & Mechanical College",
    "University of Mississippi",
    "University of Mississippi",
    "University of Missouri-Columbia",
    "University of South Carolina-Columbia",
    "The University of Tennessee-Knoxville",
    "Texas A & M University-College Station",
    "Vanderbilt University"
  )

sec_sports <- sports %>%
  filter(institution_name  %in%  sec_colleges) %>%
  filter(sports %in% c("Basketball", "Football"))
```

#### Texas A&M Filtered.

``` r
texas <- sports_cleaned %>%
  filter(institution_name == "Texas A & M University-College Station")
```

### Adding the logos to use

``` r
sec_logo <- magick::image_read("SEC_logo.png")


texas_logo <- magick::image_read("texas_logo.png")
```

### Making the actual Plot

``` r
png(
  filename = "sec_sports.png",
  width = 1500,
  height = 1200,
  units = "px",
  bg = "white"
)
ggplot(sec_sports) +
  aes(x = year,
      y = total_rev_menwomen,
      group = institution_name) +
  geom_line(size = 1,
            colour = "#112446",
            alpha = 0.4) +
  labs(
    x = "Year",
    y = "Total Revenue (in USD)",
    title = "<span style='color: maroon;'>Texas A&M</span> vs SEC",
    subtitle = "Comparing the total revenue generated from NCAA Football & NCAA Basketball <br> from 2015 to 2019 for the Universities/Colleges part of the South Eastern Conference. <br>
    Texas A&M is represented in <span style='color: maroon;'>maroon</span>",
    caption = "Source: NPR/TidyTuesday Graphic :  github.com/sidhuk"
  ) + theme(panel.grid.major = element_line(size = 0.8),
            axis.text = element_text(size = 15)) +
  ggthemes::theme_solarized_2() +
  theme(
    plot.title = element_text(size = 30,
                              face = "bold", hjust = 0.5),
    plot.subtitle = element_text(size = 16,
                                 face = "italic", hjust = 0.5),
    axis.title.y = element_text(size = 16),
    axis.title.x = element_text(size = 16),
    axis.text = element_text(size = 14),
    strip.text = element_text(size = 16, face = "bold.italic")
  ) +
  facet_wrap(vars(sports), scales = "free", ncol = 1) +
  geom_line(
    data = texas,
    aes(x = year, y = total_rev_menwomen, group = institution_name),
    size = 2,
    color = "maroon"
  ) +
  geom_point(
    data = texas,
    aes(x = year, y = total_rev_menwomen, group = institution_name),
    size = 3,
    color = "maroon"
  ) +
  theme(plot.title = element_markdown(),
        plot.subtitle = element_markdown()) +
  scale_y_continuous(labels = scales::comma)

grid::grid.raster(sec_logo,
                  x = 0.96,
                  y = 0.96,
                  width = unit(0.8, 'inches'))
grid::grid.raster(texas_logo,
                  x = 0.03,
                  y = 0.96,
                  width = unit(1.4, 'inches'))

dev.off()
```

    ## quartz_off_screen 
    ##                 2

### GGSave doesnt work with raster, not used.

``` r
 ggsave("file.jpeg", device = "jpeg",width = 20,
  height = 16,
  dpi = 250)
```

### Display the plot for the record

``` r
ggplot(sec_sports) +
  aes(x = year,
      y = total_rev_menwomen,
      group = institution_name) +
  geom_line(size = 1,
            colour = "#112446",
            alpha = 0.4) +
  labs(
    x = "Year",
    y = "Total Revenue (in USD)",
    title = "<span style='color: maroon;'>Texas A&M</span> vs SEC",
    subtitle = "Comparing the total revenue generated from NCAA Football & NCAA Basketball <br> from 2015 to 2019 for the Universities/Colleges part of the South Eastern Conference. <br>
    Texas A&M is represented in <span style='color: maroon;'>maroon</span>",
    caption = "Source: NPR/TidyTuesday Graphic :  github.com/sidhuk"
  ) + theme(panel.grid.major = element_line(size = 0.8),
            axis.text = element_text(size = 15)) +
  ggthemes::theme_solarized_2() +
  theme(
    plot.title = element_text(size = 30,
                              face = "bold", hjust = 0.5),
    plot.subtitle = element_text(size = 16,
                                 face = "italic", hjust = 0.5),
    axis.title.y = element_text(size = 16),
    axis.title.x = element_text(size = 16),
    axis.text = element_text(size = 14),
    strip.text = element_text(size = 16, face = "bold.italic")
  ) +
  facet_wrap(vars(sports), scales = "free", ncol = 1) +
  geom_line(
    data = texas,
    aes(x = year, y = total_rev_menwomen, group = institution_name),
    size = 2,
    color = "maroon"
  ) +
  geom_point(
    data = texas,
    aes(x = year, y = total_rev_menwomen, group = institution_name),
    size = 3,
    color = "maroon"
  ) +
  theme(plot.title = element_markdown(),
        plot.subtitle = element_markdown()) +
  scale_y_continuous(labels = scales::comma)

grid::grid.raster(sec_logo,
                  x = 0.96,
                  y = 0.96,
                  width = unit(0.8, 'inches'))
grid::grid.raster(texas_logo,
                  x = 0.03,
                  y = 0.96,
                  width = unit(1.4, 'inches'))
```

![](college_sports_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

![](college_sports_files/figure-gfm/sec_sports.png)<!-- -->
