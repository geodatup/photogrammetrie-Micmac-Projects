#Réalisation du modèle 3D Bidon de Claire

APN : 5D
Focale : 24mm
Diaf : 11
Distance prise de vue : 1,5 m
Résolution spatiale : ?

Nombre d'images : 19

## Etape 1 : Points homologues / points de liaison

~~~
mm3d Tapioca MulScale ".*.JPG" 300 1000
~~~

## Etape 2 : Orientation relative

~~~
mm3d Tapas RadialExtended ".*.JPG" 
~~~

Analyse des résultats : les résidus doivent être proche de 0.

~~~
RESIDU LIAISON MOYENS = 1.06066 pour Id_Pastis_Hom Evol, Moy=5.04115e-13 ,Max=3.59027e-09
--- End Iter 10 ETAPE 3
~~~


## Etape 3 : Création d'un premier nuage de point avec les orientations relatives

> traitement long

~~~
mm3d AperiCloud ".*.JPG" Ori-RadialExtended
~~~


# Etape 4 : création d'un masque 3D

~~~
mm3d SaisieMasqQT AperiCloud_RadialExtended.ply
~~~

## Etape 5 : Dense Matching

~~~
mm3d C3DC QuickMac ".*.JPG"  Ori-RadialExtended Masq3D=AperiCloud_RadialExtended_selectionInfo.xml
~~~