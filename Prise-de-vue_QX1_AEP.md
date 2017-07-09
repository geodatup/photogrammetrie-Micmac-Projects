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
3400sec

mm3d Tapas RadialStd "DSC003(71|72|73|81|82).ARW" Out='Ini'
100s

mm3d Tapas RadialStd ".*.ARW" InOri=Ini Out=Allori
400

mm3d AperiCloud ".*.ARW" Allori
400

mm3d Malt UrbanMNE ".*.ARW" Allori  ImOrtho=".*.ARW" DirMEC="Malt-UrbanMNE" DirOF="Ortho-Malt-UrbanMNE" DefCor=0.0
101700 sec = 28h (n'a pas créée de dossier Ortho pour Tawny)



mm3d Malt UrbanMNE ".*.ARW" Allori  ImOrtho=".*.ARW" DirMEC="MEC-Malt-UrbanMNE" DefCor=0.0

mm3d Malt Ortho ".*.ARW" Allori  ImOrtho=".*.ARW" ZoomF=4 ResolOrtho=0.6 DirMEC="MEC-Malt-Ortho" 
7000 sec (a bien créée le dossier ortho)

Tawny Ortho-MEC-Malt

mm3d C3DC BigMac ".*.ARW" Allori
11300sec

Ou PIMS

mm3d PIMs2Mnt BigMac DoOrtho=1 
1480 sec

mm3d Tawny PIMs-ORTHO/   Out=Orthophotomosaic.tif
111 sec