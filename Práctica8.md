Práctica 8: Merge
================
AE
27/7/2018

Previo
======

Vamos a cargar la base de hogares de la ENIGH con la que hemos estado trabajando.

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
enigh_concentrado <- read.dbf("enigh_concentrado_mod.dbf")
```

Juntando bases
==============

Muchas bases de datos están organizadas en varias tablas. La ventaja de la programación por objetos de R, nos permite tener las bases cargadas en nuestro ambiente y llamarlas y juntarlas cuando sea necesario.

Vamos a suponer que necesitamos usar información de la tabla de viviendas de la ENIGH-2016, de México, con la información a nivel de hogares que hemos estado utilizando.

Entonces cargamos la base de viviendas (la puedes bajar de INEGI , o del sitio "Base de datos" del Curso)

``` r
enigh_viviendas <- read.dbf("viviendas.dbf")
dim(enigh_viviendas)
```

    ## [1] 69169    64

Para juntar bases usamos el comando "merge" En "by" ponemos el id, correspondiente a la variable o variables que forman el id, entrecomillado. Cuando estamos mezclando bases del mismo nivel de análisis el id es igual en ambas bases. Cuando estamos incoporando información de bases de distinto nivel debemos escoger En general ponemos el id de la base de mayor nivel. En este caso, sabemos que a una vivienda corresponde más de un hogar. Tal como revisamos nuestra documentación, sabemos que el id de la tabla viviendas es "folioviv"

``` r
merge_data<- merge(enigh_viviendas, enigh_concentrado, by="folioviv")
```

Revisemos la base creada

``` r
names(merge_data)
```

    ##   [1] "folioviv"    "tipo_viv"    "mat_pared"   "mat_techos"  "mat_pisos"  
    ##   [6] "antiguedad"  "antigua_ne"  "cocina"      "cocina_dor"  "cuart_dorm" 
    ##  [11] "num_cuarto"  "disp_agua"   "dotac_agua"  "excusado"    "uso_compar" 
    ##  [16] "sanit_agua"  "biodigest"   "bano_comp"   "bano_excus"  "bano_regad" 
    ##  [21] "drenaje"     "disp_elect"  "focos_inca"  "focos_ahor"  "combustibl" 
    ##  [26] "estufa_chi"  "eli_basura"  "tenencia"    "renta"       "estim_pago" 
    ##  [31] "pago_viv"    "pago_mesp"   "tipo_adqui"  "viv_usada"   "tipo_finan" 
    ##  [36] "num_dueno1"  "hog_dueno1"  "num_dueno2"  "hog_dueno2"  "escrituras" 
    ##  [41] "lavadero"    "fregadero"   "regadera"    "tinaco_azo"  "cisterna"   
    ##  [46] "pileta"      "calent_sol"  "calent_gas"  "medidor_lu"  "bomba_agua" 
    ##  [51] "tanque_gas"  "aire_acond"  "calefacc"    "tot_resid"   "tot_hom"    
    ##  [56] "tot_muj"     "tot_hog"     "ubica_geo.x" "ageb.x"      "tam_loc.x"  
    ##  [61] "est_socio.x" "est_dis.x"   "upm.x"       "factor.x"    "foliohog"   
    ##  [66] "ubica_geo.y" "ageb.y"      "tam_loc.y"   "est_socio.y" "est_dis.y"  
    ##  [71] "upm.y"       "factor.y"    "clase_hog"   "sexo_jefe"   "edad_jefe"  
    ##  [76] "educa_jefe"  "tot_integ"   "hombres"     "mujeres"     "mayores"    
    ##  [81] "menores"     "p12_64"      "p65mas"      "ocupados"    "percep_ing" 
    ##  [86] "perc_ocupa"  "ing_cor"     "ingtrab"     "trabajo"     "sueldos"    
    ##  [91] "horas_extr"  "comisiones"  "aguinaldo"   "indemtrab"   "otra_rem"   
    ##  [96] "remu_espec"  "negocio"     "noagrop"     "industria"   "comercio"   
    ## [101] "servicios"   "agrope"      "agricolas"   "pecuarios"   "reproducc"  
    ## [106] "pesca"       "otros_trab"  "rentas"      "utilidad"    "arrenda"    
    ## [111] "transfer"    "jubilacion"  "becas"       "donativos"   "remesas"    
    ## [116] "bene_gob"    "transf_hog"  "trans_inst"  "estim_alqu"  "otros_ing"  
    ## [121] "gasto_mon"   "alimentos"   "ali_dentro"  "cereales"    "carnes"     
    ## [126] "pescado"     "leche"       "huevo"       "aceites"     "tuberculo"  
    ## [131] "verduras"    "frutas"      "azucar"      "cafe"        "especias"   
    ## [136] "otros_alim"  "bebidas"     "ali_fuera"   "tabaco"      "vesti_calz" 
    ## [141] "vestido"     "calzado"     "vivienda"    "alquiler"    "pred_cons"  
    ## [146] "agua"        "energia"     "limpieza"    "cuidados"    "utensilios" 
    ## [151] "enseres"     "salud"       "atenc_ambu"  "hospital"    "medicinas"  
    ## [156] "transporte"  "publico"     "foraneo"     "adqui_vehi"  "mantenim"   
    ## [161] "refaccion"   "combus"      "comunica"    "educa_espa"  "educacion"  
    ## [166] "esparci"     "paq_turist"  "personales"  "cuida_pers"  "acces_pers" 
    ## [171] "otros_gas"   "transf_gas"  "percep_tot"  "retiro_inv"  "prestamos"  
    ## [176] "otras_perc"  "ero_nm_viv"  "ero_nm_hog"  "erogac_tot"  "cuota_viv"  
    ## [181] "mater_serv"  "material"    "servicio"    "deposito"    "prest_terc" 
    ## [186] "pago_tarje"  "deudas"      "balance"     "otras_erog"  "smg"

``` r
dim(merge_data)
```

    ## [1] 70311   190

¿Qué observas?

1.  El orden de las variables corresponde al orden que pusimos las bases en las opciones.

2.  También vemos que las variables que se repetían en ambas bases se repiten en la nueva base, seguida de un un punto y una "x", para lo que proviene de la primera base y con una "y", lo que proviene de la segunda. R dejará las variables intactas y son coincidentes, en nuestro caso, porque las variables son iguales. R hace esto para precaver que por error tengamos alguna variable con un nombre igual y no sea la misma

Revisemos que las variables "repetidas son iguales

``` r
table(merge_data$est_socio.x, merge_data$est_socio.y)
```

    ##    
    ##         1     2     3     4
    ##   1 16017     0     0     0
    ##   2     0 37186     0     0
    ##   3     0     0 12464     0
    ##   4     0     0     0  4644

Merge hogar a población (id compuesto)
======================================

Hoy queremos incorporar la información de hogar a la base de población Vamos a cargar la base.

Cargamos la base

``` r
enigh_pobla <- read.dbf("poblacion.dbf")
dim(enigh_pobla)
```

    ## [1] 257805    178

Aquí es más evidente la diferencia del nivel de las bases de las bases porque hay 257,805 personas en 70,311 hogares en 69,169 viviendas

Tal como está en la documentación de la ENIGH (<http://www.beta.inegi.org.mx/contenidos/proyectos/enchogares/regulares/enigh/nc/2016/doc/702825091996.pdf>), el id o llave única de las bases son las siguientes

concentrado {hogares} es "folioviv" "foliohog" población {individuos} es "folioviv" "foliohog" "numren"

Esto significa que tenemos un id compuesto. No es una sola variable. Para esto modificamos ligeramente cómo ponemos el "by", pero siempre eligiendo el id de la base de mayor nivel. (Tené cuidado con los paréntesis)

``` r
merge_data2<- merge(enigh_concentrado, enigh_pobla, by=c("folioviv","foliohog"))
dim(merge_data2)
```

    ## [1] 257805    303

Revisemos la base

``` r
str(merge_data2)
```

    ## 'data.frame':    257805 obs. of  303 variables:
    ##  $ folioviv  : Factor w/ 69169 levels "0100003801","0100003802",..: 1 1 2 2 3 3 3 3 3 3 ...
    ##  $ foliohog  : Factor w/ 5 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ ubica_geo : Factor w/ 3103 levels "010010001","010010102",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ ageb      : Factor w/ 3464 levels "001-0","001-1",..: 245 245 245 245 245 245 245 245 245 245 ...
    ##  $ tam_loc   : Factor w/ 4 levels "1","2","3","4": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ est_socio : Factor w/ 4 levels "1","2","3","4": 4 4 4 4 4 4 4 4 4 4 ...
    ##  $ est_dis   : Factor w/ 536 levels "001","002","003",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ upm       : Factor w/ 7891 levels "0000010","0000020",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ factor    : int  247 247 247 247 247 247 247 247 247 247 ...
    ##  $ clase_hog : Factor w/ 5 levels "Compuesto","Corresidentes",..: 4 4 4 4 4 4 4 4 4 4 ...
    ##  $ sexo_jefe : Factor w/ 2 levels "Hombre","Mujer": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ edad_jefe : int  33 33 29 29 47 47 47 47 47 47 ...
    ##  $ educa_jefe: Factor w/ 11 levels "01","02","03",..: 10 10 10 10 10 10 10 10 10 10 ...
    ##  $ tot_integ : int  2 2 2 2 6 6 6 6 6 6 ...
    ##  $ hombres   : int  1 1 1 1 2 2 2 2 2 2 ...
    ##  $ mujeres   : int  1 1 1 1 4 4 4 4 4 4 ...
    ##  $ mayores   : int  2 2 2 2 3 3 3 3 3 3 ...
    ##  $ menores   : int  0 0 0 0 3 3 3 3 3 3 ...
    ##  $ p12_64    : int  2 2 2 2 3 3 3 3 3 3 ...
    ##  $ p65mas    : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ ocupados  : int  2 2 2 2 1 1 1 1 1 1 ...
    ##  $ percep_ing: int  2 2 2 2 1 1 1 1 1 1 ...
    ##  $ perc_ocupa: int  2 2 2 2 1 1 1 1 1 1 ...
    ##  $ ing_cor   : num  100697 100697 146616 146616 94623 ...
    ##  $ ingtrab   : num  100697 100697 144157 144157 82623 ...
    ##  $ trabajo   : num  100697 100697 144157 144157 82623 ...
    ##  $ sueldos   : num  97377 97377 91475 91475 82623 ...
    ##  $ horas_extr: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ comisiones: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ aguinaldo : num  3320 3320 10820 10820 0 ...
    ##  $ indemtrab : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ otra_rem  : num  0 0 7623 7623 0 ...
    ##  $ remu_espec: num  0 0 34239 34239 0 ...
    ##  $ negocio   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ noagrop   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ industria : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ comercio  : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ servicios : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ agrope    : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ agricolas : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ pecuarios : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ reproducc : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ pesca     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ otros_trab: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ rentas    : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ utilidad  : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ arrenda   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ transfer  : num  0 0 2459 2459 0 ...
    ##  $ jubilacion: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ becas     : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ donativos : num  0 0 2459 2459 0 ...
    ##  $ remesas   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ bene_gob  : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ transf_hog: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ trans_inst: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ estim_alqu: num  0 0 0 0 12000 12000 12000 12000 12000 12000 ...
    ##  $ otros_ing : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ gasto_mon : num  46600 46600 82428 82428 54793 ...
    ##  $ alimentos : num  10622 10622 14638 14638 21214 ...
    ##  $ ali_dentro: num  9953 9953 6281 6281 13114 ...
    ##  $ cereales  : num  1764 1764 945 945 1877 ...
    ##  $ carnes    : num  1920 1920 579 579 2803 ...
    ##  $ pescado   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ leche     : num  1130 1130 951 951 3381 ...
    ##  $ huevo     : num  0 0 675 675 244 ...
    ##  $ aceites   : num  0 0 0 0 334 ...
    ##  $ tuberculo : num  0 0 0 0 193 ...
    ##  $ verduras  : num  154 154 180 180 1466 ...
    ##  $ frutas    : num  489 489 0 0 1466 ...
    ##  $ azucar    : num  450 450 0 0 0 ...
    ##  $ cafe      : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ especias  : num  1013 1013 0 0 0 ...
    ##  $ otros_alim: num  1580 1580 2775 2775 514 ...
    ##  $ bebidas   : num  1453 1453 176 176 836 ...
    ##  $ ali_fuera : num  669 669 8357 8357 8100 ...
    ##  $ tabaco    : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ vesti_calz: num  1888 1888 9416 9416 3463 ...
    ##  $ vestido   : num  1888 1888 5674 5674 2387 ...
    ##  $ calzado   : num  0 0 3742 3742 1076 ...
    ##  $ vivienda  : num  10544 10544 9526 9526 930 ...
    ##  $ alquiler  : num  8100 8100 9000 9000 0 0 0 0 0 0 ...
    ##  $ pred_cons : num  450 450 0 0 0 0 0 0 0 0 ...
    ##  $ agua      : num  750 750 291 291 630 630 630 630 630 630 ...
    ##  $ energia   : num  1244 1244 236 236 300 ...
    ##  $ limpieza  : num  15343 15343 8407 8407 9278 ...
    ##  $ cuidados  : num  1425 1425 1524 1524 1206 ...
    ##  $ utensilios: num  0 0 342 342 695 ...
    ##  $ enseres   : num  13918 13918 6541 6541 7377 ...
    ##  $ salud     : num  983 983 4397 4397 1448 ...
    ##  $ atenc_ambu: num  983 983 1590 1590 0 ...
    ##  $ hospital  : num  0 0 2808 2808 0 ...
    ##  $ medicinas : num  0 0 0 0 1448 ...
    ##  $ transporte: num  3450 3450 10367 10367 13050 ...
    ##  $ publico   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ foraneo   : num  0 0 2754 2754 0 ...
    ##  $ adqui_vehi: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ mantenim  : num  2100 2100 5363 5363 9000 ...
    ##  $ refaccion : num  0 0 2213 2213 0 ...
    ##  $ combus    : num  2100 2100 3150 3150 9000 9000 9000 9000 9000 9000 ...
    ##   [list output truncated]

``` r
tail(merge_data2)
```

    ##          folioviv foliohog ubica_geo  ageb tam_loc est_socio est_dis
    ## 257800 3260801905        1 320580001 002-A       4         2     536
    ## 257801 3260801906        1 320580001 002-A       4         2     536
    ## 257802 3260801906        1 320580001 002-A       4         2     536
    ## 257803 3260801906        1 320580001 002-A       4         2     536
    ## 257804 3260801906        1 320580001 002-A       4         2     536
    ## 257805 3260801906        1 320580001 002-A       4         2     536
    ##            upm factor clase_hog sexo_jefe edad_jefe educa_jefe tot_integ
    ## 257800 0079010    198   Nuclear    Hombre        67         09         3
    ## 257801 0079010    198   Extenso    Hombre        62         05         5
    ## 257802 0079010    198   Extenso    Hombre        62         05         5
    ## 257803 0079010    198   Extenso    Hombre        62         05         5
    ## 257804 0079010    198   Extenso    Hombre        62         05         5
    ## 257805 0079010    198   Extenso    Hombre        62         05         5
    ##        hombres mujeres mayores menores p12_64 p65mas ocupados percep_ing
    ## 257800       1       2       2       1      1      1        0          1
    ## 257801       2       3       4       1      4      0        3          3
    ## 257802       2       3       4       1      4      0        3          3
    ## 257803       2       3       4       1      4      0        3          3
    ## 257804       2       3       4       1      4      0        3          3
    ## 257805       2       3       4       1      4      0        3          3
    ##        perc_ocupa  ing_cor  ingtrab trabajo sueldos horas_extr comisiones
    ## 257800          0 23385.60     0.00    0.00    0.00          0          0
    ## 257801          3 15888.51 11533.68 7522.82 7522.82          0          0
    ## 257802          3 15888.51 11533.68 7522.82 7522.82          0          0
    ## 257803          3 15888.51 11533.68 7522.82 7522.82          0          0
    ## 257804          3 15888.51 11533.68 7522.82 7522.82          0          0
    ## 257805          3 15888.51 11533.68 7522.82 7522.82          0          0
    ##        aguinaldo indemtrab otra_rem remu_espec negocio noagrop industria
    ## 257800         0         0        0          0       0       0         0
    ## 257801         0         0        0          0       0       0         0
    ## 257802         0         0        0          0       0       0         0
    ## 257803         0         0        0          0       0       0         0
    ## 257804         0         0        0          0       0       0         0
    ## 257805         0         0        0          0       0       0         0
    ##        comercio servicios agrope agricolas pecuarios reproducc pesca
    ## 257800        0         0      0         0         0         0     0
    ## 257801        0         0      0         0         0         0     0
    ## 257802        0         0      0         0         0         0     0
    ## 257803        0         0      0         0         0         0     0
    ## 257804        0         0      0         0         0         0     0
    ## 257805        0         0      0         0         0         0     0
    ##        otros_trab rentas utilidad arrenda transfer jubilacion becas
    ## 257800       0.00      0        0       0 21933.99   17608.69     0
    ## 257801    4010.86      0        0       0     0.00       0.00     0
    ## 257802    4010.86      0        0       0     0.00       0.00     0
    ## 257803    4010.86      0        0       0     0.00       0.00     0
    ## 257804    4010.86      0        0       0     0.00       0.00     0
    ## 257805    4010.86      0        0       0     0.00       0.00     0
    ##        donativos remesas bene_gob transf_hog trans_inst estim_alqu
    ## 257800         0       0  1614.13    1928.57      782.6    1451.61
    ## 257801         0       0     0.00       0.00        0.0    4354.83
    ## 257802         0       0     0.00       0.00        0.0    4354.83
    ## 257803         0       0     0.00       0.00        0.0    4354.83
    ## 257804         0       0     0.00       0.00        0.0    4354.83
    ## 257805         0       0     0.00       0.00        0.0    4354.83
    ##        otros_ing gasto_mon alimentos ali_dentro cereales  carnes pescado
    ## 257800         0  28360.73   6454.20    6454.20  1671.41  899.99       0
    ## 257801         0  12278.79   5965.66    5322.81  1928.55 1928.57       0
    ## 257802         0  12278.79   5965.66    5322.81  1928.55 1928.57       0
    ## 257803         0  12278.79   5965.66    5322.81  1928.55 1928.57       0
    ## 257804         0  12278.79   5965.66    5322.81  1928.55 1928.57       0
    ## 257805         0  12278.79   5965.66    5322.81  1928.55 1928.57       0
    ##          leche  huevo aceites tuberculo verduras frutas azucar cafe
    ## 257800 1002.84 668.57       0    231.42  1041.41      0      0    0
    ## 257801  334.28   0.00       0    154.28   359.99      0      0    0
    ## 257802  334.28   0.00       0    154.28   359.99      0      0    0
    ## 257803  334.28   0.00       0    154.28   359.99      0      0    0
    ## 257804  334.28   0.00       0    154.28   359.99      0      0    0
    ## 257805  334.28   0.00       0    154.28   359.99      0      0    0
    ##        especias otros_alim bebidas ali_fuera tabaco vesti_calz vestido
    ## 257800        0          0  938.56      0.00      0       0.00    0.00
    ## 257801        0          0  617.14    642.85      0     313.04  117.39
    ## 257802        0          0  617.14    642.85      0     313.04  117.39
    ## 257803        0          0  617.14    642.85      0     313.04  117.39
    ## 257804        0          0  617.14    642.85      0     313.04  117.39
    ## 257805        0          0  617.14    642.85      0     313.04  117.39
    ##        calzado vivienda alquiler pred_cons agua energia limpieza cuidados
    ## 257800    0.00   905.32        0        75  210  620.32  5548.09   412.23
    ## 257801  195.65  1395.96        0         0  300 1095.96   307.71   307.71
    ## 257802  195.65  1395.96        0         0  300 1095.96   307.71   307.71
    ## 257803  195.65  1395.96        0         0  300 1095.96   307.71   307.71
    ## 257804  195.65  1395.96        0         0  300 1095.96   307.71   307.71
    ## 257805  195.65  1395.96        0         0  300 1095.96   307.71   307.71
    ##        utensilios enseres  salud atenc_ambu hospital medicinas transporte
    ## 257800          0 5135.86 518.47     518.47        0         0    5681.05
    ## 257801          0    0.00 195.65     195.65        0         0    2651.61
    ## 257802          0    0.00 195.65     195.65        0         0    2651.61
    ## 257803          0    0.00 195.65     195.65        0         0    2651.61
    ## 257804          0    0.00 195.65     195.65        0         0    2651.61
    ## 257805          0    0.00 195.65     195.65        0         0    2651.61
    ##        publico foraneo adqui_vehi mantenim refaccion  combus comunica
    ## 257800       0 2445.65          0  2035.40    293.47 1741.93     1200
    ## 257801       0    0.00          0  1451.61      0.00 1451.61     1200
    ## 257802       0    0.00          0  1451.61      0.00 1451.61     1200
    ## 257803       0    0.00          0  1451.61      0.00 1451.61     1200
    ## 257804       0    0.00          0  1451.61      0.00 1451.61     1200
    ## 257805       0    0.00          0  1451.61      0.00 1451.61     1200
    ##        educa_espa educacion esparci paq_turist personales cuida_pers
    ## 257800    8469.43   2044.87     555    5869.56     784.17     534.17
    ## 257801     555.00      0.00     555       0.00     894.16     894.16
    ## 257802     555.00      0.00     555       0.00     894.16     894.16
    ## 257803     555.00      0.00     555       0.00     894.16     894.16
    ## 257804     555.00      0.00     555       0.00     894.16     894.16
    ## 257805     555.00      0.00     555       0.00     894.16     894.16
    ##        acces_pers otros_gas transf_gas percep_tot retiro_inv prestamos
    ## 257800          0       250          0    1542.85          0      0.00
    ## 257801          0         0          0    2005.42          0   2005.42
    ## 257802          0         0          0    2005.42          0   2005.42
    ## 257803          0         0          0    2005.42          0   2005.42
    ## 257804          0         0          0    2005.42          0   2005.42
    ## 257805          0         0          0    2005.42          0   2005.42
    ##        otras_perc ero_nm_viv ero_nm_hog erogac_tot cuota_viv mater_serv
    ## 257800          0          0    1542.85     978.26         0          0
    ## 257801          0          0       0.00    1132.82         0          0
    ## 257802          0          0       0.00    1132.82         0          0
    ## 257803          0          0       0.00    1132.82         0          0
    ## 257804          0          0       0.00    1132.82         0          0
    ## 257805          0          0       0.00    1132.82         0          0
    ##        material servicio deposito prest_terc pago_tarje deudas balance
    ## 257800        0        0        0          0     978.26      0    0.00
    ## 257801        0        0        0          0       0.00      0 1132.82
    ## 257802        0        0        0          0       0.00      0 1132.82
    ## 257803        0        0        0          0       0.00      0 1132.82
    ## 257804        0        0        0          0       0.00      0 1132.82
    ## 257805        0        0        0          0       0.00      0 1132.82
    ##        otras_erog    smg numren parentesco sexo edad madre_hog madre_id
    ## 257800          0 6573.6     03        301    2   10         2     <NA>
    ## 257801          0 6573.6     01        101    1   62         2     <NA>
    ## 257802          0 6573.6     02        201    2   42         2     <NA>
    ## 257803          0 6573.6     03        303    2   24         1        &
    ## 257804          0 6573.6     04        303    1   19         1       02
    ## 257805          0 6573.6     05        609    2    3         1       03
    ##        padre_hog padre_id disc1 disc2 disc3 disc4 disc5 disc6 disc7 causa1
    ## 257800         1       01     8  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>
    ## 257801         2     <NA>     8  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>
    ## 257802         2     <NA>     8  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>
    ## 257803         2     <NA>     8  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>
    ## 257804         2     <NA>     8  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>
    ## 257805         2     <NA>     8  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>
    ##        causa2 causa3 causa4 causa5 causa6 causa7 hablaind lenguaind
    ## 257800   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>        2      <NA>
    ## 257801   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>        2      <NA>
    ## 257802   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>        2      <NA>
    ## 257803   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>        2      <NA>
    ## 257804   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>        2      <NA>
    ## 257805   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>        2      <NA>
    ##        hablaesp comprenind etnia alfabetism asis_esc nivel grado tipoesc
    ## 257800     <NA>          2     2          1        1    06     5       1
    ## 257801     <NA>          2     2          1        2  <NA>  <NA>    <NA>
    ## 257802     <NA>          2     2          1        2  <NA>  <NA>    <NA>
    ## 257803     <NA>          2     2          1        2  <NA>  <NA>    <NA>
    ## 257804     <NA>          2     2          1        2  <NA>  <NA>    <NA>
    ## 257805     <NA>          2     2          2        1    01     1       1
    ##        tiene_b otorg_b forma_b tiene_c otorg_c forma_c nivelaprob
    ## 257800       2    <NA>    <NA>    <NA>    <NA>    <NA>          2
    ## 257801    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>          3
    ## 257802    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>          3
    ## 257803    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>          2
    ## 257804    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>          4
    ## 257805       2    <NA>    <NA>    <NA>    <NA>    <NA>          0
    ##        gradoaprob antec_esc residencia edo_conyug pareja_hog conyuge_id
    ## 257800          4      <NA>         32       <NA>       <NA>       <NA>
    ## 257801          2      <NA>         32          2          1         02
    ## 257802          3      <NA>         32          2          1         01
    ## 257803          3      <NA>         32          6       <NA>       <NA>
    ## 257804          3      <NA>         32          6       <NA>       <NA>
    ## 257805          0      <NA>       <NA>       <NA>       <NA>       <NA>
    ##        segsoc ss_aa ss_mm redsoc_1 redsoc_2 redsoc_3 redsoc_4 redsoc_5
    ## 257800   <NA>    NA    NA     <NA>     <NA>     <NA>     <NA>     <NA>
    ## 257801      2    NA    NA        2        3        2        3        2
    ## 257802      2    NA    NA        2        3        2        2        3
    ## 257803      2    NA    NA        2        3        2        3        3
    ## 257804      2    NA    NA     <NA>     <NA>     <NA>     <NA>     <NA>
    ## 257805   <NA>    NA    NA     <NA>     <NA>     <NA>     <NA>     <NA>
    ##        redsoc_6 hor_1 min_1 usotiempo1 hor_2 min_2 usotiempo2 hor_3 min_3
    ## 257800     <NA>    NA    NA       <NA>    NA    NA       <NA>    NA    NA
    ## 257801        2    30     0       <NA>    NA    NA          9    NA    NA
    ## 257802        2     7     0       <NA>    NA    NA          9    NA    NA
    ## 257803        3    NA    NA          9    NA    NA          9    NA    NA
    ## 257804     <NA>    NA    NA       <NA>    NA    NA       <NA>    NA    NA
    ## 257805     <NA>    NA    NA       <NA>    NA    NA       <NA>    NA    NA
    ##        usotiempo3 hor_4 min_4 usotiempo4 hor_5 min_5 usotiempo5 hor_6
    ## 257800       <NA>    NA    NA       <NA>    NA    NA       <NA>    NA
    ## 257801          9    NA    NA          9    NA    NA          9    NA
    ## 257802          9    NA    NA          9    NA    NA          9    20
    ## 257803          9    NA    NA          9    NA    NA          9    20
    ## 257804       <NA>    NA    NA       <NA>    NA    NA       <NA>    NA
    ## 257805       <NA>    NA    NA       <NA>    NA    NA       <NA>    NA
    ##        min_6 usotiempo6 hor_7 min_7 usotiempo7 hor_8 min_8 usotiempo8
    ## 257800    NA       <NA>    NA    NA       <NA>    NA  <NA>       <NA>
    ## 257801    NA          9    NA    NA          9    20    00       <NA>
    ## 257802     0       <NA>    NA    NA          9    45    00       <NA>
    ## 257803     0       <NA>    NA    NA          9    50    00       <NA>
    ## 257804    NA       <NA>    NA    NA       <NA>    NA  <NA>       <NA>
    ## 257805    NA       <NA>    NA    NA       <NA>    NA  <NA>       <NA>
    ##        segpop atemed inst_1 inst_2 inst_3 inst_4 inst_5 inst_6 inscr_1
    ## 257800      1      1   <NA>   <NA>      3   <NA>   <NA>   <NA>    <NA>
    ## 257801      1      2   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>    <NA>
    ## 257802      1      2   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>    <NA>
    ## 257803      1      2   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>    <NA>
    ## 257804      1      2   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>    <NA>
    ## 257805      1      2   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>    <NA>
    ##        inscr_2 inscr_3 inscr_4 inscr_5 inscr_6 inscr_7 inscr_8 prob_anio
    ## 257800    <NA>       3    <NA>    <NA>    <NA>    <NA>    <NA>      <NA>
    ## 257801    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>      <NA>
    ## 257802    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>      2014
    ## 257803    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>      <NA>
    ## 257804    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>      <NA>
    ## 257805    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>      <NA>
    ##        prob_mes prob_sal aten_sal servmed_1 servmed_2 servmed_3 servmed_4
    ## 257800     <NA>     <NA>     <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257801     <NA>     <NA>     <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257802       05        1        1      <NA>        02      <NA>      <NA>
    ## 257803     <NA>     <NA>     <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257804     <NA>     <NA>     <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257805     <NA>     <NA>     <NA>      <NA>      <NA>      <NA>      <NA>
    ##        servmed_5 servmed_6 servmed_7 servmed_8 servmed_9 servmed_10
    ## 257800      <NA>      <NA>      <NA>      <NA>      <NA>       <NA>
    ## 257801      <NA>      <NA>      <NA>      <NA>      <NA>       <NA>
    ## 257802      <NA>      <NA>      <NA>      <NA>      <NA>       <NA>
    ## 257803      <NA>      <NA>      <NA>      <NA>      <NA>       <NA>
    ## 257804      <NA>      <NA>      <NA>      <NA>      <NA>       <NA>
    ## 257805      <NA>      <NA>      <NA>      <NA>      <NA>       <NA>
    ##        servmed_11 hh_lug mm_lug hh_esp mm_esp pagoaten_1 pagoaten_2
    ## 257800       <NA>     NA     NA     NA     NA       <NA>       <NA>
    ## 257801       <NA>     NA     NA     NA     NA       <NA>       <NA>
    ## 257802       <NA>      0     45      0     15       <NA>       <NA>
    ## 257803       <NA>     NA     NA     NA     NA       <NA>       <NA>
    ## 257804       <NA>     NA     NA     NA     NA       <NA>       <NA>
    ## 257805       <NA>     NA     NA     NA     NA       <NA>       <NA>
    ##        pagoaten_3 pagoaten_4 pagoaten_5 pagoaten_6 pagoaten_7 noatenc_1
    ## 257800       <NA>       <NA>       <NA>       <NA>       <NA>      <NA>
    ## 257801       <NA>       <NA>       <NA>       <NA>       <NA>      <NA>
    ## 257802       <NA>       <NA>       <NA>       <NA>          7      <NA>
    ## 257803       <NA>       <NA>       <NA>       <NA>       <NA>      <NA>
    ## 257804       <NA>       <NA>       <NA>       <NA>       <NA>      <NA>
    ## 257805       <NA>       <NA>       <NA>       <NA>       <NA>      <NA>
    ##        noatenc_2 noatenc_3 noatenc_4 noatenc_5 noatenc_6 noatenc_7
    ## 257800      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257801      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257802      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257803      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257804      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257805      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ##        noatenc_8 noatenc_9 noatenc_10 noatenc_11 noatenc_12 noatenc_13
    ## 257800      <NA>      <NA>       <NA>       <NA>       <NA>       <NA>
    ## 257801      <NA>      <NA>       <NA>       <NA>       <NA>       <NA>
    ## 257802      <NA>      <NA>       <NA>       <NA>       <NA>       <NA>
    ## 257803      <NA>      <NA>       <NA>       <NA>       <NA>       <NA>
    ## 257804      <NA>      <NA>       <NA>       <NA>       <NA>       <NA>
    ## 257805      <NA>      <NA>       <NA>       <NA>       <NA>       <NA>
    ##        noatenc_14 noatenc_15 noatenc_16 norecib_1 norecib_2 norecib_3
    ## 257800       <NA>       <NA>       <NA>      <NA>      <NA>      <NA>
    ## 257801       <NA>       <NA>       <NA>      <NA>      <NA>      <NA>
    ## 257802       <NA>       <NA>       <NA>      <NA>      <NA>      <NA>
    ## 257803       <NA>       <NA>       <NA>      <NA>      <NA>      <NA>
    ## 257804       <NA>       <NA>       <NA>      <NA>      <NA>      <NA>
    ## 257805       <NA>       <NA>       <NA>      <NA>      <NA>      <NA>
    ##        norecib_4 norecib_5 norecib_6 norecib_7 norecib_8 norecib_9
    ## 257800      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257801      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257802      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257803      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257804      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ## 257805      <NA>      <NA>      <NA>      <NA>      <NA>      <NA>
    ##        norecib_10 norecib_11 razon_1 razon_2 razon_3 razon_4 razon_5
    ## 257800       <NA>       <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ## 257801       <NA>       <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ## 257802       <NA>       <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ## 257803       <NA>       <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ## 257804       <NA>       <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ## 257805       <NA>       <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##        razon_6 razon_7 razon_8 razon_9 razon_10 razon_11 diabetes
    ## 257800    <NA>    <NA>    <NA>    <NA>     <NA>     <NA>     <NA>
    ## 257801    <NA>    <NA>    <NA>    <NA>     <NA>     <NA>        2
    ## 257802    <NA>    <NA>    <NA>    <NA>     <NA>     <NA>        1
    ## 257803    <NA>    <NA>    <NA>    <NA>     <NA>     <NA>        1
    ## 257804    <NA>    <NA>    <NA>    <NA>     <NA>     <NA>        2
    ## 257805    <NA>    <NA>    <NA>    <NA>     <NA>     <NA>     <NA>
    ##        pres_alta peso segvol_1 segvol_2 segvol_3 segvol_4 segvol_5
    ## 257800      <NA>    2     <NA>     <NA>     <NA>     <NA>     <NA>
    ## 257801         2    2     <NA>     <NA>     <NA>     <NA>     <NA>
    ## 257802         1    1     <NA>     <NA>     <NA>     <NA>     <NA>
    ## 257803         1    1     <NA>     <NA>     <NA>     <NA>     <NA>
    ## 257804         2    2     <NA>     <NA>     <NA>     <NA>     <NA>
    ## 257805      <NA>    1     <NA>     <NA>     <NA>     <NA>     <NA>
    ##        segvol_6 segvol_7 hijos_viv hijos_mue hijos_sob trabajo_mp
    ## 257800     <NA>     <NA>        NA        NA        NA       <NA>
    ## 257801        6     <NA>        NA        NA        NA          1
    ## 257802        6     <NA>         3         0         3          1
    ## 257803        6     <NA>         1         0         1          2
    ## 257804        6     <NA>        NA        NA        NA          1
    ## 257805     <NA>     <NA>        NA        NA        NA       <NA>
    ##        motivo_aus act_pnea1 act_pnea2 num_trabaj
    ## 257800       <NA>      <NA>      <NA>       <NA>
    ## 257801       <NA>      <NA>      <NA>          1
    ## 257802       <NA>      <NA>      <NA>          1
    ## 257803       <NA>         3      <NA>       <NA>
    ## 257804       <NA>      <NA>      <NA>          2
    ## 257805       <NA>      <NA>      <NA>       <NA>

Hagamos un tabulado, por ejemplo cómo se distribuye la población según el hogar del jefe

``` r
table(merge_data2$sexo, merge_data2$sexo_jefe)
```

    ##    
    ##     Hombre  Mujer
    ##   1 104352  21613
    ##   2  94954  36886

Para visualizar mejor esta tabla, podemos agregar el comando "addmargins", con esto nos sumariza. Como R está más desarrollado para hacer matrices, no nos da los totales, pero siempre los podemos agregar.

``` r
addmargins(table(merge_data2$sexo, merge_data2$sexo_jefe))
```

    ##      
    ##       Hombre  Mujer    Sum
    ##   1   104352  21613 125965
    ##   2    94954  36886 131840
    ##   Sum 199306  58499 257805

Bases de distinto tamaño
========================

Hasta ahorita hemos hecho merge que son de unidades de distinto nivel y son incluyentes. A veces tenemos bases de datos que son de distinto tamaño y del mismo nivel. A veces las dos aportan casos y a veces aportan variables, y a veces, las dos aportan las dos cosas.

Hay secciones de la enigh que no corresponden a toda la población. Por ejemplo, la sección de trabajos sólo la llenan los mayores de 12 años, en la ENIGH. Vamos a pegar esta base a la base de población.

Cargamos la base de trabajos.

Sólo nos vamos a quedar con el trabajo principal, para que sea un renglón por persona. Porque en esta base hay información de más de un trabajo.

``` r
enigh_trabajo_total <- read.dbf("trabajos.dbf")
enigh_trabajo <- enigh_trabajo_total[enigh_trabajo_total$id_trabajo==1,]
dim(enigh_trabajo)
```

    ## [1] 120245     55

Vamos a hacer el primer tipo de merge

``` r
merge_data3<-merge(enigh_pobla,enigh_trabajo, by=c("folioviv", "foliohog", "numren"))
dim(merge_data3)
```

    ## [1] 120245    230

¡La base nueva no tiene a toda la población, solo la que tiene en la base más pequeña! Tenemos sólo 120,245 personas.

En realidad hay tres formas de hacer un merge

<b>A. Casos comunes en las dos bases</b>

Por <i>default</i>, el comando tiene activado la opción "all = TRUE", que nos deja los datos comunes a ambas bases.

``` r
merge_data3<-merge(enigh_pobla,enigh_trabajo, by=c("folioviv", "foliohog", "numren"), all = TRUE)
```

<b>B. Casos de la base 1</b>

Si queremos quedarnos con todos los datos que hay en la primera base, x, vamos a usar a opción all.x = TRUE.

``` r
merge_data3<-merge(enigh_pobla,enigh_trabajo, by=c("folioviv", "foliohog", "numren"), all.x  = TRUE)
dim(merge_data3)
```

    ## [1] 257805    230

<b>C. Casos de la base 2</b>

Notamos que hoy sí tenemos los datos de toda la población y hay missings en las variables aportadas por la base de trabajo

Si queremos lo contrario, quedarnos con los datos aportados por la segunda base, y, vamos a usar la opción all.y=TRUE En este caso, como la base de trabajos es un subconjunto, es igual a lo que hicimos en el caso A.

``` r
merge_data3<-merge(enigh_pobla,enigh_trabajo, by=c("folioviv", "foliohog", "numren"), all.y  = TRUE)
dim(merge_data3)
```

    ## [1] 120245    230
