# Fundamentos-Redes-Integrador
Este repositorio está destinado a documentar los pasos que usaremos en el proyecto integrador en la materia de Fundamentos de Redes. Contiene una guía, configuraciones y recursos útiles para facilitar el desarrollo y comprensión del proyecto.

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
   (agregar imagenes)

### Pasos para configurar:
1. Configurar `hostname` y `banner`
2. Configurar `password` y `secret`
3. Colocar contraseña a la consola
4. Colocar contraseña a la conexión Telnet
5. Configurar VLAN
6. Configurar SSH

## Configuraciones en el Router
 (agregar imagenes)
 
### Topología

### 1. Configuración básica del router

La configuración básica incluye estos pasos:

| **Acción**                                         | **Comando**                                 |
|---------------------------------------------------|---------------------------------------------|
| Acceso al modo privilegiado                       | `enable`                                    |
| Entrar al modo de configuración global            | `configure terminal`                        |
| Asignar un nombre al router (hostname)            | `hostname RE`                               |
| Establecer contraseña para modo privilegiado      | `enable secret tics`                        |
| Configurar contraseña para la consola             | `line console 0`<br>`password consola`<br>`login`<br>`exit` |
| Configurar acceso remoto (VTY 0–4)                | `line vty 0 4`<br>`password cisco`<br>`login`<br>`transport input all`<br>`exit` |
| Cifrado de contraseñas en el archivo de configuración | `service password-encryption`           |
| Mensaje de bienvenida (banner)                    | `banner motd #Bienvenido#`                  |

### Configuración básica

(imagenes y tabla)

### Configuración de interfaces

**Parámetros:**
- IP y Máscara: `172.16.0.128 / 27` → `255.255.255.224`
- Rango IPs útiles: `172.16.0.129` a `172.16.0.157`
- Prefijo IPv6 Global: `2001:db8:1:1::/64`
- IPv6 link-local: Automática

| Acción                                  | Comando                                                     |
|----------------------------------------|-------------------------------------------------------------|
| Habilitar enrutamiento IPv6            | `ipv6 unicast-routing`                                      |
| Acceder a interfaz g0/0                | `interface g0/0`                                            |
| Asignar dirección IPv4 y máscara       | `ip address 172.16.0.129 255.255.255.224`                   |
| Asignar dirección IPv6 (EUI-64)        | `ipv6 address 2001:db8:1:1::/64 eui-64`                      |
| Activar IPv6 link-local automática     | `ipv6 enable`                                               |
| Activar interfaz                       | `no shutdown`                                               |
| Descripción de la interfaz             | `description "to LAN E"`                                    |

---

## Configuraciones finales (Hosts)

1. Ping desde PC1, PC2 y PC3 a:
   - Dirección IPv4 del router: `172.16.0.129`
   - Dirección link-local IPv6 del router: `FE80::260:3EFF:FE47:9401`
   - Dirección global IPv6 del router: `2001:DB8:1:1:260:3EFF:FE47:9401`

2. Verificación con comando:
   - `show ipv6 interface g0/0`









