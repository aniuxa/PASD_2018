Ejercicio 1
================
AE
23/7/2018

Repaso de la práctica 1
=======================

1.  Escriba un vector de nombre "x", con los elementos 2,3,7,9

``` r
x <- c(2,3,7,9)
x
```

    ## [1] 2 3 7 9

1.  Escriba un vector "y", con los elementos 9,7,3, 2

``` r
y <- c(9,7,3,2)
y
```

    ## [1] 9 7 3 2

1.  Escriba un vector "year" con los años que van desde 1990 a 1993

``` r
year <- 1990:1993
year
```

    ## [1] 1990 1991 1992 1993

1.  Escriba un vector "name" con los nombres de 4 de sus compañeros de curso

``` r
names <- c("Hugo", "Paco", "Luis", "Daysi")
year
```

    ## [1] 1990 1991 1992 1993

1.  Cree una matriz "m" 2x4 que incluya los valores 101 a 108

``` r
m<-matrix(nrow=2, ncol=4, 101:108, byrow = TRUE)
m
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]  101  102  103  104
    ## [2,]  105  106  107  108

1.  ¿Cuáles son las dimensiones de la matriz "m"?

``` r
dim(m)
```

    ## [1] 2 4

``` r
attributes(m)
```

    ## $dim
    ## [1] 2 4

1.  Cree una matriz "m2" juntado los vectores x y y, por sus filas

``` r
m2<-rbind(x,y)
m2
```

    ##   [,1] [,2] [,3] [,4]
    ## x    2    3    7    9
    ## y    9    7    3    2

1.  ¿Cuáles son las dimensiones de la matriz "m2"?

``` r
dim(m2)
```

    ## [1] 2 4

``` r
attributes(m2)
```

    ## $dim
    ## [1] 2 4
    ## 
    ## $dimnames
    ## $dimnames[[1]]
    ## [1] "x" "y"
    ## 
    ## $dimnames[[2]]
    ## NULL
