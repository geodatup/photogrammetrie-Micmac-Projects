# Réalisation d'un modèle 3D et orthophoto de Rouyre avec QX1

La prise de vue a été réalisé à partir d'un drone Iris + équipé d'un Apparreil QX1, sur nacelle fixe ortientée en nadir.

Le plan de vol est réalisé avec Mission planner.

La superificie parcourue est d'environ 1ha.

Altitude de prise de vue : 50 m

Temps de la prise de vue : 3 minutes
Vent : environs 5 kt


le QX1 prend des photos tous les 14 m environs (parametrage effectuer avec mission planner.

Nombre de photo final : 65

Format fichier photo : .ARW (raw sony)

Description QX1 : 
APS-C 
objectif : 16mm (24 équivalent 35mm)
Nb pixel : 5504 3656
taille de pixel
poids fichier : 21/22mo

### Definir les paterns
~~~
PAT_1="(.*.JPG)"
PAT_2="(.*.ARW)"
PAT_3="(.*.(ARW|JPG))"
~~~

### Tapioca
~~~
mm3d Tapioca MulScale "$PAT" 300 1200 @SFS
~~~
Analyse des résultats : 

~~~
IMAGE1=""
IMAGE2=""

mm3d SEL ./ $IMAGE1 $IMAGE2 KH=NB
~~~


### Tapas

Définition de qx1 dans un nouveau fichier MicMac-LocalChantierDescripteur.xml localisé à la racine du projet.

~~~
<Global>
  <ChantierDescripteur >

 <!-- Define a camera model (name and sensor/film size) -->
    <LocCamDataBase>
   <CameraEntry>
         <Name>  ILCE-QX1 </Name>
   		 <SzCaptMm>  15.40 23.20 </SzCaptMm>
   		 <ShortName> QX1 </ShortName>
   </CameraEntry>

   <CameraEntry>
         <Name>  Canon EOS 700D </Name>
   		 <SzCaptMm>   22.3 14.9  </SzCaptMm>
   		 <ShortName> C700D  </ShortName>
   </CameraEntry>

    </LocCamDataBase>

	<KeyedNamesAssociations>
	<Calcs>
	<Arrite> 1 1 </Arrite>
	<Direct>
	<PatternTransform> .*.JPG </PatternTransform>
	<CalcName> 80.0 </CalcName>
	</Direct>
	</Calcs>
	<Calcs>
	<Arrite> 1 1 </Arrite>
	<Direct>
	<PatternTransform> .*.CR2 </PatternTransform>
	<CalcName> 50.0 </CalcName>
	</Direct>
	</Calcs>
	<Calcs>
	<Arrite> 1 1 </Arrite>
	<Direct>
	<PatternTransform> .*.ARW </PatternTransform>
	<CalcName> 24.0 </CalcName>
	</Direct>
	</Calcs>
	
	<Calcs>
	<Arrite> 1 1 </Arrite>
	<Direct>
	<PatternTransform> .* </PatternTransform>
	<CalcName>0</CalcName>
	</Direct>
	</Calcs>



<Key> NKS-Assoc-STD-FOC </Key>
</KeyedNamesAssociations>

  </ChantierDescripteur>
</Global>
~~~

<<<<<<< HEAD
Calibration interne des 3 sets d'images (car 3 caméras) séparement sur des jeux d'images "calibrables"

~~~
PAT_cal1="(IMG_4(789|799|800|801|802).CR2)"
PAT_cal2="(IMG_05(35|33|31|29|47).JPG)"
PAT_cal3="(DSC005(27|28|47|48).ARW)"
TAPAS_MDL="RadialExtended"
TAPAS_MDL="FraserBasic"
TAPAS_MDL="AutoCal"
~~~

=======
Calibrer les 3 sets d'images (car 3 caméras)
~~~	
Tapas RadialExtended "IMG_4(789|799|800|801|802).CR2" Out=iniCalib
Tapas RadialExtended "IMG_05(35|33|31|29|47).JPG" Out=iniCalib
Tapas RadialExtended "DSC005(27|28|47|48).ARW" Out=iniCalib

~~~

puis on fait l'orientation relative sur toutes les images

~~~
mm3d Tapas AutoCal ".*.(ARW|CR2|JPG)" InOri=iniCalib Out=Allori
~~~

Résultat : il semblerait que la calbration interne de la camera 700D soit mal effectuée (manque d'images d'une même scène pour effectuer la calibration)

temps de traitement : 5839.17 s



### AperiCloud
>>>>>>> origin/master

~~~	
mm3d Tapas $TAPAS_MDL $PAT_cal Out=iniCalib
~~~
<<<<<<< Updated upstream
mm3d AperiCloud ".*.(ARW|CR2|JPG)" Allori
~~~

temps de traitement : 13846 sec
=======

Resultats RadialExtended ARW :
~~~
<<<<<<< HEAD
RES:[DSC00527.ARW][C] ER2 1.17755 Nn 97.9798 Of 1386 Mul 418 Mul-NN 408 Time 0.229367
RES:[DSC00528.ARW][C] ER2 1.04642 Nn 98.4172 Of 1769 Mul 362 Mul-NN 353 Time 0.305856
RES:[DSC00547.ARW][C] ER2 1.06871 Nn 98.3598 Of 1951 Mul 446 Mul-NN 437 Time 0.329376
RES:[DSC00548.ARW][C] ER2 1.05883 Nn 98.9592 Of 1153 Mul 172 Mul-NN 170 Time 0.18804
| |  Residual = 1.08914 ;; Evol, Moy=3.59647e-12 ,Max=7.02768e-11
| |  Worst, Res 1.17755 for DSC00527.ARW,  Perc 97.9798 for DSC00527.ARW
| |  Cond , Aver 11.7131 Max 73.1346 Prop>100 0
BIGTIF suspended momentally 
--- End Iter 10 STEP 3
~~~

Resultats FraserBasic ARW :
~~~
RES:[DSC00527.ARW][C] ER2 1.18183 Nn 97.9798 Of 1386 Mul 418 Mul-NN 409 Time 0.283449
RES:[DSC00528.ARW][C] ER2 1.04974 Nn 98.4737 Of 1769 Mul 362 Mul-NN 354 Time 0.37717
RES:[DSC00547.ARW][C] ER2 1.06739 Nn 98.3598 Of 1951 Mul 446 Mul-NN 437 Time 0.414536
RES:[DSC00548.ARW][C] ER2 1.06235 Nn 98.8725 Of 1153 Mul 172 Mul-NN 170 Time 0.238723
| |  Residual = 1.09163 ;; Evol, Moy=3.64595e-13 ,Max=1.03179e-12
| |  Worst, Res 1.18183 for DSC00527.ARW,  Perc 97.9798 for DSC00527.ARW
| |  Cond , Aver 11.476 Max 73.1346 Prop>100 0
BIGTIF suspended momentally 
--- End Iter 3 STEP 3
~~~


si residus mauvais changer le modèle de calibration de la caméra. Par ex :
~~~
mm3d Tapas FraserBasic "IMG_05(35|33|31|29|47).JPG" Out=iniCalib
~~~
Resultats FraserBasic:

~~~
RES:[IMG_0529.JPG][C] ER2 0.92262 Nn 99.1018 Of 2004 Mul 561 Mul-NN 551 Time 0.40892
RES:[IMG_0531.JPG][C] ER2 1.03241 Nn 98.342 Of 2111 Mul 802 Mul-NN 784 Time 0.414552
RES:[IMG_0533.JPG][C] ER2 1.06906 Nn 97.8821 Of 2408 Mul 731 Mul-NN 710 Time 0.462782
RES:[IMG_0535.JPG][C] ER2 0.953304 Nn 97.9452 Of 1460 Mul 102 Mul-NN 98 Time 0.26877
RES:[IMG_0547.JPG][C] ER2 0.917622 Nn 99.0741 Of 1404 Mul 402 Mul-NN 397 Time 0.289319
| |  Residual = 0.980899 ;; Evol, Moy=1.86962e-12 ,Max=6.00816e-11
| |  Worst, Res 1.06906 for IMG_0533.JPG,  Perc 97.8821 for IMG_0533.JPG
| |  Cond , Aver 23.1498 Max 3.47773e+06 Prop>100 4.64163e-06
BIGTIF suspended momentally 
--- End Iter 3 STEP 3
~~~
Resultats RadialExtended:
~~~
RES:[IMG_0529.JPG][C] ER2 0.923293 Nn 99.1018 Of 2004 Mul 561 Mul-NN 551 Time 0.362292
RES:[IMG_0531.JPG][C] ER2 1.03309 Nn 98.342 Of 2111 Mul 802 Mul-NN 784 Time 0.370829
RES:[IMG_0533.JPG][C] ER2 1.06962 Nn 97.8821 Of 2408 Mul 731 Mul-NN 710 Time 0.390332
RES:[IMG_0535.JPG][C] ER2 0.953964 Nn 97.9452 Of 1460 Mul 102 Mul-NN 98 Time 0.224528
RES:[IMG_0547.JPG][C] ER2 0.91833 Nn 99.0741 Of 1404 Mul 402 Mul-NN 397 Time 0.249081
| |  Residual = 0.981549 ;; Evol, Moy=1.67674e-05 ,Max=3.38484e-05
| |  Worst, Res 1.06962 for IMG_0533.JPG,  Perc 97.8821 for IMG_0533.JPG
| |  Cond , Aver 20.9344 Max 3.47773e+06 Prop>100 3.96079e-06
BIGTIF suspended momentally 
--- End Iter 10 STEP 3

~~~



Calculer l'orientation des images sur le premier bloc 

~~~
mm3d Tapas AutoCal ".*.ARW" InCal=iniCalib InOri=iniCalib Out=AlloriARW
~~~

Résultat : 

~~~
RES:[DSC00527.ARW][C] ER2 1.56286 Nn 96.4057 Of 2031 Mul 1275 Mul-NN 1221 Time 0.55417
RES:[DSC00528.ARW][C] ER2 1.55603 Nn 98.2763 Of 4351 Mul 2957 Mul-NN 2907 Time 1.17483
RES:[DSC00547.ARW][C] ER2 1.51695 Nn 97.9296 Of 2898 Mul 1950 Mul-NN 1910 Time 0.839527
RES:[DSC00548.ARW][C] ER2 1.64434 Nn 97.524 Of 2504 Mul 1242 Mul-NN 1208 Time 0.628638
RES:[DSC00549.ARW][C] ER2 1.5551 Nn 97.8638 Of 1498 Mul 517 Mul-NN 503 Time 0.316881
RES:[DSC00550.ARW][C] ER2 1.54673 Nn 98.1809 Of 4178 Mul 2877 Mul-NN 2823 Time 1.09738
RES:[DSC00551.ARW][C] ER2 1.54963 Nn 98.4563 Of 4405 Mul 2944 Mul-NN 2894 Time 1.11524
RES:[DSC00552.ARW][C] ER2 1.56146 Nn 98.0833 Of 3600 Mul 2327 Mul-NN 2278 Time 0.924975
RES:[DSC00553.ARW][C] ER2 1.5466 Nn 98.121 Of 4364 Mul 2847 Mul-NN 2792 Time 1.22324
RES:[DSC00554.ARW][C] ER2 1.55308 Nn 98.0297 Of 4111 Mul 2772 Mul-NN 2716 Time 1.19467
RES:[DSC00555.ARW][C] ER2 1.63196 Nn 98.029 Of 4972 Mul 3669 Mul-NN 3597 Time 1.54628
RES:[DSC00556.ARW][C] ER2 1.7238 Nn 97.6825 Of 4919 Mul 3745 Mul-NN 3658 Time 1.61501
RES:[DSC00557.ARW][C] ER2 1.69447 Nn 97.9653 Of 5013 Mul 3986 Mul-NN 3912 Time 1.62365
RES:[DSC00558.ARW][C] ER2 1.58774 Nn 98.0113 Of 5330 Mul 4086 Mul-NN 4004 Time 1.68127
RES:[DSC00559.ARW][C] ER2 1.57183 Nn 98.1506 Of 5353 Mul 4129 Mul-NN 4054 Time 1.70567
RES:[DSC00560.ARW][C] ER2 1.58104 Nn 97.9139 Of 5225 Mul 3956 Mul-NN 3876 Time 1.59099
RES:[DSC00561.ARW][C] ER2 1.61901 Nn 97.5789 Of 4502 Mul 3527 Mul-NN 3444 Time 1.46265
RES:[DSC00562.ARW][C] ER2 1.45175 Nn 98.9456 Of 4173 Mul 2809 Mul-NN 2784 Time 1.22924
RES:[DSC00563.ARW][C] ER2 1.55955 Nn 97.9693 Of 3841 Mul 2790 Mul-NN 2741 Time 1.19855
RES:[DSC00564.ARW][C] ER2 1.61277 Nn 97.8158 Of 4670 Mul 3490 Mul-NN 3408 Time 1.43207
RES:[DSC00565.ARW][C] ER2 1.60306 Nn 98.0956 Of 2993 Mul 1874 Mul-NN 1843 Time 0.807125
RES:[DSC00566.ARW][C] ER2 1.65648 Nn 97.9535 Of 3616 Mul 2146 Mul-NN 2097 Time 0.936923
RES:[DSC00567.ARW][C] ER2 1.53322 Nn 98.3978 Of 3620 Mul 2207 Mul-NN 2172 Time 0.922487
RES:[DSC00568.ARW][C] ER2 1.60828 Nn 97.541 Of 3294 Mul 1994 Mul-NN 1944 Time 0.923645
RES:[DSC00569.ARW][C] ER2 1.49514 Nn 96.731 Of 1866 Mul 1076 Mul-NN 1042 Time 0.516535
RES:[DSC00570.ARW][C] ER2 1.59171 Nn 97.1839 Of 3622 Mul 2533 Mul-NN 2460 Time 1.05016
RES:[DSC00571.ARW][C] ER2 1.53136 Nn 97.1206 Of 2952 Mul 1877 Mul-NN 1820 Time 0.805426
RES:[DSC00572.ARW][C] ER2 1.53808 Nn 97.4549 Of 3772 Mul 2664 Mul-NN 2587 Time 1.10337
RES:[DSC00573.ARW][C] ER2 1.60297 Nn 96.8641 Of 3444 Mul 2442 Mul-NN 2364 Time 1.02919
RES:[DSC00574.ARW][C] ER2 1.49731 Nn 98.0086 Of 3967 Mul 2588 Mul-NN 2540 Time 1.11095
RES:[DSC00575.ARW][C] ER2 1.41482 Nn 97.1282 Of 1950 Mul 983 Mul-NN 955 Time 0.495326
RES:[DSC00576.ARW][C] ER2 1.60427 Nn 96.3736 Of 3116 Mul 1890 Mul-NN 1807 Time 0.853166
RES:[DSC00577.ARW][C] ER2 1.68178 Nn 91.9753 Of 486 Mul 227 Mul-NN 208 Time 0.109856
RES:[DSC00578.ARW][C] ER2 1.56507 Nn 98.1699 Of 3825 Mul 2728 Mul-NN 2687 Time 1.11327
RES:[DSC00581.ARW][C] ER2 1.64303 Nn 97.4399 Of 3203 Mul 2154 Mul-NN 2094 Time 0.949335
RES:[DSC00582.ARW][C] ER2 1.67667 Nn 97.0603 Of 4014 Mul 2854 Mul-NN 2771 Time 1.18786
RES:[DSC00583.ARW][C] ER2 1.578 Nn 97.896 Of 1711 Mul 1152 Mul-NN 1127 Time 0.454473
RES:[DSC00584.ARW][C] ER2 1.63046 Nn 96.9807 Of 4670 Mul 3182 Mul-NN 3081 Time 1.25745
RES:[DSC00585.ARW][C] ER2 1.62002 Nn 97.6303 Of 4642 Mul 3273 Mul-NN 3189 Time 1.26499
RES:[DSC00586.ARW][C] ER2 1.63648 Nn 97.2088 Of 3941 Mul 2838 Mul-NN 2763 Time 1.09428
RES:[DSC00587.ARW][C] ER2 1.5286 Nn 97.5062 Of 3609 Mul 2432 Mul-NN 2380 Time 0.991743
RES:[DSC00588.ARW][C] ER2 1.49888 Nn 97.2425 Of 3699 Mul 2524 Mul-NN 2460 Time 0.984663
RES:[DSC00589.ARW][C] ER2 1.4641 Nn 97.8106 Of 3517 Mul 2281 Mul-NN 2230 Time 0.933716
RES:[DSC00590.ARW][C] ER2 1.50358 Nn 97.745 Of 3725 Mul 2382 Mul-NN 2325 Time 1.02105
RES:[DSC00591.ARW][C] ER2 1.48968 Nn 97.5715 Of 3006 Mul 1962 Mul-NN 1914 Time 0.786128
RES:[DSC00592.ARW][C] ER2 1.44643 Nn 97.6744 Of 2322 Mul 1378 Mul-NN 1351 Time 0.614127
RES:[DSC00594.ARW][C] ER2 1.57258 Nn 95.9253 Of 589 Mul 341 Mul-NN 332 Time 0.154134
RES:[DSC00616.ARW][C] ER2 1.68207 Nn 96.2906 Of 647 Mul 385 Mul-NN 376 Time 0.171396
RES:[DSC00617.ARW][C] ER2 1.60994 Nn 96.8928 Of 2285 Mul 1425 Mul-NN 1380 Time 0.616201
RES:[DSC00618.ARW][C] ER2 1.65347 Nn 96.2234 Of 2542 Mul 1568 Mul-NN 1515 Time 0.686399
RES:[DSC00619.ARW][C] ER2 1.69979 Nn 94.4808 Of 3207 Mul 2113 Mul-NN 2004 Time 0.883533
RES:[DSC00620.ARW][C] ER2 1.7241 Nn 95.4435 Of 3292 Mul 2347 Mul-NN 2253 Time 0.953893
RES:[DSC00621.ARW][C] ER2 1.73259 Nn 96.9004 Of 3323 Mul 2396 Mul-NN 2331 Time 0.952965
RES:[DSC00622.ARW][C] ER2 1.76663 Nn 97.3793 Of 3625 Mul 2384 Mul-NN 2329 Time 1.07107
RES:[DSC00623.ARW][C] ER2 1.80463 Nn 97.6512 Of 4513 Mul 3171 Mul-NN 3107 Time 1.33585
RES:[DSC00624.ARW][C] ER2 1.64348 Nn 97.969 Of 4382 Mul 3129 Mul-NN 3072 Time 1.44336
RES:[DSC00625.ARW][C] ER2 1.6366 Nn 97.8979 Of 4662 Mul 3162 Mul-NN 3087 Time 1.41233
RES:[DSC00626.ARW][C] ER2 1.66017 Nn 97.3471 Of 4448 Mul 3119 Mul-NN 3033 Time 1.37057
RES:[DSC00627.ARW][C] ER2 1.56427 Nn 97.7248 Of 2769 Mul 1757 Mul-NN 1716 Time 0.756429
RES:[DSC00628.ARW][C] ER2 1.42624 Nn 98.0696 Of 3937 Mul 2478 Mul-NN 2424 Time 1.08431
RES:[DSC00629.ARW][C] ER2 1.42159 Nn 97.8684 Of 3753 Mul 2411 Mul-NN 2358 Time 0.97556
RES:[DSC00630.ARW][C] ER2 1.3252 Nn 97.9428 Of 3354 Mul 1942 Mul-NN 1902 Time 0.851563
RES:[DSC00631.ARW][C] ER2 1.4642 Nn 97.7643 Of 3623 Mul 2334 Mul-NN 2277 Time 0.949679
RES:[DSC00632.ARW][C] ER2 1.76597 Nn 97.3087 Of 3084 Mul 2075 Mul-NN 2012 Time 0.927931
RES:[DSC00633.ARW][C] ER2 1.69065 Nn 95.2418 Of 2564 Mul 1594 Mul-NN 1524 Time 0.770838
RES:[DSC00634.ARW][C] ER2 1.62572 Nn 94.7137 Of 1816 Mul 973 Mul-NN 922 Time 0.532317
RES:[DSC00635.ARW][C] ER2 1.74016 Nn 93.7718 Of 2585 Mul 1586 Mul-NN 1475 Time 0.753574
RES:[DSC00636.ARW][C] ER2 1.78114 Nn 93.9845 Of 2959 Mul 1822 Mul-NN 1688 Time 0.820673
RES:[DSC00637.ARW][C] ER2 1.9286 Nn 93.3124 Of 2871 Mul 2063 Mul-NN 1917 Time 0.872318
RES:[DSC00638.ARW][C] ER2 1.8445 Nn 93.7182 Of 3343 Mul 2264 Mul-NN 2097 Time 0.967811
RES:[DSC00639.ARW][C] ER2 1.87961 Nn 94.6123 Of 3211 Mul 2177 Mul-NN 2044 Time 0.93098
RES:[DSC00640.ARW][C] ER2 1.89012 Nn 95.5818 Of 2965 Mul 1850 Mul-NN 1785 Time 0.781359
RES:[DSC00641.ARW][C] ER2 1.90547 Nn 95.9207 Of 2623 Mul 1668 Mul-NN 1606 Time 0.691099
RES:[DSC00642.ARW][C] ER2 1.7484 Nn 96.096 Of 2792 Mul 1648 Mul-NN 1573 Time 0.703631
RES:[DSC00643.ARW][C] ER2 1.63118 Nn 97.6709 Of 3306 Mul 1776 Mul-NN 1727 Time 0.819755
RES:[DSC00644.ARW][C] ER2 1.51133 Nn 97.7407 Of 2877 Mul 1243 Mul-NN 1208 Time 0.725885
RES:[DSC00645.ARW][C] ER2 1.76477 Nn 96.0373 Of 858 Mul 473 Mul-NN 458 Time 0.213789
RES:[DSC00646.ARW][C] ER2 1.5716 Nn 96.1424 Of 3033 Mul 2233 Mul-NN 2141 Time 1.01384
RES:[DSC00647.ARW][C] ER2 1.55168 Nn 96.2216 Of 3123 Mul 2231 Mul-NN 2141 Time 1.02846
RES:[DSC00648.ARW][C] ER2 1.68555 Nn 95.7477 Of 2822 Mul 1972 Mul-NN 1897 Time 0.899754
RES:[DSC00649.ARW][C] ER2 1.65162 Nn 95.42 Of 3821 Mul 2993 Mul-NN 2856 Time 1.28143
RES:[DSC00650.ARW][C] ER2 1.62783 Nn 96.1612 Of 4194 Mul 3457 Mul-NN 3316 Time 1.48582
RES:[DSC00651.ARW][C] ER2 1.59588 Nn 96.1936 Of 4256 Mul 3457 Mul-NN 3317 Time 1.46042
RES:[DSC00652.ARW][C] ER2 1.6024 Nn 95.9188 Of 4288 Mul 3442 Mul-NN 3296 Time 1.42808
RES:[DSC00653.ARW][C] ER2 1.65073 Nn 95.8208 Of 3661 Mul 2946 Mul-NN 2818 Time 1.27166
RES:[DSC00654.ARW][C] ER2 1.71183 Nn 95.4358 Of 3637 Mul 2875 Mul-NN 2744 Time 1.23329
RES:[DSC00655.ARW][C] ER2 1.63281 Nn 95.028 Of 3037 Mul 2377 Mul-NN 2256 Time 1.02904
RES:[DSC00656.ARW][C] ER2 1.58785 Nn 95.3917 Of 3038 Mul 2363 Mul-NN 2258 Time 1.05851
RES:[DSC00657.ARW][C] ER2 1.4747 Nn 96.8868 Of 3180 Mul 1901 Mul-NN 1830 Time 0.904078
RES:[DSC00658.ARW][C] ER2 1.47167 Nn 95.942 Of 3105 Mul 1757 Mul-NN 1678 Time 0.829058
RES:[DSC00659.ARW][C] ER2 1.54458 Nn 95.6539 Of 2485 Mul 1734 Mul-NN 1662 Time 0.711343
RES:[DSC00660.ARW][C] ER2 1.4785 Nn 97.3977 Of 3151 Mul 1689 Mul-NN 1648 Time 0.859087
RES:[DSC00661.ARW][C] ER2 1.53864 Nn 96.824 Of 3432 Mul 1936 Mul-NN 1865 Time 0.99994
RES:[DSC00662.ARW][C] ER2 1.57938 Nn 96.549 Of 3796 Mul 2540 Mul-NN 2442 Time 1.15913
RES:[DSC00663.ARW][C] ER2 1.51989 Nn 96.6515 Of 3076 Mul 1801 Mul-NN 1736 Time 0.82103
RES:[DSC00664.ARW][C] ER2 1.51124 Nn 96.8971 Of 3674 Mul 2464 Mul-NN 2378 Time 1.0192
RES:[DSC00665.ARW][C] ER2 1.4147 Nn 97.4402 Of 3555 Mul 2255 Mul-NN 2192 Time 0.952664
RES:[DSC00666.ARW][C] ER2 1.46408 Nn 97.4208 Of 3916 Mul 2713 Mul-NN 2635 Time 1.04258
RES:[DSC00667.ARW][C] ER2 1.45182 Nn 97.7691 Of 3855 Mul 2543 Mul-NN 2482 Time 0.977702
RES:[DSC00668.ARW][C] ER2 1.41811 Nn 98.1808 Of 3738 Mul 2143 Mul-NN 2100 Time 0.891134
RES:[DSC00669.ARW][C] ER2 1.65295 Nn 97.1646 Of 1975 Mul 1090 Mul-NN 1063 Time 0.470779
RES:[DSC00670.ARW][C] ER2 1.66372 Nn 96.697 Of 878 Mul 508 Mul-NN 499 Time 0.247719
| |  Residual = 1.60613 ;; Evol, Moy=1.99848e-13 ,Max=1.52793e-08
| |  Worst, Res 1.9286 for DSC00637.ARW,  Perc 91.9753 for DSC00577.ARW
| |  Cond , Aver 10.836 Max 308.258 Prop>100 0.000241194
BIGTIF suspended momentally 
--- End Iter 10 STEP 3
~~~



Utiliser AperiCloud pour percevoir le résultat
~~~
mm3d AperiCloud ".*.ARW" AlloriARW
~~~

Calculer l'orientation des images sur le second bloc (si besoin) et le fusionner au premier

~~~
mm3d Tapas AutoCal ".*.(ARW|JPG)" InCal=iniCalib InOri=AlloriARW Out=AlloriARWJPG FrozenPoses=".*.ARW"
~~~

Utiliser AperiCloud pour percevoir le résultat (si besoin)
~~~
mm3d AperiCloud ".*.(ARW|JPG)" AlloriARWJPG
~~~

les résidus sont meilleurs mais j'ai toujours des nan au bout d'un moment ()

Calculer l'orientation des images sur le second  bloc uniquement 

exclude 0553
~~~
PAT_JPG="IMG_0[3-5]([0-5]([0-2]|[4-9])|[6-9]..JPG"
~~~
~~~
mm3d Tapas AutoCal $PAT_JPG InCal=iniCalib InOri=iniCalib Out=AlloriJPG
~~~

Utiliser AperiCloud pour percevoir le résultat
~~~
mm3d AperiCloud ".*.ARW" AlloriJPG
~~~

=======
mm3d AperiCloud ".*.(ARW|CR2|JPG)" Allori
~~~

temps de traitement : 13846 sec
>>>>>>> origin/master
>>>>>>> Stashed changes

### Tarama 

~~~
mm3d Tarama ".*.ARW" Allori 
~~~

####création du masque 

~~~
mm3d SaisieMasqQT TA/TA_LeChantier.tif
~~~

mm3d SaisieMasqQT "DSC00065.CR2" Attr=Plan
mm3d SaisieBascQT "DSC00065.CR2" Allori MesureBasc.xml

*Attention :* Renommer les points en Line1 et Line2

mm3d RepLocBascule ".*.ARW" Allori MesureBasc-S2D.xml RepereOrtho.xml PostPlan=_MasqPlan

### Malt
~~~
mm3d Malt Ortho ".*.ARW" Allori DirMEC=MEC-Malt-AlloriARW


mm3d Malt Ortho ".*.ARW" Allori  ImOrtho=".*.ARW" Etape0=7

mm3d to8Bits MEC-Malt/Z_Num8_DeZoom2_STD-MALT.tif Coul=1 Circ=1

mm3d to8bits Malt-UrbanMNE/Z_Num6_DeZoom4_STD-MALT.tif Coul=1 Circ=1
~~~

#tout à la suite
~~~
mm3d Tapioca MulScale ".*.ARW" 300 1200
mm3d Tapas RadialStd "DSC000(74|73|85|86|93|94).ARW" Out='Ini'
mm3d Tapas RadialStd ".*.ARW" InOri=Ini Out=Allori
mm3d AperiCloud ".*.ARW" Allori
mm3d Malt UrbanMNE ".*.ARW" Allori  ImOrtho=".*.ARW" DirMEC="Malt-UrbanMNE" DirOF="Ortho-Malt-UrbanMNE" DefCor=0.0
mm3d Tawny Ortho-Malt-UrbanMNE/ DEq=1 DegRap=1
~~~

# Ameliorer le rendu de l'ortho
~~~
mm3d Malt Ortho ".*.ARW" Allori ImOrtho=".*.ARW" SzW=1 ZReg=0,02 DefCor=0.0 Etape0=8
============= PARAMS ========== 
 -  SzWindow 2  (i.e. : 5x5)
 -  Regul 0.05
 -  Final Zoom 2
 -  Initial Zoom 32
 -  Use TA as Mask   Yes
 -  Z Step : 0.4
 -  Nb Min Visible Images : 3
~~~

-> changer la SzWindow 3x3 (`SzW=1`)

~~~
mm3d Malt UrbanMNE ".*.ARW" Allori  ImOrtho=".*.ARW" Repere=RepereOrtho.xml DirMEC="Malt-UrbanMNE" DirOF="Ortho-Malt-UrbanMNE" DefCor=0.0

============= PARAMS ========== 
 -  SzWindow 1  (i.e. : 3x3)
 -  Regul 0.02
 -  Final Zoom 1
 -  Initial Zoom 32
 -  Use TA as Mask   Yes
 -  Z Step : 0.4
 -  Nb Min Visible Images : 3
~~~
~~~
mm3d Malt Ortho ".*.JPG" RadialSTD  ImOrtho=".*.JPG" Repere=RepereOrtho.xml DefCor=0 DirMEC=Malt_DefCor0
~~~
-> changer le nb d'image visible a 2 (`NbVI=2`)

-> créer un plan : 

~~~
RepLocBascule "image.*JPG" AllOri MesureBasc-S2D.xml RepereCorrelation.xml PostPlan=_Masq
~~~

The `"PostPlan=_Masq"` argument means that MicMac will look for any picture filename ending with "*_Masq.tif". Then it will use this Mask and consider that it is flat. 
This can be done simply typing :

~~~
SaisieMasq PIC1234.ext Attr=_Flat
~~~

(note: if you type Attr=_Flat, then you should indicate PostPlan=_Flat. In my opinion it's better so that MiMac doesn't confuse with any other eventual mask you would create later in the process)
You could select a zone on the road (actually, it would be better if you'd selected 3 zones on different picture covering different areas, one at the beginning, on in the middle, and one at the end).
This is not imperative, but I guess it helps MicMac so that it doesn't miscompute its automatic mask. 



To avoid such holes in your model, you could :

- Take more (or better) pictures
- Play on the correlation coefficient (DefCor=0 for example during Malt)
  Il n'y a donc pas d'option pour densifier un BigMac, même si les options DefCor et ZReg (issues de Malt) peuvent permettre d'affiner un résultat.

~~~
mm3d Malt Ortho ".*.JPG" RadialSTD  ImOrtho=".*.JPG" Repere=RepereOrtho.xml DefCor=0 DirMEC=Malt_DefCor0
~~~

- Turn your pointcloud into a mesh, and texture it with the images

One way to check if you'll have hole in the final point cloud / ortho add in your AperiCloud the option `SeuilEc=2` (instead of 10 by default).

Maybe there is also a way to improve it going deeper in Tapas settings using EcMax to try to filter bad tie-points.

### Tawny
Tawny Ortho-MEC-Malt/ DEq=1 DegRap=1
Tawny Ortho-MEC-Malt/ DEq=1 DegRapXY=[4,1] Out=ortho-test-A (AUCUNE DIFF AVEC PRECEDENT)
Tawny Ortho-MEC-Malt/ DEq=1 DegRapXY=[4,1] ImPrio=Ort_IMG_.* SzV=3 CorThr=0.6 NbPerIm=5e4 Out=ortho-test-B

**Troubleshooting**

~~~
For required file [Ortho-MEC-Malt/MTDOrtho.xml]
------------------------------------------------------------
|   Sorry, the following FATAL ERROR happened               
|                                                           
|    Cannot open
|                  
~~~
see this

[http://forum-micmac.forumprod.com/malt-doesn-t-create-mtdortho-xml-then-tawny-fails-t808.html#p5263]()

générer MTDOrtho.xml

<?xml version="1.0" ?>
	<FileOriMnt>
	  <NameFileMnt>NO</NameFileMnt>
	  <NombrePixels>13910 8595</NombrePixels>
	  <OriginePlani>-19.8935 23.991</OriginePlani>
	  <ResolutionPlani>0.00275 -0.00275</ResolutionPlani>
	  <OrigineAlti>-10.428</OrigineAlti>
	  <ResolutionAlti>0.0055</ResolutionAlti>
	  <Geometrie>eGeomMNTEuclid</Geometrie>
	</FileOriMnt>




dans mon cas il ne le trouve toujours pas...relou

du coup j'utilise PIMS

~~~
mm3d PIMs QuickMac ".*.ARW" Allori/ Masq3D=AperiCloud_Allori_polyg3d.xml


mm3d PIMs2Mnt QuickMac DoOrtho=1 
mm3d Tawny PIMs-ORTHO/ RadiomEgal=0  Out=Orthophotomosaic.tif

~~~

mm3d C3DC BigMac ".*.ARW" Allori
