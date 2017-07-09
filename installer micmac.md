installer micmac sous MAC
=========================

-   pkg-config

~~~
    curl -OL http://pkgconfig.freedesktop.org/releases/pkg-config-0.28.tar.gz -o pkgconfig.tgz
    tar -zxf tar -zxf pkg-config-0.28.tar.gz  && cd pkg-config-0.28
     ./configure --with-internal-glib && sudo make install
~~~

-   [ExifTool](http://www.sno.phy.queensu.ca/~phil/exiftool/ExifTool-10.11.dmg)
    installation par interface

-   [Exiv2](http://www.exiv2.org/exiv2-0.25.tar.gz) Compiler les sources exiv2
    dans le dossier sources téléchargées

~~~
./configure make sudo make install 
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:/usr/lib/pkgconfig:/opt/local/lib/pkgconfig" make samples make tests 
~~~

-   [Proj4](http://download.osgeo.org/proj/proj-4.9.1.tar.gz)

dans le dossier sources téléchargées 

~~~
	./configure --mandir=/usr/local/share/man --disable-dependency-tracking make sudo make
install
~~~

[Micmac](http://logiciels.ign.fr/spip.php?page=telecharger_logiciel&id_document=174)

---

Télecharger les fichiers binaires, déplacer le dossier dans Applications.
Ajouter la variable d'environnement

~~~
 
export PATH=/Library/Frameworks/GDAL.framework/Programs:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/opt/ImageMagick/bin:/Applications/3D/micmac-1.0.beta7/bin/


export PATH=$PATH:/Applications/MicMacOs/bin/
~~~~

Tester

~~~
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
~~~

 

Depuis les sources
------------------


Installer les prérequis listés dans le Readme. De plus besoin d’installer Cmake, proj4, g++, gcc, xcode, Qt5 et autres ?

- installer cmake 

		brew install cmake

- installer qt5

Télécharger les sources qt-everywhere-opensource-src ici

[https://download.qt.io/archive/qt/5.8/5.8.0/single/]()

extraire les sources dans un dossier tmp

	cd tmp/
	./configure

répondre aux questions de la licence (opensource)

~~~
Which edition of Qt do you want to use ?

Type 'c' if you want to use the Commercial Edition.
Type 'o' if you want to use the Open Source Edition.

o


This is the Qt Open Source Edition.

You are licensed to use this software under the terms of
the GNU Lesser General Public License (LGPL) version 3.
You are also licensed to use this software under the terms of
the GNU General Public License (GPL) version 2.

Type 'L' to view the GNU Lesser General Public License version 3.
Type 'G' to view the GNU General Public License version 2.
Type 'yes' to accept this license offer.
Type 'no' to decline this license offer.

Do you accept the terms of either license? yes

Creating qmake...
.................
~~~

la compilation prend une journée.

maintenant lancer `make`

ajouter qt aux variables d'environnement

	PATH=$PATH:/usr/local/Qt-5.8.0/bin/

ou

~~~
echo 'export PATH=$PATH:/usr/local/Qt-5.8.0/bin/' >> ~/.bash_profile
source ~/.bash_profile
~~~

PATH complet

~~~
echo 'export PATH=/Library/Frameworks/GDAL.framework/Programs:/Library/Frameworks/GDAL.framework/Programs:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/opt/ImageMagick/bin:/Applications/3D/micmac-1.0.beta7/bin/:/usr/local/Qt-5.8.0/bin/:/Applications/3D/MicMacOs/bin/' >> ~/.bash_profile
source ~/.bash_profile
~~~



## Depuis Github

	git clone https://github.com/micmacIGN/micmac.git micmac

Compiler micmac avec l’option QT5

~~~
cd micmac
mkdir build
cd build


cmake -DCMAKE_PREFIX_PATH=/usr/local/Qt-5.8.0 -DWITH_HEADER_PRECOMP=OFF -DWITH_QT5=1 -DBUILD_POISSON=ON ../

cmake  -DWITH_Qt5=1 ../
make install -j8
~~~


cmake -DWITH_QT5=1 -DBUILD_POISSON=ON ../
cmake -DCMAKE_PREFIX_PATH=/usr/local/Qt-5.8.0 -DWITH_QT5=1 -DBUILD_POISSON=ON ../
cmake -DWITH_HEADER_PRECOMP=OFF -DWITH_QT5=1 -DBUILD_POISSON=ON ../


### Mettre à jour sa version avec Github

	git pull
	cd build
	cmake ../ -DWITH_Qt5=1
	make install



Documentation
=============

[forum micmac](http://forum-micmac.forumprod.com/installation-mac-t278-10.html)

 
