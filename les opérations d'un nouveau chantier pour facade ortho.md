nouveau chantier

##Preparrer les images
Si plusieurs focales, classer les images par focale et/ou par APN

lancer exiftool 
sur windows :

~~~
C:\>"exiftool(-k).exe" "C:\Users\Yogis\Desktop\orthomosaic Rouyre\etable bergerie\24mm\IMG_4051.JPG"
~~~

noter 
pour un canon 5D
~~~
Canon Image Width               : 5616
Canon Image Height              : 3744
~~~

bien séparer les images de calibration dans un dossier sans espaces !!

##Calibrer sa camera
### Lancer	Redresseur
- Créer	un	chantier	(Fichier	à Nouveau)
  - Charger  les5images(Fichierà Images)
    - Faire glisser la souris sur les images, Clic droit	 puis	 cocher	 Créer	 une nouvelle caméra (lui donner un nom en cliquant	dans le	champ	réservé	à	cet	effet :	eos5D_24mm)
- vérifierqueleschampslargeurhauteurfocale ont été correctement remplis
- Sauvegarderlechantier(Fichier à Enregistrer), et quiter Redresseur


### Lancer Match (présent dans le dossier Redresseur)

Ouvrir	le	chantier	Redresseur	préalablement	créé
- Cliquer	sur	« Calcul	des	points »
  - Cliquer sur	 « Calcul »	 pour	 lancer	 la	 mesure	 automatique	 des	 points	 de	 liaison,	 qui	 peut	prendre	quelques	minutes

~~~
calib_24mm\IMG_4120 - calib 24mm\IMG_4121    1956
calib_24mm\IMG_4119 - calib 24mm\IMG_4121    1228
--- Terminé ---  3' 8"
~~~

- Fermer Match

# Troubleshooting

A l'ouverture d'un fichier .red issue d'une calibration faite par Match, redresseur indique :
```le fichier n'existe pas ```
Faire attention à ne pas mettre d'espace dans le nom du dossier contenant les images ou la cam.