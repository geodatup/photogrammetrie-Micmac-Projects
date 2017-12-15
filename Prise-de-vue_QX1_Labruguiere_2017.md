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
sous linux
~~~
PAT_1="(.*.JPG)"
PAT_2="(.*.ARW)"
PAT_3="(.*.(ARW|JPG))"
PAT="(.*.(ARW|CR2))"

~~~
sous windows
~~~
set PAT="(.*.(ARW|CR2))"
~~~
pour rappeler le pattern sous windows utilisez %PAT%

### Tapioca

~~~
mm3d Tapioca MulScale "$PAT" 300 1200 @SFS
~~~
~~~
mm3d Tapioca MulScale %PAT% 300 1200
~~~
Tapioca commence tout d'abord par écrire des fichiers xml et dmp associé à chaque image correspondant au pattern. Ces fichiers seront parfois à nettoyé surtout si vous utilisez différentes version de micmac sur un même projet (Windows/linux/mac).

Tapioca  créer ensuite plusieurs  fichiers images tiff. Les images sont stockées dans un dossier de travail nommé Tmp-MM-Dir. Elles seront utiisées à plusieurs reprises durant le workflow. Si vous travaillez sur un  jeu d'images utilisé dans plusieurs projets, le dossier Tmp-MM-Dir peut être réutilisée et Tapioca ne créera pas ces images une nouvelle fois. 

Voici un exemple de ce qui est dans les log
~~~
"c:/MicMac64bits/bin/mm3d" MpDcraw "./IMG_4830.CR2"  Add16B8B=0  ConsCol=0  ExtensionAbs=None  16B=0  GB=1  NameOut=./Tmp-MM-Dir/IMG_4830.CR2_Ch1.tif Gamma=1.0 EpsLog=1.0
BY FILE
NO FLAT FIELD Foc0500Diaph100-FlatField.tif
"c:/MicMac64bits/bin/mm3d" PastDevlop  ./IMG_4836.CR2 Sz1=300 Sz2=1200
~~~
On peut voir que MpDcraw et PastDevlop sont les commandes appelées par Tapioca.

Le dossier Pastis est aussi créé et les images des résolutions passées en parametre de la commande y sont stockées.

Ensuite le processus de recherche de point homologue peut commencer.

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


~~~	
mm3d Tapas $TAPAS_MDL $PAT_cal Out=iniCalib
~~~
mm3d AperiCloud ".*.(ARW|CR2|JPG)" Allori
~~~

temps de traitement : 13846 sec
=======

Resultats RadialExtended ARW :
~~~
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

Résultats : 

~~~
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


##Filtrer les resultats


mm3d HomolFilterMasq %PAT% OriMasq3D=Allori/

archiver l'ancier dossier Homol et renommer le dossier HomolFilterMasq en Homol
mv Homol HomolBKP 
mv HomolFilterMasq Homol

verifier vos resultats dans un nouveau nuage de point
mm3d AperiCloud %PAT% Allori

### création du masque sur Tarama 

Attention, trop de fichiers en entrée provoque une fin de programme (erreur mémoire ou autre...?)
~~~
mm3d Tarama ".*.ARW" Allori 
~~~


~~~
mm3d SaisieMasqQT TA/TA_LeChantier.tif
~~~

on utlilise JPG car erreur avec ARW
~~~
mm3d SaisieMasqQT "DSC00610.JPG" Attr=Plan
mm3d SaisieBascQT "DSC00610.JPG" Allori MesureBasc.xml
~~~

*Attention :* Renommer les points en Line1 et Line2 et changer l'extention de l'image en ARW
~~~
<?xml version="1.0" ?>
<SetOfMesureAppuisFlottants>
     <MesureAppuiFlottant1Im>
          <NameIm>DSC00610.ARW</NameIm>
          <OneMesureAF1I>
               <NamePt>Line1</NamePt>
               <PtIm>3155.0438103375977 2660.156069911201</PtIm>
          </OneMesureAF1I>
          <OneMesureAF1I>
               <NamePt>Line2</NamePt>
               <PtIm>2420.1313501378168 3375.2060311866639</PtIm>
          </OneMesureAF1I>
     </MesureAppuiFlottant1Im>
</SetOfMesureAppuisFlottants>
~~~

On effectue la bascule sur le plan

~~~
mm3d RepLocBascule ".*.ARW" Allori MesureBasc-S2D.xml RepereOrtho.xml PostPlan=_MasqPlan
~~~

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

ou  C3DC puis PIMS

~~~
mm3d C3DC QuickMac ".*.ARW" Allori
~~~ 


Mesh :

A partir de la sortie C3DC lancer tipunch

mm3d Tipunch C3DC_QuickMac.ply Pattern=.*.ARW

puis telquila sur un set d'image réduit.




#Troubleshooting
erreur sur fichier dmp
- supprimer les fichiers xml et dmp avec la commande suivante
rm Tmp-MM-Dir/*4227.*
## Restaurer les fichiers EXIF xml et dmp
mm3d MMXmlXif %PAT%