# Manual Técnico

### Aclaraciones Importantes:

Número de Grupo: 9 <br>
Carnet Kewin Patzán: 202103206 <br>
Carnet Mario Marroquín: 202110509 <br>

Suma = 6+9 = 15, Se reemplazará "x" con el número 5 en las VLAN. <br>
Al ser grupo impar, se implementó **RSTP-rapid pvst**. <br>

---

## **VLAN**

|Departamento|	VLAN ID|	Red IP|	Máscara de subred|
|------------|---------|----------|------------------|
|Ventas	        |15|	192.168.15.0|	255.255.255.0|
|Soporte	    |25|	192.168.25.0|	255.255.255.0|
|Gerencia       |35|	192.168.35.0|	255.255.255.0|
|Seguridad	    |45|    192.168.45.0|	255.255.255.0|
|Recepción	    |55|	192.168.55.0|	255.255.255.0|

---

## **DIRECCIONES IP**

### Ventas
| Nombre  | IP            |
|---------|--------------|
| Ventas1 | 192.168.15.2 |
| Ventas2 | 192.168.15.3 |
| Ventas3 | 192.168.15.4 |
| Ventas4 | 192.168.15.5 |
| Ventas5 | 192.168.15.6 |


### Soporte
| Nombre              | IP            |
|---------------------|--------------|
| Soporte Tecnico 1  | 192.168.25.2 |
| Soporte Tecnico 2  | 192.168.25.3 |
| Soporte Tecnico 3  | 192.168.25.4 |
| Soporte Tecnico 4  | 192.168.25.5 |
| Soporte Tecnico 5  | 192.168.25.6 |


### Gerencia
| Nombre    | IP            |
|-----------|--------------|
| Gerencia1 | 192.168.35.2 |
| Gerencia2 | 192.168.35.3 |
| Gerencia3 | 192.168.35.4 |
| Gerencia4 | 192.168.35.5 |
| Gerencia5 | 192.168.35.6 |


### Seguridad
| Nombre       | IP            |
|-------------|--------------|
| Seguridad 1 | 192.168.45.2 |
| Seguridad 2 | 192.168.45.3 |
| Seguridad 3 | 192.168.45.4 |
| Seguridad 4 | 192.168.45.5 |
| Seguridad 5 | 192.168.45.6 |
| Seguridad 6 | 192.168.45.7 |
| Seguridad 7 | 192.168.45.8 |


### Recepción
| Nombre      | IP            |
|------------|--------------|
| Recepcion 1 | 192.168.55.2 |
| Recepcion 2 | 192.168.55.3 |
| Recepcion 3 | 192.168.55.4 |
| Recepcion 4 | 192.168.55.5 |


---


## **IMPLEMENTACIÓN DE LAS TOPOLOGÍAS**

### Área Central
![areaCentral](/Proyecto1/imgs/central.png)

<br>

### Área de Administración
![areaCentral](/Proyecto1/imgs/administracion.png)

<br>

### Área de Desarrollo
![areaCentral](/Proyecto1/imgs/desarrollo.png)

<br>

### Área de Gerencia 
![areaCentral](/Proyecto1/imgs/gerencia.png)

<br>

### Área de Infraestructura 
![areaCentral](/Proyecto1/imgs/infraestructura.png)

<br>

### Topología Completa
![areaCentral](/Proyecto1/imgs/completa.png)

---


## **COMANDOS UTILIZADOS**


## 📌 Área Backbone

## Servidor

### Configuración de VLANs en el Servidor
```
vlan 15
name Ventas
exit

vlan 25
name Soporte
exit

vlan 35
name Gerencia
exit

vlan 45
name Seguridad
exit

vlan 55
name Recepcion
exit
```

### Configuración de Spanning Tree en el Servidor
```
enable 
configure terminal
spanning-tree mode rapid-pvst
spanning-tree vlan 15 root primary
spanning-tree vlan 25 root primary
spanning-tree vlan 35 root primary
spanning-tree vlan 45 root primary
```

### Configuración de Trunk en el Switch Servidor
```
enable 
configure terminal
interface range FastEthernet0/1 - 4
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
exit
```

### Verificación de Spanning Tree en el Servidor
```
Switch# show spanning-tree
```

---

## MSW1 - MSW10 (VTP CLIENTE)

### Configuración VTP en los Switches
```
vtp domain G9_technet
vtp mode client
vtp password secure2025
```

### Configuración de Trunk
```
interface range FastEthernet0/1 - 4
switchport trunk encapsulation dot1q
switchport mode trunk
```

---

## 📌 Área Administrativa

### Configuración en SW0 - SW4 y MSW5 a MSW7
```
enable
configure terminal
vtp domain G9_technet
vtp mode client
vtp password secure2025
```

#### Configuración de Trunk en los Switches
```
enable
configure terminal
interface FastEthernet0/1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
```

#### SW5 - Configuración de VLAN Recepción
```
enable
configure terminal
vtp mode transparent
vlan 55
name Recepcion
interface range FastEthernet0/1
switchport mode trunk
exit
```


## Acceso a las PC’s Área Administrativa

### Configuración en SW1
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 15
interface FastEthernet0/12
switchport mode access
switchport access vlan 35
```

### Configuración en SW2
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 45
```

### Configuración en SW3
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 25
```

### Configuración en SW4
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 15
interface FastEthernet0/12
switchport mode access
switchport access vlan 35
```

### Configuración en SW5
```
enable
configure terminal
interface range FastEthernet0/11-14
switchport mode access
switchport access vlan 55
```

---

## 📌 Área de Desarrollo

### Configuración de MSW11
```
enable
configure terminal
vtp domain G9_technet
vtp mode client
vtp password secure2025
interface range FastEthernet0/1 - 5
switchport trunk encapsulation dot1q
switchport mode trunk
```

### Configuración en SW8 - SW10
```
enable
configure terminal
vtp domain G9_technet
vtp mode client
vtp password secure2025
interface range FastEthernet0/2-5
switchport mode trunk
```


## Acceso a las PC’s Área Desarrollo

### Configuración en SW8
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 15
interface FastEthernet0/12
switchport mode access
switchport access vlan 45
```

### Configuración en SW9
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 45
interface FastEthernet0/12
switchport mode access
switchport access vlan 25
```

### Configuración en SW10
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 35
interface FastEthernet0/12
switchport mode access
switchport access vlan 25
interface FastEthernet0/13
switchport mode access
switchport access vlan 15
```


---



## 📌 Área de Gerencia

### Configuración en SW11, SW12 y SW13
```
enable
configure terminal
vtp domain G9_technet
vtp mode client
vtp password secure2025
interface range FastEthernet0/2-5
switchport mode trunk
```


## Acceso a las PC’s Área Gerencia

### Configuración en SW11
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 45
interface FastEthernet0/12
switchport mode access
switchport access vlan 25
```

### Configuración en SW12
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 35
```

### Configuración en SW13
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 35
```

---

## 📌 Área de Infraestructura

### Configuración en MSW8
```
enable
configure terminal
vtp domain G9_technet
vtp mode client
vtp password secure2025
interface range FastEthernet0/1 - 5
switchport trunk encapsulation dot1q
switchport mode trunk
```

### Configuración en SW6 y SW7
```
enable
configure terminal
vtp domain G9_technet
vtp mode client
vtp password secure2025
interface FastEthernet0/1
switchport mode trunk
```

## Acceso a las PC’s Área Infraestructura

### Configuración en SW6
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 25
interface FastEthernet0/12
switchport mode access
switchport access vlan 45
```

### Configuración en SW7
```
enable
configure terminal
interface FastEthernet0/11
switchport mode access
switchport access vlan 15
interface FastEthernet0/12
switchport mode access
switchport access vlan 45
```

---

## **PING ENTRE HOSTS**


















---

## **PRESUPUESTO DEL PROYECTO**





















----