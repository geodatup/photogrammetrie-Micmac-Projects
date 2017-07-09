# Réalisation d'un modèle 3D et orthophoto de Rouyre 

La prise de vue a été réalisé à partir d'un drone Iris + équipé d'une gopro 3, sur nacelle 3 girostabilisée.

Le plan de vol est réalisé avec Mission planner à l'aide de l'outil ...

La superificie parcourue est d'environ 5ha.

Altitude de prise de vue : 50 m



Temps de la prise de vue : 11 minutes
Vent : environs 5 kt mais avec rafales à 10/15


La gorpo est paramétrée en time lapse à 2 images / seconde.

Nombre de photo final : 166




## Faire un nuage de point 3D à partir de photo


Pour démarrer Micmac, ajouter le Path des sources aux variables d'environnement :

Faites le de manière temporaire, 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
export PATH=$PATH:/Applications/3D/MicMacOs/bin/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ou permanante :

~~~
echo 'export PATH=$PATH:/Applications/3D/MicMacOs/bin/' >> ~/.bash_profile
source ~/.bash_profile
~~~

### Etape 1 : Définir les points homologues / points de liaisons
> matching d'un nuage de point non dense

On se base sur des résolutions multiples mais inferieur à la résolution maximale [300 - 1000]. Pour cela nous utilison le parametre **Mulscale** de Tapas et une expression régulière définiant toutes les images du bloc image.

> Le traitement peut prendre plusieurs heures si le nombre d'image est important.


~~~
mm3d Tapioca MulScale ".*.JPG" 300 1000
~~~

Analyse des résultats : 


### Orientation relative des sommets de camera

Pour définir l'orientation du sommet de chaque prise de vue, nous devons réaliser une calibraiton initiale de la camera. Pour cela Les parametres de calibration de la camera sont calculés automatiquement à partir d'images géometriquement remarquables (profondeur de champs, angles etc..). 
Dans le cas d'une prise de vue avec une Gopro il faut utiliser un modèle de calcul **FisheEyeBasic**.

> environ 20 sec

~~~
mm3d Tapas FishEyeBasic "G00310(44|45|46|47).JPG" Out=Ini
~~~

On effectue ensuite l'orientation de toutes images à partir de la calibrtion initiale précédente.

> plusieurs heures : utiliser Liquor (parallélisation) pour un traitement rapide de l'orientation.



~~~
mm3d Tapas FishEyeBasic ".*.JPG" InOri=Ini Out=Allori
~~~

> Orientation relative :  
L'initialisation de l'orientation pour un nombre important d'image (des centaines) peut être réalisé avec l'outil Martini :

>~~~
mm3d Martini ".*.JPG"
~~~

>~~~
AperiCloud ".*.JPG" Martini Out=Martini-cam.ply WithPoints=0
~~~

>As Martini do not account for any radial distortion of the lens, the visual inspection of the image
orientation with AperiCloud show large non-linear distortions. Initialization of the image orientation can
also be performed successfully directly with Tapas, but for large image block, the use of Martini is faster.  
The complete dataset is then aligned in a relative orientation.

>~~~
Tapas RadialBasic ".*.JPG" Out=Rel InCal=Martini
~~~

### Création du premier nuage de point avec les orientations

> traitement long

~~~
mm3d AperiCloud ".*.JPG" Allori
~~~

Ouvrir le fichier dans Meshlab pour voir si tout est correct.

### Ajustements géométriques 

Les ajustements suivants sont fait manuellement. Ils sont facultatifs mais ils améliorent la précision des rendus finaux.

####  Definir l'incidence du plan

On choisi une image sur laquelle il y a une zone plate (ou parallèle au plan de notre appareil photo).

##### Création du plan
- clic gauche (premier point)
- + clic gauche (deuxieme point)
- + clic gauche (troisieme point)
- + clic droit = polygone
- + espace : créer le masque
- + Commande + S = save

~~~
mm3d SaisieMasqQT "G0031030.JPG" Attr=Plan
~~~

2 fichiers sont créés :

- `G0031030_MasqPlan.tif`
- `G0031030_MasqPlan.xml`

le fichier xml fait un lien avec l'image tif, il ressemble à ça :

~~~
<?xml version="1.0" ?>
<FileOriMnt>
     <NameFileMnt>./G0031030_MasqPlan.tif</NameFileMnt>
     <NombrePixels>3840 2880</NombrePixels>
     <OriginePlani>0 0</OriginePlani>
     <ResolutionPlani>1 1</ResolutionPlani>
     <OrigineAlti>0</OrigineAlti>
     <ResolutionAlti>1</ResolutionAlti>
     <Geometrie>eGeomMNTFaisceauIm1PrCh_Px1D</Geometrie>
</FileOriMnt>
~~~

##### Création de la ligne d'incidence

Définir l'incidence du plan avec Line 1 et 2 (1 est le premier point et 2 le
second, la ligne doit être créée horizontalement).
Séléctionner ligne 1 ou Ligne 2 dans le tableau à droite, puis cliquer sur l'image pour créer le point. 

Fermer QT pour sauvegarder les changements.

~~~
mm3d SaisieBascQT "G0031030.JPG" Allori MesureBasc.xml
~~~

Un fichier `MesureBasc-S2D.xml` sera créé :

~~~
<?xml version="1.0" ?>
<SetOfMesureAppuisFlottants>
     <MesureAppuiFlottant1Im>
          <NameIm>G0031030.JPG</NameIm>
          <OneMesureAF1I>
               <NamePt>Line1</NamePt>
               <PtIm>1470.8544863147622 1611.22844097804546</PtIm>
          </OneMesureAF1I>
          <OneMesureAF1I>
               <NamePt>Line2</NamePt>
               <PtIm>1954.2137223485222 1634.38773648515371</PtIm>
          </OneMesureAF1I>
     </MesureAppuiFlottant1Im>
</SetOfMesureAppuisFlottants>
~~~ 


#### Bascule des orientations vis à vis du plan

> traitement rapide

~~~
mm3d RepLocBascule ".*.JPG" Allori MesureBasc-S2D.xml RepereOrtho.xml PostPlan=_MasqPlan
~~~

### Création de l'orthophoto

Premièrement on génére une orthophoto basse résolution pour créer un masque sur la zone d'intéret. Ceci accélérera le traitement par la suite

> environs 1 h

~~~
mm3d Tarama ".*.JPG" Allori Repere=RepereOrtho.xml
~~~

Création du masque :

> opération manuelle

~~~
mm3d SaisieMasqQT TA/TA_LeChantier.tif
~~~

#####Création des orthophoto avec Malt

~~~
mm3d Malt Ortho ".*.JPG" Allori  ImOrtho=".*.JPG" Repere=RepereOrtho.xml 
~~~

si vous avez besoin de redemarer une étape (car le traitement a été interrompu), il faut ajouter le parametre  `Etape0=N` ou N est le numéro de l'étape.


~~~
mm3d Malt Ortho ".*.JPG" Allori  ImOrtho=".*.JPG" Etape0=9 Repere=RepereOrtho.xml 
~~~

Il est possible de convertir en 8bit l'image résultante de la dernière étape 

~~~
mm3d to8Bits MEC-Malt/Z_Num8_DeZoom2_STD-MALT.tif Coul=1 Circ=1 Mask=MEC-Malt/AutoMask_STD-MALT_Num_7.tif
~~~

~~~
mm3d to8Bits MEC-Malt/Z_Num9_DeZoom2_STD-MALT.tif Coul=1 Circ=1 Mask=MEC-Malt/AutoMask_STD-MALT_Num_8.tif
~~~

ou en nuance de gris

~~~
mm3d GrShade MEC-Malt/Z_Num9_DeZoom2_STD-MALT.tif ModeOmbre=IgnE Mask=MEC-Malt/AutoMask_STD-MALT_Num_8.tif
~~~

Puis faire la fusion des orthophoto présente dans le dossier MecMalt

~~~
mm3d Tawny Ortho-MEC-Malt/
~~~

-> résultats, le fichier `Ortho-Eg-Test-Redr` de qualité médiocre


##### Création d'un nuage de points et des orthophotos avec C3DC et PIMS

Création du nuage de point dense.
> C3DC autorise l'utilisation d'un masque 3D réalisé avec SaisieMasqQT (en utilisant le nuage de point en entrée) 
 
~~~
mm3d C3DC BigMac ".*JPG" Allori
~~~

ou avec PIMS

~~~
mm3d PIMs BigMac ".*JPG" Allori
~~~

Fusionner les images de profondeur dans un MNS et réaliser l'orthoimoage avec PIMs2Mnt
> ne pas oublier d'ajouter le slash dans le nom du dossier PIMS sinon le process ne démarre pas

~~~
mm3d Pims2MNT PIMs-BigMac/ DoMnt=1 DoOrtho=1
~~~

Fusionner les orthoimages dans une mosaique d'orthoimage avec Tawny

~~~
mm3d Tawny PIMs-ORTHO/  Out=Orthophotomosaic.tif
~~~


###création d’un Mesh

~~~
mm3d Tipunch C3DC_BigMac.ply Filter=0  Scale=6
~~~

###Texturer le Mesh

~~~
mm3d Tequila "IMG_4051.JPG" RadialSTD C3DC_BigMac_mesh.ply
~~~
 

###Création du MNT et des Ortho

~~~
mm3d PIMs BigMac ".\*.CR2" RadialSTD

mm3d Pims2MNT PIMs-QuickMac/ DoMnt=1 DoOrtho=1
~~~





---
To avoid such holes in your model, you could :

- Take more (or better) pictures
- Play on the correlation coefficient (DefCor=0 for example during Malt)
Il n'y a donc pas d'option pour densifier un BigMac, même si les options DefCor et ZReg (issues de Malt) peuvent permettre d'affiner un résultat.
~~~
mm3d Malt Ortho ".*.JPG" RadialSTD  ImOrtho=".*.JPG" Repere=RepereOrtho.xml DefCor=0 DirMEC=Malt_DefCor0
~~~

- Turn your pointcloud into a mesh, and texture it with the images

One way to check if you'll have hole in the final point cloud / ortho add in your AperiCloud the option SeuilEc=2 (instead of 10 by default).

Maybe there is also a way to improve it going deeper in Tapas settings using EcMax to try to filter bad tie-points.