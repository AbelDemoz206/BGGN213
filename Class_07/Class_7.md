# Class 7: Machine Learning 1
Abel (PID 59018056)

Before we get into clustering methods let’s make some sample data to
cluster where we know what the aswer should be.

To help with this, I will use the `rnomrm()` function

``` r
hist(rnorm(150000, mean = 3))
```

![](Class_7_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
n = 10000
hist(c(rnorm(n, mean = 3), rnorm(n, mean=-3)))
```

![](Class_7_files/figure-commonmark/unnamed-chunk-2-1.png)

``` r
n = 30 
x <- c(rnorm(n, mean = 3), rnorm(n, mean=-3))
y <- rev(x)


z <- cbind(x, y)
plot(z)
```

![](Class_7_files/figure-commonmark/unnamed-chunk-3-1.png)

## K-mean clustering

The function in base R for K-means clustering is called `kmeans()`.

``` r
km <- kmeans(z, centers =2)
km
```

    K-means clustering with 2 clusters of sizes 30, 30

    Cluster means:
              x         y
    1 -2.758171  2.907220
    2  2.907220 -2.758171

    Clustering vector:
     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

    Within cluster sum of squares by cluster:
    [1] 49.03411 49.03411
     (between_SS / total_SS =  90.8 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

``` r
km$centers
```

              x         y
    1 -2.758171  2.907220
    2  2.907220 -2.758171

> Q. Print out the cluster membership vectro (i.e our main answer)

``` r
km$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

``` r
plot(z, col = c("red", "blue"))
```

![](Class_7_files/figure-commonmark/unnamed-chunk-7-1.png)

``` r
plot(x, col = c(1,2))
```

![](Class_7_files/figure-commonmark/unnamed-chunk-8-1.png)

Plot with clustering result and add cluster centers:

``` r
plot(z, col = km$cluster)
points(km$centers, col = "blue", pch = 17, cex = 2)
```

![](Class_7_files/figure-commonmark/unnamed-chunk-9-1.png)

\# phc = different shapes \# cex = character ambelishment

> Q. Can you cluster our data in `z` into four cluster please

``` r
km4 <- kmeans(z, centers = 4)
plot(z, col = km4$cluster)
points(km4$centers, col = "blue", pch = 15, cex = 2)
```

![](Class_7_files/figure-commonmark/unnamed-chunk-10-1.png)

## Hierarchal Clustering

The manin function for hierarchal clustering is `hclust()`

Unlike `kmeans()` I cannot just pass in my data as input. I first need a
distance matrix from my data

``` r
d <- dist(z)
hc <- hclust(d)
hc 
```


    Call:
    hclust(d = d)

    Cluster method   : complete 
    Distance         : euclidean 
    Number of objects: 60 

There is a specific hclust plot() method…

``` r
plot(hc)
abline(h = 10, col = "red")
```

![](Class_7_files/figure-commonmark/unnamed-chunk-12-1.png)

To get my main clusterign result (i.e the membership vector) I can “cut”
my tree at a given height. To do this I will use the `cutreee()`

``` r
grps <- cutree(hc, h = 10)
grps
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

``` r
plot(z, col = grps, pch = 1, cex = 2)
```

![](Class_7_files/figure-commonmark/unnamed-chunk-14-1.png)

``` r
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url, row.names = 1)
nrow(x)
```

    [1] 17

``` r
ncol(x)
```

    [1] 4

``` r
 ## x <- x[,-1]

##head(x)
```

``` r
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

``` r
barplot(as.matrix(x), beside=T, col=rainbow(nrow(x)))
```

![](Class_7_files/figure-commonmark/unnamed-chunk-17-1.png)

``` r
pairs(x, col=rainbow(10), pch=16)
```

![](Class_7_files/figure-commonmark/unnamed-chunk-18-1.png)

## Principal Component Analysis (PCA)

The main function to do PCA in base R is called `prcomp()`.

Note that I need to take the transpose of this particlular data as that
is what the `prcomp()` help page was asking for.

``` r
pca <- prcomp(t(x))
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3       PC4
    Standard deviation     324.1502 212.7478 73.87622 2.921e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.000e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.000e+00

Let’s see what is inside our result object `pca` that wwe just calulated

``` r
attributes(pca)
```

    $names
    [1] "sdev"     "rotation" "center"   "scale"    "x"       

    $class
    [1] "prcomp"

``` r
pca$x
```

                     PC1         PC2        PC3           PC4
    England   -144.99315   -2.532999 105.768945 -9.152022e-15
    Wales     -240.52915 -224.646925 -56.475555  5.560040e-13
    Scotland   -91.86934  286.081786 -44.415495 -6.638419e-13
    N.Ireland  477.39164  -58.901862  -4.877895  1.329771e-13

To make our main result figure, called a “PC plot” (or “score plot”,
“ordination plot”, or “PC1 vs PC2 plot”).

``` r
plot(pca$x[,1], pca$x[,2], col = c("black", "red", "blue", "darkblue"), pch = 16, xlab = "PC1(67.4%", ylab = "PC2 (29%")
abline(h = 0, col = "gray", lty = 2)
abline(v = 0, col = "gray", lty = 2)
```

![](Class_7_files/figure-commonmark/unnamed-chunk-22-1.png)

## Variable Loading plot

Can give us insight on how the original variable

``` r
par(mar=c(10, 3, 0.35, 0))
barplot( pca$rotation[,1], las=2 )
```

![](Class_7_files/figure-commonmark/unnamed-chunk-23-1.png)
