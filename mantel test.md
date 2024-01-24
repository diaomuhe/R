# Mantel test of microbial groups and environmental variables 
## load the packages
```
library(ggplot2)
library(vegan)
library(dplyr)
```
## install package 'linkET'
```
install.packages("devtools")
devtools::install_github("Hy4m/linkET", force = TRUE)
packageVersion("linkET")
library(linkET)
```
## load example from vegan
```
data("varechem", package = "vegan")
data("varespec", package = "vegan")
```
## mantel test
```
mantel <- mantel_test(varespec, varechem, spec_select = list(Spec01 = 1:7,Spec02 = 8:18, Spec03 = 19:37,Spec04 = 38:44)) %>% mutate(rd = cut(r, breaks = c(-Inf, 0.2, 0.4, Inf),labels = c("< 0.2", "0.2 - 0.4", ">= 0.4")), pd = cut(p, breaks = c(-Inf, 0.01, 0.05, Inf), labels = c("< 0.01", "0.01 - 0.05", ">= 0.05")))
```
## make plot of the mantel test result
```
  set_corrplot_style()
  qcorrplot(correlate(varechem), type = "upper", diag = FALSE) +
    geom_square() +
    geom_couple(aes(colour = pd, size = rd), data = mantel, curvature = nice_curvature()) +
    scale_size_manual(values = c(0.5, 1, 2)) +
    scale_colour_manual(values = color_pal(3)) +
    guides(size = guide_legend(title = "Mantel's r",override.aes = list(colour = "grey35"),order = 2),
           colour = guide_legend(title = "Mantel's p", override.aes = list(size = 3),order = 1),
           fill = guide_colorbar(title = "Pearson's r", order = 3))
```
  
