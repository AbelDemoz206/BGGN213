# Homework_6
Abel Demoz

``` r
library(bio3d)
s1 <- read.pdb("4AKE") # kinase with drug
```

      Note: Accessing on-line PDB file

``` r
s2 <- read.pdb("1AKE") # kinase no drug
```

      Note: Accessing on-line PDB file
       PDB has ALT records, taking A only, rm.alt=TRUE

``` r
s3 <- read.pdb("1E4Y") # kinase with drug
```

      Note: Accessing on-line PDB file

``` r
s1.chainA <- trim.pdb(s1, chain="A", elety="CA")
s2.chainA <- trim.pdb(s2, chain="A", elety="CA")
s3.chainA <- trim.pdb(s3, chain="A", elety="CA")
s1.b <- s1.chainA$atom$b
s2.b <- s2.chainA$atom$b
s3.b <- s3.chainA$atom$b
plotb3(s1.b, sse=s1.chainA, typ="l", ylab="Bfactor")
```

![](Homework_6_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
plotb3(s2.b, sse=s2.chainA, typ="l", ylab="Bfactor")
```

![](Homework_6_files/figure-commonmark/unnamed-chunk-1-2.png)

``` r
plotb3(s3.b, sse=s3.chainA, typ="l", ylab="Bfactor")
```

![](Homework_6_files/figure-commonmark/unnamed-chunk-1-3.png)

## Use asuupy

``` r
Drug_PI <- function(pdb) {   ## takes the pdb data point as the input 
  s <- read.pdb(pdb)         ## reads the pdb file 
  s_chain <- trim.pdb(s, chain="A", elety= "CA")   ## performs a specfic trim function of siad pdb file (which idk how it works)
  
  s_b <- s_chain$atom$b     ## performs atom specfic on said chain 
  plotb3(s_b, sse=s_chain, typ="l", ylab="Bfactor")   ## plots the accession site of said specific structure

}
```

``` r
Drug_PI("4AKE")
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    /var/folders/_f/_c48_l_x09b103fx36xst2tr0000gn/T//RtmpP9jeBu/4AKE.pdb exists.
    Skipping download

![](Homework_6_files/figure-commonmark/unnamed-chunk-3-1.png)

``` r
Drug_PI("1AKE")
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    /var/folders/_f/_c48_l_x09b103fx36xst2tr0000gn/T//RtmpP9jeBu/1AKE.pdb exists.
    Skipping download

       PDB has ALT records, taking A only, rm.alt=TRUE

![](Homework_6_files/figure-commonmark/unnamed-chunk-4-1.png)

``` r
Drug_PI("1E4Y")
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    /var/folders/_f/_c48_l_x09b103fx36xst2tr0000gn/T//RtmpP9jeBu/1E4Y.pdb exists.
    Skipping download

![](Homework_6_files/figure-commonmark/unnamed-chunk-5-1.png)
