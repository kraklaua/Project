Scooby-Doo
================
Sara Knight, Alexa Kraklau
2/23/2022

access code: ghp\_tHkEZaAkwU4wAegNSUPPWjortGDN0g4Wk57J

**Ideas** 1. color-coordinated 2. pull fonts from the internet 5. Keep
the movies/ Get rid of the movies? 6. What do they mean by crossover?

``` r
library(ggplot2)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ tibble  3.1.2     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.1
    ## ✓ purrr   0.3.4

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(dplyr)
library(tidyr)
library(readr)
library(extrafont)
```

    ## Registering fonts with R

``` r
library(infer)
```

In this course, we will:

**Import, manage, and clean data.**

``` r
scoobydoo <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-07-13/scoobydoo.csv')
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_character(),
    ##   index = col_double(),
    ##   date_aired = col_date(format = ""),
    ##   run_time = col_double(),
    ##   monster_amount = col_double(),
    ##   unmask_other = col_logical(),
    ##   caught_other = col_logical(),
    ##   caught_not = col_logical(),
    ##   suspects_amount = col_double(),
    ##   culprit_amount = col_double(),
    ##   door_gag = col_logical(),
    ##   batman = col_logical(),
    ##   scooby_dum = col_logical(),
    ##   scrappy_doo = col_logical(),
    ##   hex_girls = col_logical(),
    ##   blue_falcon = col_logical()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

``` r
scoobycleaned<-scoobydoo[!(scoobydoo$monster_name=="NULL" & scoobydoo$monster_type=="NULL"),]
```

``` r
caught_captured<-scoobydoo %>% 
  select(caught_fred:captured_scooby) %>% 
  pivot_longer(cols = everything(),
               names_to = c("action","character"),
               names_pattern = "(.*)_(.*)",
               values_to = "yes") %>% 
  filter(yes == "TRUE")
```

``` r
scoobydoo_tv <- scoobydoo %>% 
  filter(format == "TV Series")

scoobydoo_tv_segmented <- scoobydoo %>% 
  filter(format == "TV Series (segmented)")

scoobydoo_crossover <- scoobydoo %>%
  filter(format == "Crossover")

scoobydoo_movie <- scoobydoo %>% 
  filter(format == "Movie")
```

``` r
scoobydoo_where_are_you <- scoobydoo %>% 
  filter(series_name == "Scooby Doo, Where Are You!")
scoobydoo_the_new_scoobydoo_movies <- scoobydoo_tv %>% 
  filter(series_name == "The New Scooby-Doo Movies")
the_scoobydoo_show <- scoobydoo %>% 
  filter(series_name == "The Scooby-Doo Show" )
scoobydoo_and_scrappydoo <- scoobydoo %>% 
  filter(series_name == "Scooby-Doo and Scrappy-Doo (first series)")
new_scoobydoo_and_scrappydoo_show <- scoobydoo %>% 
  filter(series_name == "The New Scooby and Scrappy Doo Show")
```

**Create graphical displays and numerical summaries of data for
exploratory analysis and presentations.**

``` r
boxplot1 <- ggplot(data = caught_captured, aes(x = character, fill = action)) +
  geom_bar(stat = "count", position = position_dodge(), alpha = 1,) + 
  scale_fill_manual(values=c("captured" = "#00CFD4",
                             "caught" = "#00E304"))


boxplot1 <- boxplot1 + labs(title = "Captured versus Caught for Each Character",
              subtitle = "Comparison of amount of episodes each character was \ncaptured by monster versus catching the monster",
              x = "Characters", y = "Number of Episodes",
              fill="Action",
              tag = "1") + theme(plot.title = element_text(hjust = 0.5)) +
  theme(plot.subtitle = element_text(hjust = 0.5)) +
  theme(text=element_text(size=10,  family="Times New Roman", color= "blue" ))

boxplot1
```

![](project1_files/figure-gfm/side%20by%20side%20bar%20graph-1.png)<!-- -->
**Write R programs for simulations from probability models and
randomization-based experiments.** **Use source documentation and other
resources to troubleshoot and extend R programs.**

**Write clear, efficient, and well-documented R programs.**

Trying to create a color palette for our project

``` r
 scooby_gang_colors <- function(...) {
   palette <- c(`Shaggy Green` = "#B8B#19",
                   `Shaggy Red Brown` ="#A44138",
                   `Velma Orange` = "#F8991D",
                   `Velma Red` = "#C70C47",
                   `Daphne Green` = "#D0D61B",
                   `Daphne Dark Purple` = "#6352A3",
                   `Scooby Brown` = "#6A3400",
                   `Scooby Blue` = "#7BDB9",
                   `Fred Blue` ="#009DDC",
                   `Fred Orange`= "#F47920",
                   `Mystery Machine Green` = "#00CFD4", 
                   `Mystery Machine Blue` ="#00E304")
  cols <-c(...)
  
  if (is.null(cols))
    return(palette)
  
  palette[cols]
}
#scooby_gang_colors("Shaggy Green")
```

<https://drsimonj.svbtle.com/creating-corporate-colour-palettes-for-ggplot2>

This is my attempt a looking at all the places Mystery Inc has been.

Here I filter the data in order to make a smaller data frame. Then I
changed data points points so I could join this data fram with another.
Finally I filtered out the data points that weren’t useful to making my
map.

This is also an example of creating a graphical display of the world
with a gradient of how many episodes were in the country.

``` r
scoobydoo_earth <- scoobydoo %>% 
  select(setting_country_state)
scoobydoo_earth[scoobydoo_earth == "United States"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Alaska"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Arizona"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "California"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Colorado"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Alaska"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Florida"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Georgia"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Hawaii"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Illinois"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Kentucky"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Kentukey"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Alaska"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Louisiana"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Massachusetts"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Mississippi"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Missouri"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Nevada"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "New Jersey"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "New Mexico"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "New York"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "North Carolina"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Pennsylvania"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Ohio"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Rhode Island"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Tennessee"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Texas"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Vermont"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Washington"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Washington D.C."] <- "USA"
scoobydoo_earth[scoobydoo_earth == "Wisconsin"] <- "USA"
scoobydoo_earth[scoobydoo_earth == "England"] <- "UK"
scoobydoo_earth[scoobydoo_earth == "Scotland"] <- "UK"
scoobydoo_earth[scoobydoo_earth == "Hong Kong"] <- "China"
scoobydoo_earth[scoobydoo_earth == "Siam"] <- "Thailand"
scoobydoo_earth <- scoobydoo_earth %>% 
  group_by(setting_country_state) %>% 
  summarise(num_of_episodes = n()) %>% 
  select(setting_country_state, num_of_episodes) %>% 
  filter(setting_country_state != "Atlantis") %>% 
  filter(setting_country_state != "Bermuda Triangle") %>% 
  filter(setting_country_state != "Indian Reserve") %>%  # Too vague
  filter(setting_country_state != "Mars") %>% 
  filter(setting_country_state != "Moon") %>% 
  filter(setting_country_state != "Pre-Historic") %>% 
  filter(setting_country_state != "Space") %>% 
  filter(setting_country_state != "Africa") %>% # Africa is a continent
  filter(setting_country_state != "Caribean") %>% # Caribean is part of many countries
  filter(setting_country_state != "South America") %>% #South America is a continent
  filter(setting_country_state != "Tibet") #I don't know enough about the history of Tibet
  
names(scoobydoo_earth)[names(scoobydoo_earth) == "setting_country_state"] <- "region"
mapdata <- map_data("world")
mapdata <-left_join(mapdata, scoobydoo_earth, by = "region")
#mapdata1 <- mapdata %>% filter(!is.na(mapdata$num_of_episodes)) This was making it so I couldn't see all the countries and commenting it out fixed it.
map1 <- ggplot(mapdata, aes(x = long, y = lat, group = group))+
  geom_polygon(aes(fill = num_of_episodes), color = "black")
map1
```

![](project1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
scoobydoo_world_map <- map1 + scale_fill_gradient(name = "Number of Episodes", low = scooby_gang_colors("Mystery Machine Blue"), high = scooby_gang_colors("Mystery Machine Green"), na.value = "gray")+
  labs(title = "Mystery Inc Traveling Around the World",
       caption = "Excluded Places inclued: Atlantis, Bermuda Triangle, Mars, Moon, Pre-Historic, and Space", tag = "2")+
  theme(plot.title = element_text(hjust = 0.5))+
  theme(axis.text.x = element_blank(),
      axis.text.y = element_blank(),
      axis.ticks = element_blank(),
      axis.title.x = element_blank(),
      axis.title.y = element_blank(),
      rect = element_blank())
scoobydoo_world_map
```

![](project1_files/figure-gfm/unnamed-chunk-3-2.png)<!-- -->

``` r
#This was an attempt to figure out where the two data frames weren't matching up
#in_scooby_not_in_real_world <- anti_join(scoobydoo_earth, mapdata, by = "region")
```

``` r
scooby_tv_tidy <- scoobydoo_tv %>% 
  select(index, imdb) %>% 
  filter( imdb != "NULL")

scooby_sample <- scooby_tv_tidy %>% 
  rep_sample_n(size = 50, replace = TRUE, reps = 1000)

scooby_sample <- as.data.frame(apply(scooby_sample, 2, as.numeric)) 

scoobyboot <- scooby_sample %>% 
  group_by(index) %>% 
  summarize(mean_imdb = mean(imdb))

scoobyboot
```

    ## # A tibble: 359 x 2
    ##    index mean_imdb
    ##    <dbl>     <dbl>
    ##  1     1       8.1
    ##  2     2       8.1
    ##  3     3       8  
    ##  4     4       7.8
    ##  5     5       7.5
    ##  6     6       8.4
    ##  7     7       7.6
    ##  8     8       8.2
    ##  9     9       8.1
    ## 10    10       8  
    ## # … with 349 more rows

``` r
ggplot(scoobyboot, aes(x = mean_imdb)) +
  geom_histogram(binwidth = .1, color = scooby_gang_colors("Fred Blue"), fill = scooby_gang_colors("Fred Orange")) +
  labs(title = "Distribution of Sample Means from 1000 Resamples", x = "Sampled Mean IMDB", y = "Count", tag = "3")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](project1_files/figure-gfm/graph%20bootstrapping-1.png)<!-- -->

| Header                     | Description                                  |
|:---------------------------|:---------------------------------------------|
| `index`                    | Index ordering based on Scoobypedia (dbl)    |
| `series_name`              | Name of Series (char)                        |
| `network`                  | TV Network TV series takes place in (char)   |
| `season`                   | Season of TV Series (char)                   |
| `title`                    | Title of Show/Movie (char)                   |
| `imbd`                     | Score on IMBD (char)                         |
| `engagment`                | Number of Reviews on IMBD (char)             |
| `date_aired`               | Date aired in US (dbl)                       |
| `run_time`                 | Run time in min (dbl)                        |
| `format`                   | Type of media (char)                         |
| `monster_name`             | Name of monster (char)                       |
| `monster_gender`           | Binary monster gender (char)                 |
| `monster_type`             | Monster type (char)                          |
| `monster_subtype`          | Monster subtype (char)                       |
| `monster_species`          | Monster species (char)                       |
| `monster_real`             | Was monster real (char)                      |
| `monster_amount`           | Monster amount (dbl)                         |
| `caught_fred`              | Caught by Fred (char)                        |
| `caught_daphnie`           | Caught by Daphnie (char)                     |
| `caught_velma`             | Caught by Velma (char)                       |
| `caught_shaggy`            | Caught by Shaggy (char)                      |
| `caught_scooby`            | Caught by Scooby (char)                      |
| `captured_fred`            | Captured Fred (char)                         |
| `captured_daphnie`         | Captured Daphnie (char)                      |
| `captured_velma`           | Captured Velma (char)                        |
| `captured_shaggy`          | Captured Shaggy (char)                       |
| `captured_scooby`          | Captured Scooby (char)                       |
| `unmask_fred`              | Unmasked by Fred (char)                      |
| `unmask_daphnie`           | Unmasked by Daphnie (char)                   |
| `unmask_velma`             | Unmasked by Velma (char)                     |
| `unmask_shaggy`            | Unmasked by Shaggy (char)                    |
| `unmask_scooby`            | Unmasked by Scooby (char)                    |
| `snack_fred`               | Snack eaten by Fred (char)                   |
| `snack_daphnie`            | Snack eaten by Daphnie (char)                |
| `snack_velma`              | Snack eaten by Velma (char)                  |
| `snack_shaggy`             | Snack eaten by Shaggy (char)                 |
| `snack_scooby`             | Snack eaten by Scooby (char)                 |
| `unmask_other`             | Unmasked by other (chat)                     |
| `caught_other`             | Caught by other (char)                       |
| `caught_not`               | Not caught (logical)                         |
| `trap_work_first`          | Trap worked first time (char)                |
| `caught_shaggy`            | Caught by Shaggy (char)                      |
| `setting_terrain`          | Setting type of terrain (char)               |
| `setting_country_state`    | Setting Country State (char)                 |
| `suspect_amount`           | Suspect amount (dbl)                         |
| `non_suspect`              | Non suspect (char)                           |
| `arrested`                 | Arrested (char)                              |
| `culprit_name`             | Culprit name (char)                          |
| `culprit_gender`           | Culprit binary gender(char)                  |
| `culprit_amount`           | Culprit amount (dbl)                         |
| `motive`                   | Motive (char)                                |
| `if_it_wasn't_for`         | Phrase at end the of the show (char)         |
| `door_gag`                 | Door gag happened (logical)                  |
| `number_of_snacks`         | Number of snacks (char)                      |
| `split_up`                 | Did they split up (char)                     |
| `another_mystery`          | Another mystery (char)                       |
| `set_a_trap`               | Did they set a trap (char)                   |
| `jeepers`                  | Times “Jeepers” said (char)                  |
| `jinkies`                  | Times “Jinkies” said (char)                  |
| `my_glasses`               | Times “My glasses” said (char)               |
| `just_about_wrapped_up`    | Times “Just about wrapped up” said (char)    |
| `zoinks`                   | Times “Zoinks” said (char)                   |
| `groovy`                   | Times “Groovy” said (char)                   |
| `scooby_doo_where_are_you` | Times “Scooby doo where are you” said (char) |
| `rooby_rooby_roo`          | Times “Rooby rooby roo” said (char)          |
| `batman`                   | Batman in episode (logical)                  |
| `scooby_dum`               | Scooby Dum in episode (logical)              |
| `scrappy_do`               | Scrappy Doo in episode (logical)             |
| `hex_girls`                | Hex Girls in episode (logical)               |
| `blue_falcon`              | Blue Falcon in episode (logical)             |
| `fred_va`                  | Fred voice actor (char)                      |
| `daphnie_va`               | Daphnie voice actor (char)                   |
| `velma_va`                 | Velma voice actor (char)                     |
| `shaggy_va`                | Shaggy voice actor (char)                    |
| `scooby_va`                | Scooby voice actor (char)                    |

**Questions**

1.  How did the average running time of Scooby-Doo change?

``` r
scoobydoo %>% 
  ggplot(mapping = aes(x = season, y = run_time))+
    geom_boxplot()
```

![](project1_files/figure-gfm/season%20vs.%20airtime-1.png)<!-- --> What
is up with season 2?

``` r
scoobydoo %>% 
  ggplot(mapping = aes(x = index, y = imdb))+
  geom_point()
```

![](project1_files/figure-gfm/season%20vs.%20imbd-1.png)<!-- -->

At the very start it looks like they were highly rated. Then it almost
stabilized a little. Finally all over the place. Where I would go next
is how well are the movies rated compared to shows or crossovers.

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = caught_fred))+
  geom_bar(stat = "identity", fill = scooby_gang_colors("Fred Orange"))
```

![](project1_files/figure-gfm/Monster%20caught%20by%20Fred-1.png)<!-- -->

This is just me playing around with color and trying to graph things.

``` r
scoobycleaned %>% 
    ggplot(mapping = aes(y = season, x = captured_fred))+
  geom_bar(stat = "identity", fill = "light blue")
```

![](project1_files/figure-gfm/Fred%20was%20captured-1.png)<!-- -->

Fred captures the monster only slightly more than getting caught. Get
rid of null?

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = caught_daphnie))+
  geom_bar(stat = "identity", fill = "purple")
```

![](project1_files/figure-gfm/Daphnie%20catches%20the%20monster-1.png)<!-- -->
Daphnie almost never caught the monster.

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = captured_daphnie))+
  geom_bar(stat = "identity", fill = "purple")
```

![](project1_files/figure-gfm/Daphnie%20caught%20by%20the%20monster-1.png)<!-- -->
However she got caught less than I thought (I tried light purple and it
didn’t work)

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = caught_velma))+
  geom_bar(stat = "identity", fill = " dark orange")
```

![](project1_files/figure-gfm/Velma%20caught%20the%20monster-1.png)<!-- -->

Velma was a little more sucessful than Daphnie, but not much

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = captured_velma))+
  geom_bar(stat = "identity", fill = "dark orange")
```

![](project1_files/figure-gfm/Velma%20got%20caught%20by%20the%20monster-1.png)<!-- -->
Looks to close to Daphnie. (I couldn’t use light orange)

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = caught_shaggy))+
  geom_bar(stat = "identity", fill = "light green")
```

![](project1_files/figure-gfm/Shaggy%20caught%20the%20monster-1.png)<!-- -->

Now I’m confused about what null would mean. My first thought would be
the episodes where Shaggy wasn’t in them, but that doesn’t make sense.

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = captured_shaggy))+
  geom_bar(stat = "identity", fill = "light green")
```

![](project1_files/figure-gfm/Shaggy%20caught%20by%20monster-1.png)<!-- -->

Less than Velma and Daphnie, but more than Fred.

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = caught_scooby))+
  geom_bar(stat = "identity", fill = "tan")
```

![](project1_files/figure-gfm/Scooby%20caught%20the%20monster-1.png)<!-- -->

I’m kinda suprised, kinda not that Scooby caught the most monsters.

``` r
scoobycleaned %>% 
  ggplot(mapping = aes(y = season, x = captured_scooby))+
  geom_bar(stat = "identity", fill = "tan")
```

![](project1_files/figure-gfm/Scooby%20caught%20by%20monster-1.png)<!-- -->

``` r
ggplot(data=scoobydoo, aes(x=monster_real, fill=monster_real)) +
    geom_bar(colour="blue", stat="Count") +
    guides(fill=FALSE)
```

    ## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
    ## "none")` instead.

![](project1_files/figure-gfm/bar%20graph-1.png)<!-- -->
