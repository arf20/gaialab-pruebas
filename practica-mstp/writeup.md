# MSTP

## Requisitos

 - GNS3
 - vpcs
 - libc6:i386, libstdc++6:i386, libssl3:i386
 - /usr/lib/i386-linux-gnu/libcrypto.so.4 -> /usr/lib/i386-linux-gnu/libcrypto.so.3 (xd)
 - Imagen IOS on UNIX Cisco L2: [i86bi-linux-l2-upk9-15.0b.bin](https://mega.nz/#!9RkE2SbL!E231Iw8MawfWFDVN5JyhkrAhstx4jENu3N0rbd8jVCA)
 - Keygen licencia cisco: 
[CiscoIOUKeygen3f.py](http://www.ipvanquish.com/download/CiscoIOUKeygen3f.py)

## Arquitectura

### Enlaces

Triangulo formado por subtriangulos de switches (i86bi L2 15.0), en forrma de Sierpiński n=2, nombrados IOU{1-9}, siendo los vertices del triangulo [[1, 2, 3], [4, 5, 6], [7, 8, 9]]. Los switches subtriangulos estan interconectados con 3 links, y a su vez dos de ellos estan conectados al vertice adyacente tal como [2-4, 3-8, 6-9].

Dos PCs conectados a vertices distintos del triangulo grande: PC1 con IOU1, PC2 con IOU6.

### MSTP

#### Regiones 

Los subswitches de cada vertice del triangulo grande: [IOU{1-3}: "rega", IOU{4-6}: "regb", IOU{7-9}: "regc"]

#### Switch raiz

IOU1

## Configuracion

IOU1 [rega] (root):
```
configure
spanning-tree mode mst
spanning-tree mst configure
name rega
revision 1
exit
spanning-tree mst 0 root primary
exit
```

IOU{2-3} [rega]:
```
configure
spanning-tree mode mst
spanning-tree mst configure
name rega
revision 1
exit
```

IOU{4-6} [regb]:
```
configure
spanning-tree mode mst
spanning-tree mst configure
name regb
revision 1
exit
```

IOU{7-9} [regc]:
```
configure
spanning-tree mode mst
spanning-tree mst configure
name regc
revision 1
exit
```

PC1:
```
ip 10.0.0.1 255.255.255.0
```

PC2:
```
ip 10.0.0.2 255.255.255.0
```
