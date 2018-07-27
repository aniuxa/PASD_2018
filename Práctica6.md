Práctica 6. Introducción a la inferencia
================
AE
25/7/2018

Intervalos de confianza
-----------------------

Hemos estado trabajando con estimaciones puntuales

¡Recuerda, poner el directorio!

``` r
setwd("/Users/anaescoto/Dropbox/DGAPA/2018/RMD")
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

La base que usamos es la modificada de la práctica anterior, la 5. Y que debe estar en nuestro directorio de trabajo

``` r
enigh_concentrado <- read.dbf("enigh_concentrado_mod.dbf")
```

t-test
======

Este comando nos sirve para calcular diferentes tipos de test, que tienen como base la distribución t

<b>Univariado para estimación</b> Revisamos los ingresos que los hogares obtienen como parte de su trabajo (Ojo, datos trimestrales!)

``` r
t.test(enigh_concentrado$ingtrab)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  enigh_concentrado$ingtrab
    ## t = 184.31, df = 70310, p-value < 2.2e-16
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  27388.73 27977.50
    ## sample estimates:
    ## mean of x 
    ##  27683.12

<b>Univariado para hipótesis específica</b>

Supongamos que queremos saber si ingreso corriente per cápita es superior la canasta básica urbana trimestral ($8,073.2 pesos)

``` r
#Cálculo ingresos percápita que provienen del trabjao
enigh_concentrado$ing_pc<-enigh_concentrado$ingtrab/enigh_concentrado$tot_integ

t.test(enigh_concentrado$ing_pc, mu=8073.2)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  enigh_concentrado$ing_pc
    ## t = 9.8094, df = 70310, p-value < 2.2e-16
    ## alternative hypothesis: true mean is not equal to 8073.2
    ## 95 percent confidence interval:
    ##  8465.829 8661.908
    ## sample estimates:
    ## mean of x 
    ##  8563.868

prop.test
=========

Si recordamos un poco de la práctica 3, podemos guardar en un objeto la tabla de frecuencias. Podemos también obtener la estimación puntual de nuestra estimación y también podemos obtener de ahí nuestra prueba que incluye la inferencia.

``` r
freq.sex<-table(enigh_concentrado$sexo_jefe)
prop.table(freq.sex)
```

    ## 
    ##    Hombre     Mujer 
    ## 0.7384051 0.2615949

``` r
freq.sex
```

    ## 
    ## Hombre  Mujer 
    ##  51918  18393

``` r
prop.test(freq.sex)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  freq.sex, null probability 0.5
    ## X-squared = 15984, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is not equal to 0.5
    ## 95 percent confidence interval:
    ##  0.7351364 0.7416477
    ## sample estimates:
    ##         p 
    ## 0.7384051

Por default, toma los valores de la primera categoría, también podemos hacer la estimación con los datos del total de "éxitos" y el total de "intentos". Calculemos para las mujeres jefas

``` r
prop.test(18393,70311)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  18393 out of 70311, null probability 0.5
    ## X-squared = 15984, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is not equal to 0.5
    ## 95 percent confidence interval:
    ##  0.2583523 0.2648636
    ## sample estimates:
    ##         p 
    ## 0.2615949

Para hacer la prueba con respecto a un nivel de proporción.

``` r
prop.test(18393,70311, alternative = "greater", p=0.25)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  18393 out of 70311, null probability 0.25
    ## X-squared = 50.353, df = 1, p-value = 6.422e-13
    ## alternative hypothesis: true p is greater than 0.25
    ## 95 percent confidence interval:
    ##  0.2588707 1.0000000
    ## sample estimates:
    ##         p 
    ## 0.2615949

Estimaciones bivariadas
=======================

Diferencias de medias por grupos
--------------------------------

¿Podemos decir, con significancia estadística que los valores medios de una variable son diferentes entre los grupos?

``` r
tapply(enigh_concentrado$ing_pc,enigh_concentrado$sexo_jefe, mean)
```

    ##   Hombre    Mujer 
    ## 8936.747 7511.341

``` r
t.test(enigh_concentrado$ing_pc~enigh_concentrado$sexo_jefe)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  enigh_concentrado$ing_pc by enigh_concentrado$sexo_jefe
    ## t = 13.247, df = 35944, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  1214.500 1636.312
    ## sample estimates:
    ## mean in group Hombre  mean in group Mujer 
    ##             8936.747             7511.341

Gráficamente lo podemos ver en un "boxplot" \[Recuerda que si has seguido las prácticas, no tienes que volver a instalar las librerías\]

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

Ocupamos qplot para graficarlos como un boxplot

``` r
qq<-qplot(sexo_jefe, ing_pc, data = enigh_concentrado, geom = "boxplot")
qq #imprimo mi objeto
```

![](Práctica6_files/figure-markdown_github/unnamed-chunk-10-1.png)
<p>
</p>
Si cambiamos la escala a logarítmica, podemos observar mejor

``` r
qplot(sexo_jefe, log(ing_pc), data = enigh_concentrado, geom = "boxplot")
```

    ## Warning: Removed 6803 rows containing non-finite values (stat_boxplot).

![](Práctica6_files/figure-markdown_github/unnamed-chunk-11-1.png)

Podemos también ver estas diferencias por sexo, con una tercera variable, que serán nuestros "facets""

``` r
qplot(sexo_jefe, log(ing_pc), data = enigh_concentrado, geom = "boxplot") + facet_grid(. ~clase_hog)
```

    ## Warning: Removed 6803 rows containing non-finite values (stat_boxplot).

![](Práctica6_files/figure-markdown_github/unnamed-chunk-12-1.png)

Si no queremos usar el grid (que da control de qué va en horizontal y en lo vertical), podemos usar facet\_wrap

``` r
qplot(sexo_jefe, log(ing_pc), data = enigh_concentrado, geom = "boxplot") + facet_wrap(~clase_hog)
```

    ## Warning: Removed 6803 rows containing non-finite values (stat_boxplot).

![](Práctica6_files/figure-markdown_github/unnamed-chunk-13-1.png)

Prueba chi-cuadrado chi-sq
==========================

Cuando tenemos dos variables cualitativas o nominales podemos hacer esta la prueba chi-cuadrado, o prueba de independencia. Esta tiene una lógica un poco diferente a las pruebas que hacemos, porque proviene de comparar la distribución de los datos dado que no hay independencia entre las variables y los datos que tenemos. La hipótesis nula postula una distribución de probabilidad totalmente especificada como el modelo matemático de la población que ha generado la muestra, por lo que si la rechazamos hemos encontrado evidencia estadística sobre la dependencia de las dos variables.

``` r
table(enigh_concentrado$clase_hog, enigh_concentrado$sexo_jefe)
```

    ##                
    ##                 Hombre Mujer
    ##   Compuesto        331   188
    ##   Corresidentes    157    92
    ##   Extenso        10526  5880
    ##   Nuclear        36562  8813
    ##   Unipersonal     4342  3420

``` r
chisq.test(enigh_concentrado$clase_hog, enigh_concentrado$sexo_jefe)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  enigh_concentrado$clase_hog and enigh_concentrado$sexo_jefe
    ## X-squared = 3192.1, df = 4, p-value < 2.2e-16
