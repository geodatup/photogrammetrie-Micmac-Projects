
Ground imagery
==============

##Etable

### Tapioca pour trouver les points homologues
~~~
mm3d Tapioca MulScale "24mm/IMG.*.JPG" 300 1000
~~~

les images de calibration initiale se situent dans le sous dossier : ```"Cali_.*.JPG"```

### Tapas pour effectuer la calibration initiale
~~~
mm3d Tapioca MulScale "calib_24mm/.*.JPG" 300 1000
mm3d Tapas RadialStd "calib_24mm/.*.JPG" Out='Ini'
~~~

copier le dossier  Ori-Ini pour le déposer dans le dossier des images à traiter

### Tapas pour faire l'orientation relative
~~~
cd 24mm
mm3d Tapas RadialStd "IMG.*.JPG" 
~~~

### Apericloud pour créer un permier nuage de point issu des oientations

~~~
mm3d AperiCloud "IMG.*.JPG" RadialStd
~~~

### Créer la ligne d'incidence  sur une image 

~~~
mm3d SaisieMasqQT "IMG_4056.JPG" Attr=Plan

mm3d SaisieBascQT "IMG_4056.JPG" RadialStd MesureBasc.xml

mm3d RepLocBascule ".*.JPG" RadialStd MesureBasc-S2D.xml RepereOrtho.xml PostPlan=_MasqPlan
~~~

### création de l'orthophoto

~~~
mm3d Tarama ".*.JPG" RadialStd Repere=RepereOrtho.xml 
~~~

####création du masque 

~~~
mm3d SaisieMasqQT TA/TA_LeChantier.tif
~~~


####génération du nuage de point 3D complet

~~~
mm3d C3DC BigMac ".*JPG" RadialStd
~~~
le rendu du nuage de point peut être visualisé dans meshlab

#### générer ortho et MNT
~~~
mm3d Pims2MNT PIMs-BigMac/ DoMnt=1 DoOrtho=1
~~~

####verifier le MNT en sortie 

~~~
mm3d GrShade PIMs-TmpMntOrtho/Z_Num1_DeZoom2_LeChantier.tif ModeOmbre=IgnE
~~~

mosaicage des images 

~~~
mm3d Tawny PIMs-ORTHO/ Out=Orthophotomosaic.tif
~~~

j'ai une erreur, micmac ne trouve pas le fichier MTDOrtho.xml (il n'a pas été généré par C3DC ou PIMS et du coup Tawny est paumé.

obligé donc de passé par Malt pour générer le fichier

~~~
mm3d Malt Ortho ".*.JPG" RadialSTD  ImOrtho=".*.JPG" Repere=RepereOrtho.xml 


mm3d Tawny Ortho-MEC-Malt/ Out=Ortho-toit

~~~



Apparement le rendu PIMS de C3DC a merdé quelque part. Pourtant le nuage de point C3DC est ok.

Je lance PIMS en mode BigMac seul

~~~
mm3d PIMs BigMac ".*JPG" RadialStd

mm3d GrShade PIMs-TmpBasc/PIMS-Merged_Prof.tif ModeOmbre=IgnE
~~~




### Essai avec un modèle cylindrique

~~~
mm3d RepLocBascule ".*.JPG" RadialStd MesureBasc-S2D.xml Ortho-Cyl1.xml PostPlan=_MasqPlan OrthoCyl=true
mm3d Tarama ".*.JPG" RadialStd Repere=Ortho-Cyl1.xml Out=TA-OC Zoom=4
mm3d SaisieMasqQT TA/TA_LeChantier.tif
mm3d Malt Ortho ".*.JPG" RadialStd Repere=Ortho-Cyl1.xml SzW=1 ZoomF=1 DirMEC=Malt-OC-F1 DirTA=TA-OC
mm3d Tawny Ortho-UnAnam-Malt-OC-F1/
mm3d GrShade Malt-OC-F1/Z_Num1_DeZoom1_Malt-Ortho-UnAnam.tif ModeOmbre=IgnE
~~~

Bergerie facade SUD
-------------------

image de calibration initiale :


```"Cali.\*.CR2"```

~~~
mm3d Tapioca MulScale "Fsud.\*.CR2" 400 1500

mm3d Tapas RadialStd "Cali.\*.CR2" Out=Ini

mm3d Tapas RadialStd "Fsud.\*.CR2" InOri=Ini Out=Allori

mm3d AperiCloud "Fsud.\*.CR2" Allori

mm3d SaisieMasqQT "Fsud\_IMG\_3095.CR2" Attr=Plan

mm3d SaisieBascQT "Fsud\_IMG\_3095.CR2" Allori MesureBasc.xml

mm3d RepLocBascule "Fsud.\*.CR2" Allori MesureBasc-S2D.xml RepereOrtho.xml PostPlan=\_MasqPlan

mm3d Tarama "Fsud.\*.CR2" Allori Repere=RepereOrtho.xml

mm3d SaisieMasqQT TA/TA\_LeChantier.tif

mm3d Malt Ortho "Fsud.\*.CR2" Allori ImMNT="Fsud.\*.CR2" ImOrtho="Fsud.\*.CR2"
Repere=RepereOrtho.xml

mm3d Tawny Ortho-MEC-Malt/

mm3d Nuage2Ply MEC-Malt/NuageImProf\_STD-MALT\_Etape\_8.xml
Attr=Ortho-MEC-Malt/Ortho-Eg-Test-Redr.tif RatioAttrCarte=2 Scale=2
~~~
 

**Phase 2**

~~~
mm3d Tapioca MulScale "F.\*.CR2" 400 1500

mm3d Tapas RadialStd "(Fsud\_IMG\_3091\|Cali.\*).CR2" Out=Ini

mm3d Tapas RadialStd "F.\*.CR2" InOri=Ini Out=Allori

mm3d AperiCloud "F.\*.CR2" Allori

mm3d C3DC BigMac ".\*.CR2" Allori

mm3d Tipunch C3DC\_BigMac.ply Filter=0 Depth=12
~~~
 

 

Four à pain
-----------

Réutilisation du fichier ini de la bergerie

~~~
mm3d Tapioca MulScale ".\*.CR2" 300 1000

mm3d Tapas RadialStd ".\*32(20\|21\|18\|19).CR2" Out=Ini

mm3d Tapas RadialStd ".\*.CR2" InOri=Ini Out=Allori

mm3d AperiCloud ".\*.CR2" Allori

 

mm3d SaisieMasqQT AperiCloud\_Allori.ply

 

 

mm3d C3DC QuickMac ".\*.CR2" Allori Masq3D=AperiCloud\_Allori\_selectionInfo.xml
~~~

###création d’un Mesh

~~~
mm3d Tipunch C3DC\_QuickMac.ply Filter=0 Depth=12
~~~

###Texturer le Mesh

~~~
mm3d Tequila ".\*.CR2" Allori C3DC\_QuickMac.ply
~~~
 

###Création du MNT et des Ortho

~~~
mm3d PIMs BigMac ".\*.CR2" Ori-Allori/

mm3d Pims2MNT PIMs-QuickMac/ DoMnt=1 DoOrtho=1
~~~
 

###Fusion des ortho

~~~
mm3d Tawny PIMs-ORTHO/ Out=Orthomosaic.tif
~~~
 

Inside bergerie
---------------

 

~~~
mm3d Tapioca MulScale ".\*.CR2" 400 1500

mm3d Tapas Figee ".\*.CR2" InCal=Ini

mm3d AperiCloud ".\*.CR2" Allori

mm3d C3DC QuickMac ".\*.CR2" Allori

mm3d Tipunch C3DC\_QuickMac.ply

mm3d Tequila « .\*.CR2" Allori C3DC\_QuickMac.ply
~~~

