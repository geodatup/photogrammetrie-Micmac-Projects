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
poid fichier : 21/22mo

Calcul de la résolution au sol :

Qualité pour sol :

Qualité pour toiture :

Qualité maraichage :

Qualité foret :

### Tapiocas
~~~
mm3d Tapioca MulScale ".*.ARW" 300 1200
~~~
Analyse des résultats : 



### Tapas

Définition de qx1 dans dicocamera.xml

	<CameraEntry>
		<Name>  Sony ILCE-QX1 </Name>
		<SzCaptMm>   16.0 24.0  </SzCaptMm>
		<ShortName> QX1  </ShortName>
	</CameraEntry>

~~~	
mm3d Tapas RadialStd "DSC000(74|73|85|86|93|94).ARW" Out='Ini'
~~~
~~~
mm3d Tapas RadialStd ".*.ARW" InOri=Ini Out=Allori
~~~

### AperiCloud

~~~
mm3d AperiCloud "photo/.*.ARW" Allori
~~~

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
mm3d Malt Ortho ".*.ARW" Allori  ImOrtho=".*.ARW" DefCor=0.0


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
