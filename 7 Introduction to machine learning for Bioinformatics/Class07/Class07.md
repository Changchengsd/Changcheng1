# Class07: Machine learning 1
Changcheng Li (PID: A69027828)

# Clustering

We will start with k-means clustering, one of the most prevalent of all
clustering methods.

To get started let’s make some data up:

``` r
hist( rnorm(10000, mean = 3) )
```

![](Class07_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
tmp <- c( rnorm(30,3), rnorm(30,-3))
x <- cbind(x = tmp, y = rev(tmp))
x
```

                  x         y
     [1,]  3.263660 -2.151674
     [2,]  3.823025 -2.886459
     [3,]  3.235114 -3.724376
     [4,]  3.473038 -3.811519
     [5,]  3.129429 -2.617308
     [6,]  3.289808 -3.715678
     [7,]  1.600452 -2.829884
     [8,]  2.418001 -3.401293
     [9,]  4.446308 -3.827214
    [10,]  3.524910 -4.840933
    [11,]  2.183675 -4.447348
    [12,]  1.458772 -2.405775
    [13,]  3.781186 -2.995890
    [14,]  2.381951 -3.505003
    [15,]  2.572524 -3.965905
    [16,]  2.191022 -3.587386
    [17,]  4.012782 -3.757584
    [18,]  2.925370 -2.733567
    [19,]  3.021339 -3.257657
    [20,]  3.128205 -1.946362
    [21,]  2.135622 -1.283570
    [22,]  2.604177 -2.587849
    [23,]  2.714212 -4.111065
    [24,]  2.466141 -1.368590
    [25,]  4.567795 -2.284618
    [26,]  3.554430 -2.225133
    [27,]  3.404495 -1.840121
    [28,]  3.826040 -1.704362
    [29,]  4.208780 -1.718144
    [30,]  3.705229 -2.546246
    [31,] -2.546246  3.705229
    [32,] -1.718144  4.208780
    [33,] -1.704362  3.826040
    [34,] -1.840121  3.404495
    [35,] -2.225133  3.554430
    [36,] -2.284618  4.567795
    [37,] -1.368590  2.466141
    [38,] -4.111065  2.714212
    [39,] -2.587849  2.604177
    [40,] -1.283570  2.135622
    [41,] -1.946362  3.128205
    [42,] -3.257657  3.021339
    [43,] -2.733567  2.925370
    [44,] -3.757584  4.012782
    [45,] -3.587386  2.191022
    [46,] -3.965905  2.572524
    [47,] -3.505003  2.381951
    [48,] -2.995890  3.781186
    [49,] -2.405775  1.458772
    [50,] -4.447348  2.183675
    [51,] -4.840933  3.524910
    [52,] -3.827214  4.446308
    [53,] -3.401293  2.418001
    [54,] -2.829884  1.600452
    [55,] -3.715678  3.289808
    [56,] -2.617308  3.129429
    [57,] -3.811519  3.473038
    [58,] -3.724376  3.235114
    [59,] -2.886459  3.823025
    [60,] -2.151674  3.263660

``` r
plot(x)
```

![](Class07_files/figure-commonmark/unnamed-chunk-2-1.png)

The main function in R for K-means clustering is called ‘kmeans()’

``` r
k <- kmeans(x, centers = 2, nstart = 20)
k
```

    K-means clustering with 2 clusters of sizes 30, 30

    Cluster means:
              x         y
    1 -2.935950  3.101583
    2  3.101583 -2.935950

    Clustering vector:
     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

    Within cluster sum of squares by cluster:
    [1] 43.75593 43.75593
     (between_SS / total_SS =  92.6 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

``` r
#View(k)
```

> Q1. How many points are in each cluster

``` r
k$size
```

    [1] 30 30

> Q2. The clustering result i.e. membership vector?

``` r
k$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

> Q3. Cluster centers

``` r
k$centers
```

              x         y
    1 -2.935950  3.101583
    2  3.101583 -2.935950

> Q4. Make a plot of our data colored by clustering results with
> optionally the cluster centers shown

``` r
plot(x, col = k$cluster, pch = 16)
points(k$centers, col = "blue", pch = 16, cex =2)
```

![](Class07_files/figure-commonmark/unnamed-chunk-8-1.png)

> Q5. Run kmeans again but cluster into 3 groups and plot results like
> we did above

``` r
k3 <- kmeans(x, centers = 3, nstart = 20)
plot(x, col = k3$cluster, pch = 16)
points(k3$centers, col = "blue", pch = 16, cex =2)
```

![](Class07_files/figure-commonmark/unnamed-chunk-9-1.png)

K-means will always return a clustering result - even if there is no
clear groupings.

\#Hierarchical Clustering

Hierarchical clustering it has an advantage in that it can reveal the
structure in your data rather than imposing a structure as k-means will.

The main function in “base” R is called ‘hclust()’

It requires a distance matrix input.

``` r
hc <- hclust(dist(x))
```

``` r
plot(hc)
abline(h = 8, col = "red")
```

![](Class07_files/figure-commonmark/unnamed-chunk-11-1.png)

The function to get our clusters/groups fron a hclust object is called
‘cutree()’

``` r
grps <- cutree(hc, h=8)
```

> Q. Plot our hclust results in terms of our data colored by cluster
> membership.

``` r
plot(x, col = grps)
```

![](Class07_files/figure-commonmark/unnamed-chunk-13-1.png)

\#Principle component analysis

``` r
#Q1. How many rows and columns are in your new data frame named x? What R functions could you use to answer this questions?
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url)
## Complete the following code to find out how many rows and columns are in x?
dim(x)
```

    [1] 17  5

``` r
## Preview the first 6 rows
head(x)
```

                   X England Wales Scotland N.Ireland
    1         Cheese     105   103      103        66
    2  Carcass_meat      245   227      242       267
    3    Other_meat      685   803      750       586
    4           Fish     147   160      122        93
    5 Fats_and_oils      193   235      184       209
    6         Sugars     156   175      147       139

``` r
# Note how the minus indexing works
rownames(x) <- x[,1]
x <- x[,-1]
head(x)
```

                   England Wales Scotland N.Ireland
    Cheese             105   103      103        66
    Carcass_meat       245   227      242       267
    Other_meat         685   803      750       586
    Fish               147   160      122        93
    Fats_and_oils      193   235      184       209
    Sugars             156   175      147       139

``` r
dim(x)
```

    [1] 17  4

``` r
#A better way for exclude rowname
x <- read.csv(url, row.names=1)
head(x)
```

                   England Wales Scotland N.Ireland
    Cheese             105   103      103        66
    Carcass_meat       245   227      242       267
    Other_meat         685   803      750       586
    Fish               147   160      122        93
    Fats_and_oils      193   235      184       209
    Sugars             156   175      147       139

\#Q2. Which approach to solving the ‘row-names problem’ mentioned above
do you prefer and why? Is one approach more robust than another under
certain circumstances? “x \<- read.csv(url, row.names=1) head(x)” works
more robust because when you rerun the chunk the x \<- x\[,-1\] in the
first method will shrink x

``` r
barplot(as.matrix(x), beside=T, col=rainbow(nrow(x)))
```

![](Class07_files/figure-commonmark/unnamed-chunk-16-1.png)

``` r
#Q3: Changing what optional argument in the above barplot() function results in the following plot?
#beside = FALSE
barplot(as.matrix(x), beside=FALSE, col=rainbow(nrow(x)))
```

![](Class07_files/figure-commonmark/unnamed-chunk-16-2.png)

\#Q5: Generating all pairwise plots may help somewhat. Can you make
sense of the following code and resulting figure? What does it mean if a
given point lies on the diagonal for a given plot? The code uses 17
colors to distinguish different food. The resulting figure are food
consumption paired for the corresponding horizontal and vertical
countries. When a given point lies on the diagonal, it represents that
this food consumption is similar in the two corresponding countries.

``` r
pairs(x, col=rainbow(10), pch=16)
```

![](Class07_files/figure-commonmark/unnamed-chunk-17-1.png)

\#Q6. What is the main differences between N. Ireland and the other
countries of the UK in terms of this data-set? The points are more
dispersed from the diagonal in plots comparing N.Ireland and other
countries than that in other plots(like Fresh_potatoes represented by
the blue point and Fresh_fruit represented by the orange point), which
means food consumption is more different N.Ireland.

\#PCA to rescue

Help me make sense of this data The main function for PCA in base R is
called ‘prcomp()’

``` r
# Use the prcomp() PCA function 
pca <- prcomp( t(x) )
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3       PC4
    Standard deviation     324.1502 212.7478 73.87622 3.176e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.000e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.000e+00

One of the main results that folks look for is called the “score plot”
a.k.a PC plot, PC1 vs PC2 plot.

``` r
plot(pca$x[,1], pca$x[,2])
abline(h = 0, v = 0, col = 'green', lty = "dashed")
```

![](Class07_files/figure-commonmark/unnamed-chunk-19-1.png)

``` r
# Plot PC1 vs PC2
#Q7. Complete the code below to generate a plot of PC1 vs PC2. The second line adds text labels over the data points.
plot(pca$x[,1], pca$x[,2], xlab="PC1", ylab="PC2", xlim=c(-270,500))
text(pca$x[,1], pca$x[,2], colnames(x))
#Q8. Customize your plot so that the colors of the country names match the colors in our UK and Ireland map and table at start of this document.
text(pca$x[,1], pca$x[,2], colnames(x), col = c("red", "yellow", "blue", "green"))
```

![](Class07_files/figure-commonmark/unnamed-chunk-20-1.png)

``` r
v <- round( pca$sdev^2/sum(pca$sdev^2) * 100 )
v
```

    [1] 67 29  4  0

``` r
## or the second row here...
z <- summary(pca)
z$importance
```

                                 PC1       PC2      PC3          PC4
    Standard deviation     324.15019 212.74780 73.87622 3.175833e-14
    Proportion of Variance   0.67444   0.29052  0.03503 0.000000e+00
    Cumulative Proportion    0.67444   0.96497  1.00000 1.000000e+00

``` r
barplot(v, xlab="Principal Component", ylab="Percent Variation")
```

![](Class07_files/figure-commonmark/unnamed-chunk-23-1.png)

``` r
## Lets focus on PC1 as it accounts for > 90% of variance 
par(mar=c(10, 3, 0.35, 0))
barplot( pca$rotation[,1], las=2 )
```

![](Class07_files/figure-commonmark/unnamed-chunk-24-1.png)

``` r
#Q9: Generate a similar ‘loadings plot’ for PC2. What two food groups feature prominantely and what does PC2 mainly tell us about?
par(mar=c(10, 3, 0.35, 0))
barplot( pca$rotation[,2], las=2 )
```

![](Class07_files/figure-commonmark/unnamed-chunk-25-1.png)

``` r
#Fresh_potatoes and Soft_drinks are the two food groups feature prominantely.PC2 tells that Scotland and Wales differ major in consumption of these two food.
```

``` r
library(ggplot2)

df <- as.data.frame(pca$x)
df_lab <- tibble::rownames_to_column(df, "Country")

# Our first basic plot
ggplot(df_lab) + 
  aes(PC1, PC2, col=Country) + 
  geom_point()
```

![](Class07_files/figure-commonmark/unnamed-chunk-26-1.png)

``` r
ggplot(df_lab) + 
  aes(PC1, PC2, col=Country, label=Country) + 
  geom_hline(yintercept = 0, col="gray") +
  geom_vline(xintercept = 0, col="gray") +
  geom_point(show.legend = FALSE) +
  geom_label(hjust=1, nudge_x = -10, show.legend = FALSE) +
  expand_limits(x = c(-300,500)) +
  xlab("PC1 (67.4%)") +
  ylab("PC2 (28%)") +
  theme_bw()
```

![](Class07_files/figure-commonmark/unnamed-chunk-27-1.png)

``` r
ld <- as.data.frame(pca$rotation)
ld_lab <- tibble::rownames_to_column(ld, "Food")

ggplot(ld_lab) +
  aes(PC1, Food) +
  geom_col()
```

![](Class07_files/figure-commonmark/unnamed-chunk-28-1.png)

``` r
ggplot(ld_lab) +
  aes(PC1, reorder(Food, PC1), bg=PC1) +
  geom_col() + 
  xlab("PC1 Loadings/Contributions") +
  ylab("Food Group") +
  scale_fill_gradient2(low="purple", mid="gray", high="darkgreen", guide=NULL) +
  theme_bw()
```

![](Class07_files/figure-commonmark/unnamed-chunk-29-1.png)

``` r
## The inbuilt biplot() can be useful for small datasets 
biplot(pca)
```

![](Class07_files/figure-commonmark/unnamed-chunk-30-1.png)

``` r
url2 <- "https://tinyurl.com/expression-CSV"
rna.data <- read.csv(url2, row.names=1)
head(rna.data)
```

           wt1 wt2  wt3  wt4 wt5 ko1 ko2 ko3 ko4 ko5
    gene1  439 458  408  429 420  90  88  86  90  93
    gene2  219 200  204  210 187 427 423 434 433 426
    gene3 1006 989 1030 1017 973 252 237 238 226 210
    gene4  783 792  829  856 760 849 856 835 885 894
    gene5  181 249  204  244 225 277 305 272 270 279
    gene6  460 502  491  491 493 612 594 577 618 638

``` r
#Q10: How many genes and samples are in this data set?
dim(rna.data)
```

    [1] 100  10

``` r
#100 genes and 10 samples
```

``` r
## Again we have to take the transpose of our data 
pca <- prcomp(t(rna.data), scale=TRUE)
 
## Simple un polished plot of pc1 and pc2
plot(pca$x[,1], pca$x[,2], xlab="PC1", ylab="PC2")
```

![](Class07_files/figure-commonmark/unnamed-chunk-33-1.png)

``` r
summary(pca)
```

    Importance of components:
                              PC1    PC2     PC3     PC4     PC5     PC6     PC7
    Standard deviation     9.6237 1.5198 1.05787 1.05203 0.88062 0.82545 0.80111
    Proportion of Variance 0.9262 0.0231 0.01119 0.01107 0.00775 0.00681 0.00642
    Cumulative Proportion  0.9262 0.9493 0.96045 0.97152 0.97928 0.98609 0.99251
                               PC8     PC9      PC10
    Standard deviation     0.62065 0.60342 3.457e-15
    Proportion of Variance 0.00385 0.00364 0.000e+00
    Cumulative Proportion  0.99636 1.00000 1.000e+00

``` r
plot(pca, main="Quick scree plot")
```

![](Class07_files/figure-commonmark/unnamed-chunk-35-1.png)

``` r
## Variance captured per PC 
pca.var <- pca$sdev^2

## Percent variance is often more informative to look at 
pca.var.per <- round(pca.var/sum(pca.var)*100, 1)
pca.var.per
```

     [1] 92.6  2.3  1.1  1.1  0.8  0.7  0.6  0.4  0.4  0.0

``` r
barplot(pca.var.per, main="Scree Plot", 
        names.arg = paste0("PC", 1:10),
        xlab="Principal Component", ylab="Percent Variation")
```

![](Class07_files/figure-commonmark/unnamed-chunk-37-1.png)

``` r
## A vector of colors for wt and ko samples
colvec <- colnames(rna.data)
colvec[grep("wt", colvec)] <- "red"
colvec[grep("ko", colvec)] <- "blue"

plot(pca$x[,1], pca$x[,2], col=colvec, pch=16,
     xlab=paste0("PC1 (", pca.var.per[1], "%)"),
     ylab=paste0("PC2 (", pca.var.per[2], "%)"))

text(pca$x[,1], pca$x[,2], labels = colnames(rna.data), pos=c(rep(4,5), rep(2,5)))
```

![](Class07_files/figure-commonmark/unnamed-chunk-38-1.png)

``` r
library(ggplot2)

df <- as.data.frame(pca$x)

# Our first basic plot
ggplot(df) + 
  aes(PC1, PC2) + 
  geom_point()
```

![](Class07_files/figure-commonmark/unnamed-chunk-39-1.png)

``` r
# Add a 'wt' and 'ko' "condition" column
df$samples <- colnames(rna.data) 
df$condition <- substr(colnames(rna.data),1,2)

p <- ggplot(df) + 
        aes(PC1, PC2, label=samples, col=condition) + 
        geom_label(show.legend = FALSE)
p
```

![](Class07_files/figure-commonmark/unnamed-chunk-40-1.png)

``` r
p + labs(title="PCA of RNASeq Data",
       subtitle = "PC1 clealy seperates wild-type from knock-out samples",
       x=paste0("PC1 (", pca.var.per[1], "%)"),
       y=paste0("PC2 (", pca.var.per[2], "%)"),
       caption="Class example data") +
     theme_bw()
```

![](Class07_files/figure-commonmark/unnamed-chunk-41-1.png)

``` r
loading_scores <- pca$rotation[,1]

## Find the top 10 measurements (genes) that contribute
## most to PC1 in either direction (+ or -)
gene_scores <- abs(loading_scores) 
gene_score_ranked <- sort(gene_scores, decreasing=TRUE)

## show the names of the top 10 genes
top_10_genes <- names(gene_score_ranked[1:10])
top_10_genes 
```

     [1] "gene100" "gene66"  "gene45"  "gene68"  "gene98"  "gene60"  "gene21" 
     [8] "gene56"  "gene10"  "gene90" 

``` r
sessionInfo()
```

    R version 4.3.2 (2023-10-31 ucrt)
    Platform: x86_64-w64-mingw32/x64 (64-bit)
    Running under: Windows 10 x64 (build 19045)

    Matrix products: default


    locale:
    [1] LC_COLLATE=Chinese (Simplified)_China.utf8 
    [2] LC_CTYPE=Chinese (Simplified)_China.utf8   
    [3] LC_MONETARY=Chinese (Simplified)_China.utf8
    [4] LC_NUMERIC=C                               
    [5] LC_TIME=Chinese (Simplified)_China.utf8    

    time zone: America/Tijuana
    tzcode source: internal

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base     

    other attached packages:
    [1] ggplot2_3.4.4

    loaded via a namespace (and not attached):
     [1] vctrs_0.6.4       cli_3.6.1         knitr_1.45        rlang_1.1.2      
     [5] xfun_0.41         generics_0.1.3    jsonlite_1.8.8    labeling_0.4.3   
     [9] glue_1.6.2        colorspace_2.1-0  htmltools_0.5.7   scales_1.3.0     
    [13] fansi_1.0.5       rmarkdown_2.25    grid_4.3.2        evaluate_0.23    
    [17] munsell_0.5.0     tibble_3.2.1      fastmap_1.1.1     yaml_2.3.7       
    [21] lifecycle_1.0.4   compiler_4.3.2    dplyr_1.1.4       pkgconfig_2.0.3  
    [25] rstudioapi_0.15.0 farver_2.1.1      digest_0.6.33     R6_2.5.1         
    [29] tidyselect_1.2.0  utf8_1.2.4        pillar_1.9.0      magrittr_2.0.3   
    [33] withr_2.5.2       tools_4.3.2       gtable_0.3.4     
