Plafond cylindrique
===

##mosquee abu dhabi

### Tapioca sur un sous éhantillonage pour trouver les principaux points homologues
~~~
mm3d Tapioca MulScale "FOC96.*.CR2" 300 1000
mm3d Tapioca MulScale "FOC97.*.CR2" 300 1000
mm3d Tapioca MulScale "FOC99.*.CR2" 300 1000
mm3d Tapioca line "FOC100.*.CR2" 
mm3d Tapioca MulScale "FOC102.*.CR2" 300 1000
mm3d Tapioca MulScale "FOC105.*.CR2" 300 1000


mm3d Tapioca All ".*.CR2" 1500
~~~

il n'y a pas d'images de calibration initiale. on prend les 5 premieres : "IMG_448(1|2|3|4|5)*.CR2"


### Tapas pour effectuer la calibration initiale
~~~

mm3d Tapas RadialStd ".*.CR2" Focs=[104,106] Out=F105
mm3d Tapas RadialStd ".*.CR2" InOri=F105 Out=All

mm3d Tapas RadialStd "FOC105_IMG_4((48[6-9])|(50[1-3])).*.CR2" Out=calibFOC105


 Focs=[95,97] Out=FOC96
Tapas AutoCal ".*.CR2" Focs=[97,100] Out=FOC99
Tapas AutoCal ".*.CR2" Focs=[99,101] Out=FOC100

Tapas AutoCal ".*.CR2" Focs=[100,101] Out=FOC100

Tapas AutoCal ".*.CR2" InCal=FOC96 Focs=[95,97] Out=Tmp96

~~~

copier le dossier  Ori-Ini pour le déposer dans le dossier des images à traiter

### Tapas pour faire l'orientation relative
~~~
mm3d Tapas RadialStd "IMG.*.CR2" 
~~~

### Apericloud pour créer un permier nuage de point issu des oientations

~~~
mm3d AperiCloud "IMG.*.CR2" RadialStd
~~~


##All inline
~~~~
mm3d Tapioca MulScale ".*.CR2" 300 1000
mm3d Tapas RadialStd "IMG_448(1|2|3|4|5)*.CR2" Out='Ini'
mm3d Tapioca MulScale ".*.CR2" 300 1000
mm3d Tapas RadialStd "IMG.*.CR2"
mm3d AperiCloud "IMG.*.CR2" RadialStd

~~~
