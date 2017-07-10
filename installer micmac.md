#installer micmac sous MAC



* pkg-config
```
curl -OL http://pkgconfig.freedesktop.org/releases/pkg-config-0.28.tar.gz -o pkgconfig.tgz
tar -zxf tar -zxf pkg-config-0.28.tar.gz  && cd pkg-config-0.28
 ./configure --with-internal-glib && sudo make install
```


* [ExifTool](http://www.sno.phy.queensu.ca/~phil/exiftool/ExifTool-10.11.dmg)
installation par interface

* [Exiv2](http://www.exiv2.org/exiv2-0.25.tar.gz)
Compiler les sources exiv2
dans le dossier sources téléchargées
''
./configure
make
sudo make install
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:/usr/lib/pkgconfig:/opt/local/lib/pkgconfig" 
make samples
make tests
'''

* [Proj4](http://download.osgeo.org/proj/proj-4.9.1.tar.gz)

dans le dossier sources téléchargées
'''
./configure --mandir=/usr/local/share/man --disable-dependency-tracking
make
sudo make install
'''

# [Micmac](http://logiciels.ign.fr/spip.php?page=telecharger_logiciel&id_document=174)
Télecharger les fichiers binaires, déplacer le dossier dans Applications.
Ajouter la variable d'environnement
```
export PATH=$PATH:/Applications/MicMacOs/bin/
```
Tester
```
$ mm3d CheckDependencies
mercurial revision : 5368

byte order   : little-endian
address size : 64 bits

--- Qt enabled : 5

make:  found (/usr/bin/make)
exiftool:  found (/usr/local/bin/exiftool)
exiv2:  found (/usr/local/bin/exiv2)
convert:  found (/opt/ImageMagick/bin/convert)
proj:  found (/usr/local/bin/proj)
cs2cs:  found (/usr/local/bin/cs2cs)
siftpp_tgi.OSX:  found (/Applications/MicMacOs/binaire-aux/siftpp_tgi.OSX)
ann_samplekey200filtre.OSX:  found (/Applications/MicMacOs/binaire-aux/ann_samplekey200filtre.OSX)
```

```
cd /Users/R/Desktop/Rouyre/ortho\ 3D/
```

#imagemagick
Corriger l'effet fisheye
```
for file in ./*.JPG; do
  convert "$file" -distort barrel "0.06335 -0.18432 -0.13009" "${file%.JPG}"_corrige.JPG
done
```



Tapioca
```
mm3d Tapioca MulScale ".*JPG" 300 1000 ExpTxt=1
```

Tapas 
```
mm3d Tapas RadialStd ".*JPG" Out=Cal1 ExpTxt=1
mm3d Tapas AutoCal ".*JPG" InCal=Cal1 Out=Ori1 ExpTxt=1
```

AperiCloud
```
mm3d AperiCloud ".*JPG" Ori1 Out=PosCams.ply ExpTxt=1
```


# [Meshlab](https://sourceforge.net/projects/meshlab/files/meshlab/MeshLab%20v1.3.3/MeshLabMac_v133.dmg/download)
Visualiser le nuage de point .ply


Malt
```
Malt Ortho ".*JPG" Ori1 "DirMEC=Resultats3" UseTA=1 ZoomF=4 ZoomI=32 Purge=true
mm3d Tawny Ortho-Resultats3/
Nuage2Ply Resultats3/NuageImProf_STD-MALT_Etape_6.xml Attr="Ortho-Resultats3/Ortho-Eg-Test-Redr.tif"
```




#Avec GPS

Verifier que l'exif de vos photos contient le Model
```
exiftool [path.jpg] | grep Model 
```
car il se peut qu'APM mission planner ecrase les données exif nom de la camera lorsqu'il ajoute les données GPS
Voici la commande exif pour ajouter le model de camera à l'exif
```
exiftool -Model='Hero3-Silver Edition' -Make='Gopro' /Users/R/Desktop/Rouyre/aerial\ imagery\ campain/2nd\ prise/geotagged
```
sinon micmac échouera car il ne trouvera pas de reference du model Gorpo dans son dictionnaire


# Micmac

```
mm3d Tapioca MulScale ".*JPG" 300 1000 
```




```
mm3d Tapioca MulScale ".*JPG" 300 1000 
mm3d Tapas RadialStd "(1[0-9]|2[0-4]).jpg" Out=Init
mm3d Tapas AutoCal "(1[0-9]|2[0-4]).jpg" InCal=Init Out=Init1
mm3d AperiCloud "(1[0-9]|2[0-4]).jpg" Init1
mm3d OriConvert OriTxtInFile position_UTM33_cut_Zcst.csv jmfgps
mm3d CenterBascule "(1[0-9]|2[0-4]).jpg" Ori-Init1 Ori-jmfgps Abs0
mm3d Campari "(1[0-9]|2[0-4]).jpg" Abs0 Abs1 EmGPS=[jmfgps,0.1]]
mm3d Malt GeomImage "(1[5-9]).jpg" Abs1 Master=16.jpg DirMEC=Result1 ZoomF=4 ZoomI=32 Purge=true DefCor=0.001
mm3d Nuage2Ply Result1/NuageImProf_STD-MALT_Etape_6.xml Attr=16.jpg RatioAttrCarte=4
mm3d Malt GeomImage "(1[1-3]|2[0-2]).jpg" Abs1 Master=21.jpg DirMEC=Result2 UseTA=1 ZoomF=4 ZoomI=32 \
Purge=true DefCor=0.001
mm3d Nuage2Ply Result2/NuageImProf_STD-MALT_Etape_6.xml Attr=21.jpg RatioAttrCarte=4
mm3d Malt GeomImage "(10|2[3-4]).jpg" Abs1 Master=10.jpg DirMEC=Result4 UseTA=1 ZoomF=4 ZoomI=32 \
Purge=true DefCor=0.001
mm3d Nuage2Ply Result4/NuageImProf_STD-MALT_Etape_6.xml Attr=10.jpg RatioAttrCarte=4
```







# Documentation
[forum micmac](http://forum-micmac.forumprod.com/installation-mac-t278-10.html)