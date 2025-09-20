Lab 02 - Plastic waste
================
Malie Vigneux
22 septembre 2025

## Chargement des packages et des données

``` r
library(tidyverse) 
```

``` r
plastic_waste <- read_csv("data/plastic-waste.csv")
```

Commençons par filtrer les données pour retirer le point représenté par
Trinité et Tobago (TTO) qui est un outlier.

``` r
plastic_waste <- plastic_waste %>%
  filter(plastic_waste_per_cap < 3.5)
```

## Exercices

### Exercise 1

``` r
ggplot(plastic_waste, aes(x=plastic_waste_per_cap)) +
  geom_histogram(binwidth = 0.2) +
  facet_wrap(~continent)
```

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- --> La
plupart de ces continents ont des habitants qui consomment en moyenne
0.25 kg de déchets par jour. L’Afrique et l’Asie ont une bonne quantité
d’habitants qui consomment moins de 0,25 kg/jour de déchets. L’Europe et
l’Amérique du Nord ont aussi un certain pourcentage de la population qui
consomment moins de 0,25 kg/jour de déchets, mais le nombre de ces
habitants est moindre que l’Afrique et l’Asie. On retrouve aussi une
quantité d’habitants qui consomment plus que 0,25 kg/jour de déchets,
tout comme l’Asie, l’Océanie et l’Amérique du Sud.

### Exercise 2

``` r
ggplot(plastic_waste, aes(x = plastic_waste_per_cap, color = continent, fill = continent)) +
  geom_density(alpha = 0.5)
```

![](lab-02_files/figure-gfm/plastic-waste-density-1.png)<!-- -->

Les réglages dans aes() sont des modifications par rapports aux
variables des données. Dans notre cas, “alpha” ne modifie pas la
transparence par rapport à une variable, mais de tout le graphique. Nous
modifions seulement une constante, donc c’est dans geom_density() que ça
se passe.

### Exercise 3

Boxplot:

``` r
ggplot(plastic_waste, aes(x = continent, y = plastic_waste_per_cap)) +
  geom_boxplot()
```

![](lab-02_files/figure-gfm/plastic-waste-boxplot-1.png)<!-- -->

Violin plot:

``` r
ggplot(plastic_waste, aes(x = continent, y = plastic_waste_per_cap)) +
  geom_violin()
```

![](lab-02_files/figure-gfm/plastic-waste-violin-1.png)<!-- -->

Les “violin plots” permettent de voir une meilleure visualisation de la
distribution de déchets par continent que les boxplots. Ils permettent
de visualiser les muliples “pics” (courbes) des données plus facilement.

### Exercise 4

``` r
ggplot(plastic_waste, aes(x = plastic_waste_per_cap, 
                          y = mismanaged_plastic_waste_per_cap,
                          color = continent)) +
  geom_point()
```

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-1.png)<!-- -->

Les pays tels que l’Europe, l’Amérique du Nord et l’Amérique du sud
smeble avoir une tendance à avoir une quatité de déchets gérés plus
élevée.

### Exercise 5

``` r
ggplot(plastic_waste, aes(x = total_pop, 
                          y = plastic_waste_per_cap,
                          color = continent)) +
  geom_point()
```

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-population-total-1.png)<!-- -->

``` r
ggplot(plastic_waste, aes(x = coastal_pop, 
                          y = plastic_waste_per_cap,
                          color = continent)) +
  geom_point()
```

![](lab-02_files/figure-gfm/plastic-waste-population-coastal-1.png)<!-- -->

Il ne semble pas y avoir un graphique avec une plus grande relation
entre ses deux variables que l’autre. La plupart des continents, que ce
soit pour toute leur population ou celle vivant sur une côtière, se
retrouvent du côté gauche du graphique avec un distribution variée. Pour
le très peu de relation qu’il pourrait y avoir, nous pourrions dire que
les continents possédant une plus grande population avec des habitants
sur des côtières, principalement l’Asie, semblent produire une moins
grande quantité de déchets que les autres continents.

## Conclusion

Recréez la visualisation:

``` r
plastic_waste_coastal <- plastic_waste %>% 
  mutate(coastal_pop_prop = coastal_pop / total_pop) %>%
  filter(plastic_waste_per_cap < 3)


ggplot(plastic_waste_coastal, aes(x = coastal_pop_prop, 
                          y = plastic_waste_per_cap,
                          color = continent)) +
  geom_point() +
  labs(title="Quantité de déchets plastiques vs Proportion de la population côtière",
             subtitle="Selon le continent",
             x="Proportion de la population côtière (Coastal / total population)",
             y="Nombre de déchets plastiques par habitants",
             color = "Continent") +
  
  geom_smooth(aes(group = 1),
              color = "black")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 10 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/recreate-viz-1.png)<!-- -->

Interprétation…
