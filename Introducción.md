Introducción R
================
AE
23/7/2018

Instalación
===========

R puede ser descargado de una de los sitios "espejo" o "mirror sites"" in <http://cran.r-project.org/mirrors.html>. Escogeremos el más cercano a nuestra locación, en México.

Una vez instalado nuestro programa, es una actividad saludable tener nuestro programa actualizado. Lo más sencillo es correr este código de vez en cuando. Yo lo obtuve de este sitio <https://www.r-statistics.com/2013/03/updating-r-from-r-on-windows-using-the-installr-package/>. Funciona sólo en Windows.

Este código se puede pegar en la consola, donde vemos el signo "&gt;".

    # installing/loading the package:
    if(!require(installr)) { install.packages("installr",  repos = "http://cran.us.r-project.org", dependencies = TRUE); require(installr)} #load / install+load installr
     
    # step by step functions:
    check.for.updates.R() # tells you if there is a new version of R or not.
    install.R() # download and run the latest R installer
    copy.packages.between.libraries() # copy your packages to the newest R installation from the one version before it (if ask=T, it will ask you between which two versions to perform the copying)

Rstudio
=======

En realidad no vamos usar R. ¡Este curso fue una trampa! No. R como pueden ver es programa que no es tan amigable para usuarios principiantes. Vamos a usar un "IDE" (Integrated development environment IDE) que se llama RStudio

Lo vamos a descargar <https://www.rstudio.com/products/rstudio/download/>, vamos a utilizar la primera opción con licencia "Open Source", porque no lo utilizaremos para uso comercial.

Primer acercamiento al uso del programa
=======================================

En RStudio podemos tener varias ventanas que nos permiten tener más control de nuestro "ambiente", el historial, los "scripts" o códigos que escribimos y por supuesto, tenemos nuestra consola, que también tiene el símbolo "&gt;" con R. Podemos pedir operaciones básicas

``` r
2+5
```

    ## [1] 7

``` r
5*3
```

    ## [1] 15

``` r
#Para escribir comentarios y que no los lea como operaciones ponemos el símbolo de gato
# Lo podemos hacer para un comentario en una línea o la par de una instrucción
1:5               # Secuencia 1-5
```

    ## [1] 1 2 3 4 5

``` r
seq(1, 10, 0.5)   # Secuencia con incrementos diferentes a 1
```

    ##  [1]  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0  7.5
    ## [15]  8.0  8.5  9.0  9.5 10.0

``` r
c('a','b','c')  # Vector con caracteres
```

    ## [1] "a" "b" "c"

``` r
1:7             # Entero
```

    ## [1] 1 2 3 4 5 6 7

``` r
40<80           # Valor logico
```

    ## [1] TRUE

``` r
2+2 == 5        # Valor logico
```

    ## [1] FALSE

``` r
T == TRUE       # T expresion corta de verdadero
```

    ## [1] TRUE

R es un lenguaje de programación por objetos. Por lo cual vamos a tener objetos a los que se les asigna su contenido. Si usamos una flechita "&lt;-" o "-&gt;" le estamos asignando algo al objeto que apunta la felcha.

``` r
x <- 24         # Asignacion de valor 24 a la variable x para su uso posterior (OBJETO)
x/2             # Uso posterior de variable u objeto x
```

    ## [1] 12

``` r
x               # Imprime en pantalla el valor de la variable u objeto
```

    ## [1] 24

``` r
x <- TRUE       # Asigna el valor logico TRUE a la variable x OJO: x toma el ultimo valor que se le asigna
x
```

    ## [1] TRUE

Vectores
========

Los vectores son uno de los objetos más usados en R.

``` r
y <- c(2,4,6)     # Vector numerico
y <- c('Primaria', 'Secundaria') # Vector caracteres
```

Dado que poseen elementos, podemos también observar y hacer operaciones con sus elementos, usando "\[ \]" para acceder a ellos

``` r
y[2]              # Acceder al segundo valor del vector y
```

    ## [1] "Secundaria"

``` r
y[3] <- 'Preparatoria y más' # Asigna valor a la tercera componente del vector
sex <-1:2         # Asigna a la variable sex los valores 1 y 2
names(sex) <- c("Femenino", "Masculino") # Asigna nombres al vector de elementos sexo
sex[2]            # Segundo elemento del vector sex
```

    ## Masculino 
    ##         2

Funciones
=========

Algunas funciones básicas son las siguientes. Vamos a ir viendo más funciones, pero para entender cómo funcionan, haremos unos ejemplos y cómo pedir ayuda sobre ellas.

``` r
sum (10,20,30)    # Función suma
```

    ## [1] 60

``` r
rep('R', times=3) # Repite la letra R el numero de veces que se indica
```

    ## [1] "R" "R" "R"

``` r
sqrt(9)           # Raiz cuadrada de 9
```

    ## [1] 3

Ayuda
=====

Pedir ayuda es indispensable para aprender a escribir nuestros códigos. A prueba y error, es el mejor sistema para aprender. Podemos usar la función help, example y ?

``` r
help(sum)         # Ayuda sobre función sum
example(sum)      # Ejemplo de función sum
```

    ## 
    ## sum> ## Pass a vector to sum, and it will add the elements together.
    ## sum> sum(1:5)
    ## [1] 15
    ## 
    ## sum> ## Pass several numbers to sum, and it also adds the elements.
    ## sum> sum(1, 2, 3, 4, 5)
    ## [1] 15
    ## 
    ## sum> ## In fact, you can pass vectors into several arguments, and everything gets added.
    ## sum> sum(1:2, 3:5)
    ## [1] 15
    ## 
    ## sum> ## If there are missing values, the sum is unknown, i.e., also missing, ....
    ## sum> sum(1:5, NA)
    ## [1] NA
    ## 
    ## sum> ## ... unless  we exclude missing values explicitly:
    ## sum> sum(1:5, NA, na.rm = TRUE)
    ## [1] 15

Si no estamos seguro del nombre de la funci´ón de la que tenemos la duda o la que queremos aplicar, pordemos usar la función "apropos" para hacer búsquedas de funciones.

``` r
apropos("nova") 
```

    ## [1] ".__C__anova"          ".__C__anova.glm"      ".__C__anova.glm.null"
    ## [4] "anova"                "manova"               "power.anova.test"    
    ## [7] "stat.anova"           "summary.manova"

Matrices
========

Las matrices son muy importantes, porque nos permiten hacer operaciones y casi todas nuestras bases de datos tendran un aspecto de matriz.

``` r
m <- matrix (nrow=2, ncol=3, 1:6, byrow = TRUE) # Matrices Ejemplo 1
m
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    4    5    6

``` r
m <- matrix (nrow=2, ncol=3, 1:6, byrow = FALSE) # Matrices Ejemplo 1
m
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

``` r
dim(m)
```

    ## [1] 2 3

``` r
attributes(m)
```

    ## $dim
    ## [1] 2 3

``` r
n <- 1:6     # Matrices Ejemplo 2
dim(n) <- c(2,3)
n
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

``` r
xx <-10:12   # Matrices Ejemplo 3
yy<-14:16
cbind(xx,yy) # Une vectores por Columnas
```

    ##      xx yy
    ## [1,] 10 14
    ## [2,] 11 15
    ## [3,] 12 16

``` r
rbind(xx,yy) # Une vectores por Renglones
```

    ##    [,1] [,2] [,3]
    ## xx   10   11   12
    ## yy   14   15   16

``` r
mi_matrix<-cbind(xx,yy) # este resultado lo puedo asignar a un objeto
```

Mi ambiente
===========

Todos los objetos que hemos declarado hasta ahora son parte de nuestro "ambiente" (enviroment). Para saber qué está en nuestro ambiente usamos el comando

``` r
ls()
```

    ## [1] "m"         "mi_matrix" "n"         "sex"       "x"         "xx"       
    ## [7] "y"         "yy"

``` r
gc()           # Garbage collection, reporta memoria en uso
```

    ##          used (Mb) gc trigger (Mb) max used (Mb)
    ## Ncells 386715 20.7     750400 40.1   592000 31.7
    ## Vcells 588473  4.5    1308461 10.0   893106  6.9

Para borrar todos nuestros objetos, usamos el siguiente comando, que equivale a usar la escobita de la venta de environment

``` r
rm(list=ls())  # Borrar objetos actuales
```

Directorio de trabajo
=====================

Es muy útil saber dónde estamos trabajando y donde queremos trabajar. Por eso podemos utilizar los siguientes comandos para saberlo

Ojo, checa, si estás desdes una PC, cómo cambian las "" por "/" o por "\\"

``` r
getwd()           # Directorio actual
```

    ## [1] "/Users/anaescoto/Dropbox/DGAPA/2018/RMD"

``` r
setwd("/Users/anaescoto/Dropbox/DGAPA/2018/RMD")# Cambio de directorio

list.files()      # Lista de archivos en ese directorio
```

    ##  [1] "addin.png"                 "concentradohogar.dbf"     
    ##  [3] "Ej8.html"                  "Ej8.Rmd"                  
    ##  [5] "Ejercicio1.html"           "Ejercicio1.Rmd"           
    ##  [7] "Ejercicio3.html"           "Ejercicio3.Rmd"           
    ##  [9] "enigh_concentrado_mod.dbf" "EnviromentD3.RData"       
    ## [11] "Excel_2014_IMCO.xlsx"      "gastospersona.dbf"        
    ## [13] "Icon\r"                    "Introducción.html"        
    ## [15] "Introducción.Rmd"          "poblacion.dbf"            
    ## [17] "Práctica2.html"            "Práctica2.Rmd"            
    ## [19] "Práctica3.html"            "Práctica3.Rmd"            
    ## [21] "Práctica4_files"           "Práctica4.md"             
    ## [23] "Práctica4.Rmd"             "Práctica5.html"           
    ## [25] "Práctica5.Rmd"             "Práctica6.html"           
    ## [27] "Práctica6.Rmd"             "Práctica7.html"           
    ## [29] "Práctica7.Rmd"             "Práctica8.html"           
    ## [31] "Práctica8.Rmd"             "rsconnect"                
    ## [33] "script.R"                  "trabajos.dbf"             
    ## [35] "viviendas.dbf"
