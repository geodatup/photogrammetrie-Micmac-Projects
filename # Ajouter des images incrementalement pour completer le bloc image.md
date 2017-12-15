# Ajouter des images incrementalement dans la recherche de point homologue
set PAT_1="(.*.CR2)"
set PAT_2="(.*.ARW)"
set PAT_1-2="(.*.(CR2|ARW))"
set PAT3="(IMG_4819.CR2|IMG_4837.CR2|IMG_4838.CR2|IMG_4840.CR2|IMG_4841.CR2|IMG_4842.CR2|IMG_4843.CR2|IMG_4844.CR2|IMG_4845.CR2|IMG_4849.CR2|IMG_4850.CR2|IMG_4851.CR2|IMG_4852.CR2)"
set PAT_23="(IMG_4819.CR2|IMG_4837.CR2|IMG_4838.CR2|IMG_4840.CR2|IMG_4841.CR2|IMG_4842.CR2|IMG_4843.CR2|IMG_4844.CR2|IMG_4845.CR2|IMG_4849.CR2|IMG_4850.CR2|IMG_4851.CR2|IMG_4852.CR2)|(.*.ARW)"

sous windows

mm3d Tapioca MulScale %PAT_1% 300 2000 @SFS

mm3d Tapas FraserBasic %PAT_1% InCal=iniCalib InOri=iniCalib Out=ARW_FraserBasic
*
mm3d Tapioca MulScale %PAT_2% 300 2000 @SFS
mm3d Tapioca All %PAT_1% 2000 Pat2=%PAT_2%


mm3d Tapas AutoCal %PAT_23%  InCal=iniCalib InOri=ARW_FraserBasic/


+ link

mm3d Tapas FraserBasic %PAT_2% InCal=iniCalib InOri=iniCalib Out=qx_FraserBasic


mm3d Tapioca MulScale %PAT_3% 300 2000 @SFS
mm3d Tapioca All %PAT_2% 2000 Pat2=%PAT_3%


sous linux :

PAT1="(.*.CR2)"
PAT2="(.*.ARW)"
PAT3="(IMG_4819.CR2|IMG_4837.CR2|IMG_4838.CR2|IMG_4840.CR2|IMG_4841.CR2|IMG_4842.CR2|IMG_4843.CR2|IMG_4844.CR2|IMG_4845.CR2|IMG_4849.CR2|IMG_4850.CR2|IMG_4851.CR2|IMG_4852.CR2)"
PAT23="(IMG_4819.CR2|IMG_4837.CR2|IMG_4838.CR2|IMG_4840.CR2|IMG_4841.CR2|IMG_4842.CR2|IMG_4843.CR2|IMG_4844.CR2|IMG_4845.CR2|IMG_4849.CR2|IMG_4850.CR2|IMG_4851.CR2|IMG_4852.CR2)|(.*.ARW)"

PAT_2B3="(IMG_4823.CR2|IMG_4824.CR2|IMG_4825.CR2)|(DSC006(25|26|27|28|29|30|31).ARW)"


mm3d Tapioca MulScale PAT1 300 2000 @SFS


+ link


mm3d Tapas FraserBasic $PAT2 InCal=iniCalib InOri=iniOri Out=qx_FraserBasic

mm3d Tapioca MulScale $PAT3 300 2000 @SFS
mm3d Tapioca All $PAT2 2000 Pat2=$PAT3

mm3d Tapas AutoCal $PAT23 InCal=iniCalib InOri=iniOri


mm3d AperiCloud $PAT23 AutoCal



Merging Orientation with Morito

---

mm3d Tapioca MulScale %PAT_2B3% 2000 4000 @SFS

Tapas FraserBasic "DSC006(25|26|27|28|29|30|31).ARW" Out=iniOri

mm3d Tapas Fraser %PAT_2B3% InCal=IniCalib InOri=iniOri Out=withCR2


mm3d AperiCloud %PAT_2B3% iniOri
