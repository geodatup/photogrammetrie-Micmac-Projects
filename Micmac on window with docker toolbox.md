# on window with docker toolbox

mount pictures data and set path to bin micmac

docker stop micmac_image
docker rm  micmac_image
~~~

docker run --volume  //c/Users/Yogis/Pictures/drone/Labruguiere/qx1-16mm://home --name micmac_image -e PATH="$PATH:/micmac/bin"  -i -t geodatup/micmac /bin/bash
~~~



docker run --volume  //c/Users/Yogis/Pictures/drone/Labruguiere/5D-50mm-mur_mairie://home --name micmac_image -e PATH="$PATH:/micmac/bin"  -i -t geodatup/micmac /bin/bash
~~~

