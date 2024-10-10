# OSPF

## Topología

```
PID (AS) 10
+-------------------------------------------------------------------------------------------------+
| AREA 0                                          AREA 1                                          |
| +-----------------------------------------------+---------------------------------------------+ |
| |                                   10.0.4.2/24 |                                             | |
| |                                       +-----+ |                                             | |
| |                                       | PC2 | |     Ethernet                                | |
| |  Ethernet                             +-----+ |     0/0 10.0.2.1/24                         | |
| |  0/0 10.0.0.1/24                           |  |     0/1 10.0.0.2/24                         | |
| |  0/1 10.0.1.1/24              10.0.4.0/24  |  |     0/2 10.0.4.1/24                         | |
| |  0/2 10.0.3.1/24                          0/2 |     1/0 10.10.0.1/24                        | |
| |  +------+           10.0.0.0/24          +---------+                                        | |
| |  | IOU1 |0/0--------------------------0/1| IOU2    |                                        | |
| |  +------+                                +---------+                                        | |
| |  0/2   0/1                              0/0   |   1/0                                       | |
| |   |     |           +------+             |    |    |            +------+                    | |
| |   |     +--------0/0| IOU3 |0/1----------+    |    +---------1/1| IOU4 |                    | |
| |   |   10.0.1.0/24   +------+      10.0.2.0/24 |  10.1.0.0/24    +------+                    | |
| |   |                0/2     0/0 10.0.1.2/24    |                0/2      Ethernet            | |
| |   |10.0.3.0/24      |      0/1 10.0.2.2/24    |                 |       0/2 10.1.1.1/24     | |
| |   |                 |      0/2 10.0.5.1/24    |                 |       1/1 10.1.0.2/24     | |
| | +-----+             |      Ethernet           |      10.1.1.0/24|                           | |
| | | PC1 |             |                         |                 |                           | |
| | +-----+             |10.0.5.0/24              |                 |                           | |
| | 10.0.3.2/24      +-----+                      |             +------+                        | |
| |                  | PC3 |10.0.5.2/24           |  10.1.1.2/24| PC4  |                        | |
| |                  +-----+                      |             +------+                        | |
| +-----------------------------------------------+---------------------------------------------+ |
+-------------------------------------------------------------------------------------------------+
```

### Redes

- 10.0.0.0/8 - AS 10
    - 10.0.0.0/16 - AREA 0
        - 10.0.0.0/24 - IOU1,IOU2
        - 10.0.1.0/24 - IOU1,IOU3
        - 10.0.2.0/24 - IOU2,IOU3
        - 10.0.3.0/24 - IOU1,PC1
        - 10.0.4.0/24 - IOU2,PC2
        - 10.0.5.0/24 - IOU3,PC2
    - 10.1.0.0/16 - AREA 1
        - 10.1.0.0/24 - IOU2,IOU4
        - 10.1.1.0/24 - IOU4,PC4



## Configuracion

IOU1:
```
configure terminal
interface Ethernet 0/0
no shutdown
ip address 10.0.0.1 255.255.255.0
exit

interface Ethernet 0/1
no shutdown
ip address 10.0.1.1 255.255.255.0
exit

interface Ethernet 0/2
no shutdown
ip address 10.0.3.1 255.255.255.0
exit

router ospf 10
network 10.0.0.0 0.0.0.255 area 0
network 10.0.1.0 0.0.0.255 area 0
network 10.0.3.0 0.0.0.255 area 0
end
```

IOU2 (nexo):
```
configure terminal
interface Ethernet 0/0
no shutdown
ip address 10.0.0.1 255.255.255.0
exit

interface Ethernet 0/1
no shutdown
ip address 10.0.1.1 255.255.255.0
exit

interface Ethernet 0/2
no shutdown
ip address 10.0.3.1 255.255.255.0
exit

interface Ethernet 1/0
no shutdown
ip address 10.1.0.1 255.255.255.0
exit

router ospf 10
network 10.0.0.0 0.0.0.255 area 0
network 10.0.1.0 0.0.0.255 area 0
network 10.0.4.0 0.0.0.255 area 0
network 10.1.0.0 0.0.0.255 area 1
end
```

IOU3:
```
configure terminal
interface Ethernet 0/0
no shutdown
ip address 10.0.1.2 255.255.255.0
exit

interface Ethernet 0/1
no shutdown
ip address 10.0.2.2 255.255.255.0
exit

interface Ethernet 0/2
no shutdown
ip address 10.0.5.1 255.255.255.0
exit

router ospf 10
network 10.0.0.0 0.0.0.255 area 0
network 10.0.1.0 0.0.0.255 area 0
network 10.0.5.0 0.0.0.255 area 0
end
```

IOU4:
```
configure terminal

interface Ethernet 0/2
no shutdown
ip address 10.1.1.1 255.255.255.0
exit

interface Ethernet 1/1
no shutdown
ip address 10.1.0.2 255.255.255.0
exit

router ospf 10
network 10.1.0.0 0.0.0.255 area 1
network 10.1.1.0 0.0.0.255 area 1
end
```

PC1:
```
ip 10.0.3.2 255.255.255.0 10.0.3.1
```

PC2:
```
ip 10.0.4.2 255.255.255.0 10.0.4.1
```

PC3:
```
ip 10.0.5.2 255.255.255.0 10.0.5.1
```

PC4:
```
ip 10.1.1.2 255.255.255.0 10.1.1.1
```
