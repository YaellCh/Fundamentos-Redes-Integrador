# Fundamentos-Redes-Integrador
Este repositorio está destinado a documentar los pasos que usaremos en el proyecto integrador en la materia de Fundamentos de Redes. Contiene una guía, configuraciones y recursos útiles para facilitar el desarrollo y comprensión del proyecto.

# Integrantes del Equipo

- **Chávez Atanacio Yael Antonio**    - [230110032]
- **Gómez Mejía Eduardo**                 - [230110727]
- **Hernández Martínez Brayan**     - [230110578]
- **Bernal Franco Lizbeth de Jesús** - [230110346]
- **Gress Ugarte María Guadalupe** - [220110989]

# Configuración del switch 
| Ip                             | Dispositivo                        | Ip sugerida                                                                 |
|--------------------------------|------------------------------------|------------------------------------------------------------------------------|
| 172.16.0.128 ID                | Router (RE)                        | 172.16.0.129  (Esta IP es nuestro GATEWAY)                                  |
| 172.16.0.158 BR                | Switch (SE)                        | 172.16.0.158 (Administración (última IP útil))                              |
| 255.255.255.224 Mask           | PC1                                | 172.16.0.130                                                                 |
|                                | PC2                                | 172.16.0.131                                                                 |
|                                | PC3                                | 172.16.0.132                                                                 |
|                                |                                    | **NOTA:** A partir de la IP con terminación .130 podemos asignar dispositivos hasta la .157 |

## Configurar Switch

1. Seleccionar la PC y el Switch para hacer la conexión.


![Conexión del Switch(SE) con las computadoras](Imgs/Conexion_PC_SE.png)

![Topología de red](Imgs/Topologia_SE.png)
### Pasos para configurar:
1. Configurar `hostname` y `banner`
2. Configurar `password` y `secret`
3. Colocar contraseña a la consola
4. Colocar contraseña a la conexión Telnet
5. Configurar VLAN
6. Configurar SSH

# Activar soporte para IPV6
```bash
Switch>enable
Switch#configure terminal
Switch(config)#sdmpreferdual-ipv4-and-ipv6default
Switch(config)#end
Switch#reload
```
Después de reiniciar, continúas con la configuración:

## Comandos para configurar el Switch
# Configuración básica del Switch
```bash
Switch>enable
Switch#configure terminal
Switch(config)#hostname SE
SE(config)#banner motd "Welcome to the switch"
SE(config)#enable password consola
SE(config)#enable secret tics
SE(config)#line console 0
SE(config-line)#password cisco
SE(config-line)#login
SE(config-line)#exit
SE(config)#line vty 0 15
SE(config-line)#password telnet
SE(config-line)#login
SE(config-line)#exit
```
# Configuración de VLAN 1 con IPv4 e IPv6
```bash
 SE(config)#interface vlan 1
 SE(config-if)#ip address 172.16.0.158 255.255.255.224
 SE(config-if)#ipv6 address 2001:db8:1:1::/64eui-64
 SE(config-if)#ipv6 address FE80::2 link-local
 SE(config-if)#no shutdown
 SE(config-if)#description"to admin"
 SE(config-if)#exit
```
# Configuración de SSH
```bash
 SE(config)#ip domain-name cisco.com
 SE(config)#username Admin password Admin
 SE(config)#crypto key generate rsa
 How many bits in the modulus [512]:1024
 SE(config)#linevty015
 SE(config-line)#transport input ssh
 SE(config-line)#login local
 SE(config-line)#exit
 SE(config)#service password-encryption
```
![](Imgs/Conf_Basic.png)
![](Imgs/Conf_Basic2.png)
## Configuración IPv4 en PCs

| Dispositivo | IPv4 Address   | Default Gateway  |
|-------------|----------------|------------------|
| PC1         | 172.16.0.130   | 172.16.0.1       |
| PC2         | 172.16.0.131   | 172.16.0.1       |
| PC3         | 172.16.0.132   | 172.16.0.1       |

## Configuración IPv6 en PCs

| Dispositivo | IPv6 Address              | Link-local Address |
|-------------|---------------------------|--------------------|
| PC1         | 2001:db8:11::101/64       | FE80::101          |
| PC2         | 2001:db8:11::102/64       | FE80::102          |
| PC3         | 2001:db8:11::103/64       | FE80::103          |

## Configuración en Packet Tracer y Pruebas de conectividad
![](Imgs/Cambiar_Ip.png)
Ingresa a la PCI, luego dirigete a Desktop y cambia las IP según las tablas de configuración IPV4 y
IPV6 deberas repetir este mismo paso en las demas PCs.

# Conexión en telnet por IPV4:
![](Imgs/Telnet_1.png)

![](Imgs/Telnet_2.png)

# Conexión en telnet por IPV6:
![](Imgs/Telnet_3.png)

# Conexión SSH usando IPV4
![](Imgs/ssh_IPV4.png)

# Conexión SSH usando IPV6
![](Imgs/ssh_IPV6.png)

## Configuraciones en el Router
![Topología de red](Imgs/topologia.png)

### Configuración básica

| Acción                                | Comando                             |
|--------------------------------------|-------------------------------------|
| Acceso al modo privilegiado          | `enable`                            |
| Modo configuración global            | `configure terminal`                |
| Asignar nombre al router             | `hostname RE`                       |
| Cifrado de contraseñas               | `service password-encryption`       |
| Mensaje de bienvenida                | `banner motd #Bienvenido#`          |

### Configurar consola

```bash
line console 0
password consola
login
exit
```

### Configurar acceso remoto (VTY 0–4)

```bash
line vty 0 4
password cisco
login
transport input all
exit
```
`Nota: El comando transport input ssh telnet puede no funcionar en versiones antiguas del IOS. Usar transport input all`

![Configuraciones Basicas](Imgs/configuraciones_basicas.png)

### Configuración de interfaces (IPv4 / IPv6)

**Parámetros:**
- IP y Máscara: `172.16.0.128 / 27` → `255.255.255.224`
- Rango IPs útiles: `172.16.0.129` a `172.16.0.157`
- Prefijo IPv6 Global: `2001:db8:1:1::/64`
- IPv6 link-local: Automática

| Acción                                  | Comando                                                     |
|----------------------------------------|-------------------------------------------------------------|
| Habilitar enrutamiento IPv6            | `ipv6 unicast-routing`                                     |
| Acceder a la interfaz g0/0             | `interface g0/0`                                           |
| Asignar dirección IPv4 y máscara       | `ip address 172.16.0.129 255.255.255.224`                  |
| Asignar dirección IPv6 con EUI-64      | `ipv6 address 2001:db8:1:1::/64 eui-64`                     |
| Activar IPv6 link-local automática     | `ipv6 enable`                                              |
| Añadir descripción a la interfaz       | `description "to LAN E"`                                   |
| Activar la interfaz                    | `no shutdown`                                              |
| Salir de la configuración de interfaz  | `exit`                                                     |

![Configuraciones de Interfaces](Imgs/configuraciones_interfaces.png)

### Configuración de los dispositivos finales (host)

![PC1](Imgs/PC1.png)

![PC2](Imgs/PC2.png)

![PC3](Imgs/PC3.png)

### Ping entre las interfaces 

1. Hacer ping desde PC1, PC2 y PC3 a:

- Dirección IPv4 del router: 172.16.0.129

- Dirección link-local IPv6 del router: FE80::260:3EFF:FE47:9401

- Dirección global IPv6 del router: 2001:DB8:1:1:260:3EFF:FE47:9401

2. Verificar IPv6 con el comando:

```bash
show ipv6 interface g0/0
```
`Este comando muestra información detallada de la configuración IPv6 de la interfaz especificada, 
en este caso GigabitEthernet0/0.
`

![Comando](Imgs/show.png)

3. Resultados

- Ping a la dirección IPv4 del router 

![1. Ping a la dirección IPv4 del router (172.16.0.129)](Imgs/ping_ipv4.png)  

- Ping a la dirección link-local IPv6 del router 
  
![2. Ping a la dirección link-local IPv6 del router (FE80::260:3EFF:FE47:9401)](Imgs/ping_ipv6_link-local.png)  

- Ping a la dirección global IPv6 del router 

![3. Ping a la dirección global IPv6 del router (2001:DB8:1:1:260:3EFF:FE47:9401)](Imgs/ping_ipv6_global.png)  





