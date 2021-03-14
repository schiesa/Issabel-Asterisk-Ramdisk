Ramdisk en Asterisk
Pasos a Seguir:

1- Edito el fstab, para crear la particion de memoria RAM

nano /etc/fstab

/dev/ram0 /mnt/ramdisk tmpfs size=2048M 0 0

en este ejemplo la carpeta se esta /mnt/ramdisk y el size es 2048 megas

2- Creo el ejecutable en /root/scripts creamos en este ejemplo
una capeta en /audios, el ejecutable se encarga de mover los audios
de /mnt/ramdisk a /audios

nano /root/scripts/moveraudios.sh

#!/bin/sh
#
# RAMDISK Watcher
#
# Revisa el contenido del ram0 y lo pasa a disco duro

## Variables
RMDIR="/mnt/ramdisk/"
ALMACEN="/audios/"

for i in $(ls -1 $RMDIR/*.wav) ; do
lsof $RMDIR/$i &> /dev/null
valor=$?
if [ $valor -ne 0 ] ; then
mv $i $ALMACEN
fi
done

3- Colocar en el Crontab la siguente linea, en este ejemplo lo hace cada
1 min

# crontab -e
*/1 * * * * /root/scripts/moveraudios.sh

Para Hacer cambios en caliente

mount -t tmpfs tmpfs /mnt/ramdisk -o size=2G,mode=1777,remount
