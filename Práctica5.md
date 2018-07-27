Práctica 5. Más gráficos e intro a estadísticos
================
AE
25/7/2018

Más ggplot &lt;3
----------------

¡Recuerda, poner el directorio!

``` r
setwd("/Users/anaescoto/Dropbox/DGAPA/2018/RMD")
```

En esta práctica vamos a volver a la base de la ENIGH. Con nuestro paquete foreign

``` r
install.packages("foreign", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## Installing package into '/Users/anaescoto/Library/R/3.3/library'
    ## (as 'lib' is unspecified)

    ## 
    ##   There is a binary version available but the source version is
    ##   later:
    ##         binary source needs_compilation
    ## foreign 0.8-69 0.8-71              TRUE

    ## installing the source package 'foreign'

    ## Warning in install.packages("foreign", repos = "http://cran.us.r-
    ## project.org", : installation of package 'foreign' had non-zero exit status

``` r
library(foreign)
```

Llamamos nuestra base

``` r
enigh_concentrado <- read.dbf("concentradohogar.dbf")
```

Vamos a trabajar con la variable "Clase de hogar"

``` r
table(enigh_concentrado$clase_hog)
```

    ## 
    ##     1     2     3     4     5 
    ##  7762 45375 16406   519   249

Etiquetamos la variable

``` r
enigh_concentrado$clase_hog<- factor(enigh_concentrado$clase_hog,label= c("Unipersonal", "Nuclear", "Extenso", "Compuesto","Corresidentes"))
table(enigh_concentrado$clase_hog)
```

    ## 
    ##   Unipersonal       Nuclear       Extenso     Compuesto Corresidentes 
    ##          7762         45375         16406           519           249

Hacemos mismo para el sexo del jefe

``` r
enigh_concentrado$sexo_jefe<- factor(enigh_concentrado$sexo_jefe,label= c("Hombre", "Mujer"))

table(enigh_concentrado$sexo_jefe)
```

    ## 
    ## Hombre  Mujer 
    ##  51918  18393

Gráficos de comparación cuanti y cuali
======================================

Los gráficos de comparaciones de cajas y distribuciones son muy útiles. Nos permiten ver cómo una variable cuantitativa se comporta entre la población dividida entre los grupos.

Vamos a volver a usar nuestro paquete ggplot2

``` r
install.packages("ggplot2", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## Installing package into '/Users/anaescoto/Library/R/3.3/library'
    ## (as 'lib' is unspecified)

    ## also installing the dependency 'vdiffr'

    ## 
    ##   There are binary versions available but the source versions are
    ##   later:
    ##         binary source needs_compilation
    ## vdiffr   0.2.1  0.2.3              TRUE
    ## ggplot2  2.2.1  3.0.0             FALSE

    ## installing the source packages 'vdiffr', 'ggplot2'

    ## Warning in install.packages("ggplot2", repos = "http://cran.us.r-
    ## project.org", : installation of package 'vdiffr' had non-zero exit status

    ## Warning in install.packages("ggplot2", repos = "http://cran.us.r-
    ## project.org", : installation of package 'ggplot2' had non-zero exit status

``` r
library(ggplot2)
```

Vamos a usar la función qplot, otra vez.

Vamos a ver cómo se distribuyen los ingresos

<b>Univariado </b>

``` r
q<-qplot(ing_cor, data=enigh_concentrado, geom="density", 
   main="Distribución de los ingresos corrientes", xlab="Pesos mexicanos", 
   ylab="Density")
q
```

![](Práctica5_files/figure-markdown_github/unnamed-chunk-8-1.png)

Si transformamos a logaritmo, tenemos una distribución más parecida a una normal

``` r
q<-qplot(log(ing_cor), data=enigh_concentrado, geom="density", 
   main="Distribución de los ingresos corrientes", xlab="Logaritmo", 
   ylab="Density")
q
```

    ## Warning: Removed 6 rows containing non-finite values (stat_density).

![](Práctica5_files/figure-markdown_github/unnamed-chunk-9-1.png)

<b>Bivariado</b>

Podemos comparar la distribución incluyendo en la opción "fill=" y poner la variable que separa a nuestro grupos. Para que no se transpongan la distribuciones, ponemos alpha con un intensidad menor a 1.

``` r
q<-qplot(log(ing_cor), data=enigh_concentrado, geom="density", fill=factor(sexo_jefe), alpha=I(.5), 
   main="Distribución de los ingresos corrientes", xlab="Logaritmo", 
   ylab="Density")
q
```

    ## Warning: Removed 6 rows containing non-finite values (stat_density).

![](Práctica5_files/figure-markdown_github/unnamed-chunk-10-1.png)

Podemos comparar la distribución incluyendo en la opción "fill" el nombre la de variable que tenemos de grupo. A veces lo mejor es incluirla como un factor

``` r
q<-qplot(log(ing_cor), data=enigh_concentrado, geom="histogram", fill=factor(sexo_jefe), alpha=I(.5), 
   main="Distribución de los ingresos corrientes", xlab="Logaritmo", 
   ylab="Frecuencia")
q
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 6 rows containing non-finite values (stat_bin).

![](Práctica5_files/figure-markdown_github/unnamed-chunk-11-1.png)

Análisis descriptivo
====================

Un truco para hacer el análisis descriptivo es generar una subbase con las variables que vamos analizar

``` r
mydata<- enigh_concentrado[, c("ing_cor", "tot_integ","ocupados", "sexo_jefe", "clase_hog")]
tail(mydata)
```

    ##        ing_cor tot_integ ocupados sexo_jefe   clase_hog
    ## 70306 28169.64         1        1    Hombre Unipersonal
    ## 70307 24940.72         4        2    Hombre     Extenso
    ## 70308 19814.80         1        0     Mujer Unipersonal
    ## 70309 23160.72         1        1    Hombre Unipersonal
    ## 70310 23385.60         3        0    Hombre     Nuclear
    ## 70311 15888.51         5        3    Hombre     Extenso

Si queremos obtener las medias de estas tres variables, usamos la función sapply. Esto nos permite repetir la función para todo el data frame Excluyendo missing values

``` r
sapply(mydata, mean, na.rm=TRUE)
```

    ## Warning in mean.default(X[[i]], ...): argument is not numeric or logical:
    ## returning NA

    ## Warning in mean.default(X[[i]], ...): argument is not numeric or logical:
    ## returning NA

    ##      ing_cor    tot_integ     ocupados    sexo_jefe    clase_hog 
    ## 42038.245423     3.664548     1.694500           NA           NA

Además de media (mean) tenemos las siguientes opciones: mean, sd, var, min, max, median, range, and quantile.

Para obtener de una sola vez las funciones mean,median,25th and 75th quartiles,min,max, usamos la función "summary"

``` r
summary(mydata)
```

    ##     ing_cor           tot_integ         ocupados       sexo_jefe    
    ##  Min.   :       0   Min.   : 1.000   Min.   : 0.000   Hombre:51918  
    ##  1st Qu.:   18149   1st Qu.: 2.000   1st Qu.: 1.000   Mujer :18393  
    ##  Median :   29837   Median : 4.000   Median : 2.000                 
    ##  Mean   :   42038   Mean   : 3.665   Mean   : 1.694                 
    ##  3rd Qu.:   49232   3rd Qu.: 5.000   3rd Qu.: 2.000                 
    ##  Max.   :35824113   Max.   :21.000   Max.   :11.000                 
    ##          clase_hog    
    ##  Unipersonal  : 7762  
    ##  Nuclear      :45375  
    ##  Extenso      :16406  
    ##  Compuesto    :  519  
    ##  Corresidentes:  249  
    ## 

``` r
summary(mydata[mydata$sexo_jefe=="Hombre",])
```

    ##     ing_cor           tot_integ         ocupados      sexo_jefe    
    ##  Min.   :       0   Min.   : 1.000   Min.   : 0.00   Hombre:51918  
    ##  1st Qu.:   18934   1st Qu.: 3.000   1st Qu.: 1.00   Mujer :    0  
    ##  Median :   30833   Median : 4.000   Median : 2.00                 
    ##  Mean   :   43721   Mean   : 3.837   Mean   : 1.78                 
    ##  3rd Qu.:   50799   3rd Qu.: 5.000   3rd Qu.: 2.00                 
    ##  Max.   :35824113   Max.   :21.000   Max.   :11.00                 
    ##          clase_hog    
    ##  Unipersonal  : 4342  
    ##  Nuclear      :36562  
    ##  Extenso      :10526  
    ##  Compuesto    :  331  
    ##  Corresidentes:  157  
    ## 

``` r
summary(mydata[mydata$sexo_jefe=="Mujer",])
```

    ##     ing_cor          tot_integ         ocupados       sexo_jefe    
    ##  Min.   :      0   Min.   : 1.000   Min.   : 0.000   Hombre:    0  
    ##  1st Qu.:  16225   1st Qu.: 2.000   1st Qu.: 1.000   Mujer :18393  
    ##  Median :  27214   Median : 3.000   Median : 1.000                 
    ##  Mean   :  37287   Mean   : 3.178   Mean   : 1.453                 
    ##  3rd Qu.:  45047   3rd Qu.: 4.000   3rd Qu.: 2.000                 
    ##  Max.   :2702479   Max.   :21.000   Max.   :10.000                 
    ##          clase_hog   
    ##  Unipersonal  :3420  
    ##  Nuclear      :8813  
    ##  Extenso      :5880  
    ##  Compuesto    : 188  
    ##  Corresidentes:  92  
    ## 

Tapply, se parece a sapply, nos permite hacer una función en un subconjunto que se deriva de un vector. Por ejemplo, a continuación podemos pedirle al programa que nos haga un summary del ingreso, tomando en cuenta un vector de factor

``` r
tapply(enigh_concentrado$ing_cor, enigh_concentrado$clase_hog, summary)
```

    ## $Unipersonal
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0   10800   17880   27050   29750 3284000 
    ## 
    ## $Nuclear
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ##        0    18340    29270    42310    47830 35820000 
    ## 
    ## $Extenso
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    2230   23760   37380   47600   58390 2702000 
    ## 
    ## $Compuesto
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    4113   29700   44470   55590   68440  386900 
    ## 
    ## $Corresidentes
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    4621   28220   47240   65300   80670  430800

``` r
tapply(enigh_concentrado$ing_cor, enigh_concentrado$sexo_jefe, summary)
```

    ## $Hombre
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ##        0    18930    30830    43720    50800 35820000 
    ## 
    ## $Mujer
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0   16220   27210   37290   45050 2702000

Usando datos expandidos
=======================

Un elemento importante, sobre todo cuando estamos usando una encuesta con representatividad nacional como la ENIGH del 2016, es que podemos establecer los valores poblacionales. Para ello, los descriptivos se presentan con los datos de la población y no de la muestra. Esto se hace expandiendo los datos muestrales según el factor.

En el caso de la enigh este es factor "factor"

Para esto usaremos la librería "questionr"

``` r
install.packages("questionr", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## Installing package into '/Users/anaescoto/Library/R/3.3/library'
    ## (as 'lib' is unspecified)

    ## 
    ##   There is a binary version available but the source version is
    ##   later:
    ##           binary source needs_compilation
    ## questionr  0.6.2  0.6.3             FALSE

    ## installing the source package 'questionr'

``` r
library(questionr)
```

Este paquete tiene la función wdt.mean, calcula las medias tomando en cuenta los pesos (!Recuerda que estos datos son trimestrales!)

``` r
    wtd.mean(enigh_concentrado$ing_cor, weights = enigh_concentrado$factor)
```

    ## [1] 46520.63

``` r
#También podemos usar el valor con el comando weighted mean
    weighted.mean(x = enigh_concentrado$ing_cor, w = enigh_concentrado$factor)
```

    ## [1] 46520.63

Para variables cualitativas podemos hacer tablas con los factores de expansión

``` r
    wtd.table(enigh_concentrado$sexo_jefe, weights = enigh_concentrado$factor)
```

    ##   Hombre    Mujer 
    ## 24176830  9285768

Para una tabla cruzada

``` r
    wtd.table(enigh_concentrado$sexo_jefe,enigh_concentrado$clase_hog, weights = enigh_concentrado$factor)
```

    ##        Unipersonal  Nuclear  Extenso Compuesto Corresidentes
    ## Hombre     1840569 17011911  5102731    136577         85042
    ## Mujer      1740349  4415294  2986756     89913         53456

Mi primera función
==================

Unos de los elementos más poderosos de R es hacer nuestra propias funciones. Y cuando trabajamos bases de datos a veces hacemos un procedimiento que lo queremos repetir. Tal vez para una base de datos, tal vez para una variable, tal vez para grupos de casos.

Para ello haremos una función sencilla. Para sumarle un valor un 1

``` r
mi_funcion<-function(x) {
    x<-x+1
    x
}

mi_funcion(5)
```

    ## [1] 6

Qué tal hoy una una función donde podamos establecer qué valor sumamos

``` r
mi_funcion<-function(x,a) {
    x<-x+a
    x
}

mi_funcion(5,2)
```

    ## [1] 7

Mi función para expandir medias

``` r
expandir<- function(x){
  weighted.mean(x, w = enigh_concentrado$factor)
}
```

Entonces puedo ya sólo poner la variable

``` r
expandir(enigh_concentrado$edad_jefe)
```

    ## [1] 49.19521

``` r
expandir(enigh_concentrado$ing_cor)
```

    ## [1] 46520.63

Plyr (hermano de dplyr)
=======================

Vamos a instalar el paquete "plyr"

``` r
install.packages("plyr", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## Installing package into '/Users/anaescoto/Library/R/3.3/library'
    ## (as 'lib' is unspecified)

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmpTcvIVR/downloaded_packages

``` r
library(plyr)
```

Este paquete nos ayuda también tener datos ponderados y poderlos hacer para grupos.

``` r
ddply(enigh_concentrado,.(clase_hog),summarise, wm = weighted.mean(ing_cor,factor))
```

    ##       clase_hog       wm
    ## 1   Unipersonal 30961.70
    ## 2       Nuclear 47513.93
    ## 3       Extenso 50022.66
    ## 4     Compuesto 55287.56
    ## 5 Corresidentes 76243.18

Finalmente, si quisiéramos guardar la base de datos en un su formato. Como hemos colocado el directorio, podemos sólo poner el nombre entre comillas

``` r
write.dbf(enigh_concentrado, "enigh_concentrado_mod.dbf" )
```

Guardando todo nuestro Environment
==================================

``` r
save.image("EnviromentD3.RData")
```
