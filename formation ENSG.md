\--------------------------------------------

\| MICMAC - Formation juin 2016  \|

\--------------------------------------------

21/6/2016 : Jeu de données FermeDuBuisson

21/06/2016, mardi apres-midi

mm3d

mm3d Tapioca

mm3d Tapioca -help

mm3d Tapioca MulScale ".\*.CR2" 500 1500

SEL ./ IMG\_9682.CR2 IMG\_9683.CR2 KH=NB

mm3d Tapas RadialStd "IMG\_96[0-9]{2}.CR2" Out=Ini

mm3d Tapas RadialStd ".\*.CR2" InOri=Ini Out=Toutes

mm3d AperiCloud ".\*.CR2" Toutes

mm3d SaisieMasq "IMG\_9689.CR2" Attr=Plan

mm3d SaisieBasc "IMG\_9689.CR2" Toutes MesureBasc.xml

mm3d RepLocBascule "IMG\_96[0-9]{2}.CR2" Toutes MesureBasc-S2D.xml
RepereOrtho.xml PostPlan=\_MasqPlan

mm3d Tarama "IMG\_96[0-9]{2}.CR2" Toutes Repere=RepereOrtho.xml

mm3d SaisieMasq TA/TA\_LeChantier.tif

mm3d Malt Ortho ".\*.CR2" Toutes ImMNT="IMG\_96[0-9]{2}.CR2"
ImOrtho="IMG\_97[0-9]{2}.CR2" Repere=RepereOrtho.xml

mm3d to8Bits MEC-Malt/Z\_Num8\_DeZoom2\_STD-MALT.tif Coul=1 Circ=1
Mask=MEC-Malt/AutoMask\_STD-MALT\_Num\_7.tif

mm3d GrShade MEC-Malt/Z\_Num8\_DeZoom2\_STD-MALT.tif ModeOmbre=IgnE
Mask=MEC-Malt/AutoMask\_STD-MALT\_Num\_7.tif

mm3d Tawny Ortho-MEC-Malt/

mm3d Nuage2Ply MEC-Malt/NuageImProf\_STD-MALT\_Etape\_8.xml
Attr=Ortho-MEC-Malt/Ortho-Eg-Test-Redr.tif RatioAttrCarte=2 Scale=2

22/6/2016 : Jeu de donné Cuxa

mm3d Tapioca MulScale ".\*jpg" 200 800

mm3d Tapas RadialBasic "Abbey-IMG\_(0248\|0247\|0249\|0238\|0239\|0240).jpg"
Out=Calib

mm3d AperiCloud "Abbey-IMG\_(0248\|0247\|0249\|0238\|0239\|0240).jpg" Calib

mm3d Tapas RadialBasic "Abbey-.\*.jpg" InCal=Calib Out=All-Rel

Mm3d TestRegEx “.\*.jpg” =\> Tester Expression Regulier

mm3d AperiCloud "Abbey-IMG\_[0-9]\*.jpg" All-Rel

mm3d SaisieAppuisInit "Abbey-IMG\_(021[12]\|023[3456]).jpg" Ori-All-Rel/
NamePointInit.txt MesurePoints.xml

mm3d GCPBascule "Abbey-.\*jpg" All-Rel Terrain-init F120601.xml
MesurePoints-S2D.xml

mm3d SaisieAppuisPredic "Abbey-.\*jpg" Terrain-init F120601.xml MesureFinale.xml

mm3d GCPBascule "Abbey.\*jpg" Terrain-init Terrain F120601.xml
MesureFinale-S2D.xml

Mm3d AperiCloud “.\*.jpg” Terrain

mm3d Campari "Abbey.\*.jpg" Terrain TerrainFinal
GCP=[F120601.xml,0.1,MesureFinale-S2D.xml,0.5]

mm3d C3DC MicMac ".\*jpg" TerrainFinal

mm3d PIMs BigMac ".\*.jpg" Ori-TerrainCamp/

mm3d Pims2MNT PIMs-QuickMac DoMnt=1 DoOrtho=1

mm3d GrShade PIMs-TmpMntOrtho/Z\_000\_DeZoom2\_LeChantier.tif  ModeOmbre=IgnE

mm3d Pims2Ply PIMs-QuickMac/ Out=Pims2ply.ply

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

Viabon, 22/23 juin

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

"mm3d" "Tapioca" "MulScale" "image\_.\*tif" "400" "800"

mm3d Tapas FraserBasic "image\_.\*tif" Out=FraserRelatif

mm3d AperiCloud "image\_.\*tif" Ori-FraserRelatif/

\*\*\*\*\*\*\*\*\*\*\*\*\*\*

mm3d OriConvert OriTxtInFile sommetsCamerasFromGPS\_SSCorr\_GeoC.txt Nav-GPS-RTL
ChSys=GeoC\@SysCoRTL.xml

mm3d GCPConvert AppInFile Coords\_GCP\_GeoC.txt Out=Reduced\_Appuis-RTL.xml
ChSys=GeoC\@SysCoRTL.xml

mm3d GCPConvert AppInFile Coords\_CP\_GeoC.txt Out=Reduced\_Ctrl-RTL.xml
ChSys=GeoC\@SysCoRTL.xml

\*\*\*\*\*\*\*\*\*\*\*\*\*\*

mm3d CenterBascule "image\_.\*tif" Ori-FraserRelatif/ Ori-Nav-GPS-RTL/
BasculeBruteFraserOnGps

mm3d GCPCtrl "image\_.\*tif" BasculeBruteFraserOnGps Reduced\_Ctrl-RTL.xml
MesuresImages.xml

\*\*\*\*\*\*\*\*\*\*\*\*\*\*

mm3d Campari "image\_.\*.tif" BasculeBruteFraserOnGps CampariOnGps
EmGPS=[Nav-GPS-RTL,1,3]

mm3d GCPCtrl "image\_.\*tif" CampariOnGps Reduced\_Ctrl-RTL.xml
MesuresImages.xml

\*\*\*\*\*\*\*\*\*\*\*\*\*\*

mm3d Campari “image\_002\_00.\*tif" Ori-BasculeFraserOnGps/ CampariOnGpsBra
EmGPS=[Nav-GPS-RTL,1,3] GpsLa=[0,0,0]

mm3d GCPCtrl "image\_.\*tif" CampariOnGpsBra Reduced\_Ctrl-RTL.xml
MesuresImages.xml

\*\*\*\*\*\*\*\*\*\*\*\*\*\*

mm3d Campari "image\_.\*.tif" BasculeBruteFraserOnGps CampariOnGpsBraGCP
EmGPS=[Nav-GPS-RTL,1,3] GCP=[Reduced\_Appuis-RTL.xml,0.1,MesuresImages.xml,1]

mm3d GCPCtrl "image\_.\*tif" CampariOnGpsBraGCP Reduced\_Ctrl-RTL.xml
MesuresImages.xml

\*\*\*\*\*\*\*\*\*\*\*\*\*\*

mm3d SaisieMasqQT  

mm3d PIMs QuickMac "image\_.\*tif" CampariOnGpsBraGCP
Masq3D=AperiCloud\_CampariOnGpsBraGCP\_polyg3d.xml

mm3d PIMs2Mnt QuickMac DoOrtho=1

mm3d to8Bits PIMs-TmpBasc/PIMs-Merged\_Prof.tif Circ=1 Dyn=5
Mask=PIMs-TmpBasc/PIMs-Merged\_Masq.tif Out=MntCol.tif

mm3d GrShade PIMs-TmpBasc/PIMs-Merged\_Prof.tif FZ=5
Mask=PIMs-TmpBasc/PIMs-Merged\_Masq.tif Out=Shade.tif

mm3d Tawny PIMs-ORTHO/ Out=Orthophotomosaic.tif

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* CYLINDRE 23 juin
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

\# information exif manquante

mm3d SetExif "Calib-FG0A0.\*"  F=100 F35=100 Cam="Canon EOS 5D Mark III"

mm3d SetExif "Cyl.\*"  F=100 F35=100 Cam="Canon EOS 5D Mark III"

mm3d SetExif "Calib-IMG.\*"  F=20 F35=31.7 Cam="Canon EOS 100D"

\#   ============================== BANC DE CALIBRATION ====================

\# Points homologues sur le banc de calibration

Tapioca All Calib-.\*jpg 1000

\# =\>  6 minutes

\# Orientation des images courtes focales sur le banc

Tapas FraserBasic "Calib-IMG.\*jpg"  RefineAll=0 Out=Calib-CF

\# =\> 1 minutes

\# Orientation de toutes les images de calibration

Tapas FraserBasic "Calib-.\*jpg"  RefineAll=0 InOri=Calib-CF Out=All-Calib

\# =\> 3 minutes

\#   ============================== CYLINDRE ====================

\#   \*\*\*\*\*\*\*\*\* Orientation \*\*\*\*\*\*\*\*\*\*\*

\# Points homologues sur la chapelle

Tapioca Line  Cyl\_03.\*jpg 1500 7

\# =\> 3 minutes

\# Orientation avec Tapas

Tapas FraserBasic Cyl\_03.\* InCal=Ori-All-Calib/ Out=TapasCyl

\# =\> 3 minutes

AperiCloud Cyl\_03.\* Ori-TapasCyl/

\# Orientation avec Martini

mm3d Martini Cyl\_03.\*jpg OriCalib=Ori-All-Calib/

AperiCloud Cyl\_03.\*.jpg Ori-MartiniAll-Calib/

mm3d Campari "Cyl\_03.\*jpg" Ori-MartiniAll-Calib/ Comp1 AllFree=1

AperiCloud  Cyl\_03.\*jpg Ori-Comp1/

\#   \*\*\*\*\*\*\*\*\* estimation du cylindre \*\*\*\*\*\*\*\*\*\*\*

\# calcul de nuage pour estimer un cylindr

SaisieMasq  Cyl\_0332.jpg

mm3d Malt GeomImage "Cyl\_03(28\|32\|36).jpg"  Ori-Comp1/ Master=Cyl\_0332.jpg

SaisieMasq  Cyl\_0341.jpg

mm3d Malt GeomImage "Cyl\_03(37\|41\|44).jpg"  Ori-Comp1/ Master=Cyl\_0341.jpg

\# calcul du cylindre

mm3d CASA MM-Malt-Img-Cyl\_0332/NuageImProf\_STD-MALT\_Etape\_8.xml
 N2=MM-Malt-Img-Cyl\_0341/NuageImProf\_STD-MALT\_Etape\_8.xml

\# visualisation

mm3d TestLib San2Ply  Cyl\_03.\*jpg Ori-Comp1/ TheCyl.xml

\#   \*\*\*\*\*\*\*\*\* Appariement \*\*\*\*\*\*\*\*\*\*\*

mm3d SaisieMasqQT AperiCloud\_Comp1.ply

mm3d PIMs MicMac  Cyl\_03.\*jpg Ori-Comp1/ Masq3D=AperiCloud\_Comp1.ply

mm3d PIMs2Mnt MicMac Repere=TheCyl.xml  DoOrtho=1

Tawny PIMs-ORTHO/

GrShade PIMs-TmpBasc/PIMs-Merged\_Prof.tif
 Mask=PIMs-TmpBasc/PIMs-Merged\_Masq.tif ModeOmbre=IgnE FZ=2

to8Bits PIMs-TmpBasc/PIMs-Merged\_Prof.tif
 Mask=PIMs-TmpBasc/PIMs-Merged\_Masq.tif Dyn=6 Circ=1

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

Pleiades tristereo, 23 juin

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

mm3d Convert2GenBundle
IMG\_PHR1A\_P\_201202250026276\_SEN\_PRG\_FC\_5109-001\_R1C1.tif
RPC\_PHR1A\_P\_201202250026276\_SEN\_PRG\_FC\_5109-001.XML RPC
ChSys=WGS84toUTM.xml Degre=0

mm3d Convert2GenBundle
IMG\_PHR1A\_P\_201202250025599\_SEN\_PRG\_FC\_5108-001\_R1C1.tif
RPC\_PHR1A\_P\_201202250025599\_SEN\_PRG\_FC\_5108-001.XML RPC
ChSys=WGS84toUTM.xml Degre=0

mm3d Convert2GenBundle
IMG\_PHR1A\_P\_201202250025329\_SEN\_PRG\_FC\_5110-001\_R1C1.tif
RPC\_PHR1A\_P\_201202250025329\_SEN\_PRG\_FC\_5110-001.XML RPC
ChSys=WGS84toUTM.xml Degre=0

mm3d Campari ".\*.tif" RPC RPC-adj

mm3d Malt UrbanMNE .\*.tif Ori-RPC-adj/ ZMoy=0 ZInc=500 ZoomF=4
