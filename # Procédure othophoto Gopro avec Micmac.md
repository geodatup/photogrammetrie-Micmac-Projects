Procédure othophoto Gopro avec Micmac
=====================================

 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
export PATH=$PATH:/Applications/3D/MicMacOs/bin/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

Description prise de vue
------------------------

Description des images sources
------------------------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Localisation : /
Liste des noms :
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Exif et fichier LocalChantier.xml
---------------------------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
for file in ./*.JPG; do
  convert "$file" -distort barrel "0.06335 -0.18432 -0.13009" "fisheyeless/${file%.JPG}"_corrige.JPG
done
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tapioca
-------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mm3d Tapioca MulScale ".*JPG" 300 1000 ExpTxt=1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tapas
-----

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mm3d Tapas RadialStd ".*JPG" Out=Cal1 ExpTxt=1
mm3d Tapas AutoCal ".*JPG" InCal=Cal1 Out=Ori1 ExpTxt=1


mm3d Tapas RadialStd "G0027(501|502)_corrige.JPG" Out=Cal1 ExpTxt=1
mm3d Tapas AutoCal ".*JPG" InCal=Cal1 Out=Ori1 ExpTxt=1

mm3d Tapas FishEyeEqui "G0028(820|821|822|823).JPG" Out=Cal1 ExpTxt=1
mm3d Tapas FishEyeEqui ".*JPG" InCal=Cal1 Out=Ori1 ExpTxt=1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

AperiCloud
----------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mm3d AperiCloud ".*JPG" Ori1 Out=PosCams.ply ExpTxt=1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[Meshlab](https://sourceforge.net/projects/meshlab/files/meshlab/MeshLab%20v1.3.3/MeshLabMac_v133.dmg/download)
===============================================================================================================

Visualiser le nuage de point .ply

Malt
----

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Malt Ortho ".*JPG" Ori1 "DirMEC=Resultats3" UseTA=1 ZoomF=4 ZoomI=32 Purge=true
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 


Tawny
-----

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

mm3d Tawny Ortho-Resultats3/
Nuage2Ply Resultats3/NuageImProf_STD-MALT_Etape_6.xml Attr="Ortho-Resultats3/Ortho-Eg-Test-Redr.tif"

mm3d Tapioca MulScale "0(0[5-9]|1[0-9]|2[0-7]{1}).jpg" 300 1000
mm3d Tapas RadialStd "0(0[5-9]|1[0-9]|2[0-7]{1}).jpg" Out=Cal1
mm3d Tapas AutoCal "0(0[5-9]|1[0-9]|2[0-7]{1}).jpg" InCal=Cal1 Out=Ori1
mm3d AperiCloud "0(0[5-9]|1[0-9]|2[0-7]{1}).jpg" Ori1 Out=PosCams.ply
mm3d Malt Ortho "0(0[5-9]|1[0-9]|2[0-7]{1}).jpg" Ori1 "DirMEC=Resultats3" UseTA=1 ZoomF=4 ZoomI=32 Purge=true
mm3d Tawny Ortho-Resultats3/
Nuage2Ply Resultats3/NuageImProf_STD-MALT_Etape_6.xml Attr="Ortho-Resultats3/Ortho-Eg-Test-Redr.tif"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

for file in ./\*.JPG; do convert "$$file" -rotate "180"
"$${file%.JPG}"\_corrige.JPG done

for file in ./\*.JPG; do convert "$$file" -resize 2500x2500^ -gravity Center
-crop 2500x2500+0+0 +repage "crops/$${file%.JPG}"\_resize.JPG done

 

 

Faire un masque
===============

mm3d SaisieMasqQT

(bouton de gauche pour d´efinir le polygone \`a conserver qui sera marqu´e en
vert, shift-bouton de gauche pour fermer le polygone, et finalement ctrl-bouton
de droite et Exit pour sauver les fichier .tif du masque et .xml de description)

 

 

Tout à la suite
===============

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
export PATH=$PATH:/Applications/3D/MicMacOs/bin/

mm3d Tapioca MulScale ".*JPG" 300 1000 ExpTxt=1
mm3d Tapas RadialStd ".*JPG" Out=Cal1 ExpTxt=1
mm3d Tapas AutoCal ".*JPG" InCal=Cal1 Out=Ori1 ExpTxt=1
mm3d AperiCloud ".*JPG" Ori1 Out=PosCams.ply ExpTxt=1
Malt Ortho ".*JPG" Ori1 "DirMEC=Resultats3" UseTA=1 ZoomF=4 ZoomI=32 Purge=true UseTA=1
mm3d Tawny Ortho-Resultats3/
Nuage2Ply Resultats3/NuageImProf_STD-MALT_Etape_6.xml Attr="Ortho-Resultats3/Ortho-Eg-Test-Redr.tif"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
