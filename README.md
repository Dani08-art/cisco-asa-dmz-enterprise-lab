# Cisco ASA Lab - Red interna jerárquica + DMZ + ISP

## Descripción

Este laboratorio fue diseñado para practicar **Cisco ASA** dentro de una topología tipo empresa pequeña, integrando una **red interna jerárquica**, una **DMZ**, salida hacia un **ISP**, servicios internos y controles básicos de seguridad.

El objetivo principal fue entender cómo se relacionan **switching, routing, servicios de red y seguridad perimetral** en un mismo escenario, especialmente en la parte de firewall con ASA, ya que era la primera vez que lo configuraba de forma práctica.

---

## Topología

Topología del laboratorio:

![Topología del laboratorio](sandbox:/mnt/data/30bec70b-3cc8-4de5-8779-f24bd5b6181a.png)

También puedes verla directamente aquí:  
[Ver topología](sandbox:/mnt/data/30bec70b-3cc8-4de5-8779-f24bd5b6181a.png)

---

## Objetivos del laboratorio

- Practicar configuración básica e intermedia de **Cisco ASA**
- Implementar una red interna con diseño jerárquico
- Separar servicios públicos en una **DMZ**
- Simular conectividad hacia Internet mediante un **ISP**
- Aplicar segmentación por VLANs
- Implementar servicios como **DHCP, DNS y WEB**
- Controlar acceso mediante **ACLs**
- Configurar **NAT/PAT** y **Static NAT**
- Restringir la administración del firewall por **SSH** solo desde la VLAN de TI

---

## Dispositivos y funciones

### Capa de acceso / distribución
- **DSW1, DSW2, DSW3**
  - Creación de VLANs
  - Asignación de puertos de acceso
  - Port-security
  - SSH

- **DSW4**
  - Segmento donde se ubica el servidor DHCP interno

### Core
- **Core-SW-BOG**
  - SVIs
  - IP Routing
  - STP
  - DHCP relay

### Enrutamiento
- **Router-BOG**
  - Enrutamiento entre la red interna y ASA

- **R-ISP**
  - Simulación de conectividad hacia Internet

### Seguridad perimetral
- **ASA-BOG**
  - Interfaces por zonas:
    - `inside`
    - `dmz`
    - `outside`
  - NAT dinámico / PAT para salida de la red interna
  - Static NAT para publicación de servicios
  - ACLs por interfaz
  - MPF (Modular Policy Framework)
  - Restricción de administración SSH

### DMZ
- **DMZ-SW-BOG**
  - Switch para conectar los servidores publicados

- **Servidores en DMZ**
  - DNS
  - WEB

### Red inalámbrica
- **AP WiFi**
  - VLAN 40
  - WPA2-AES

---

## Segmentación lógica

La red fue dividida en varias VLANs para separar funciones y usuarios:

- **VLAN 10** - Sales
- **VLAN 11** - Engineering
- **VLAN 20** - Guest
- **VLAN 21** - TI
- **VLAN 30** - Admin
- **VLAN 40** - WiFi
- **VLAN 50** - DHCP Server

> La administración por **SSH hacia el ASA** fue permitida únicamente desde la **VLAN TI**.

---

## Tecnologías y conceptos practicados

- VLANs
- Trunking
- STP
- SVIs
- Inter-VLAN routing
- DHCP
- DHCP relay
- SSH
- Port-security
- Routing estático
- Cisco ASA security levels
- ACLs en ASA
- NAT/PAT
- Static NAT
- MPF
- DMZ
- Publicación de servicios
- WiFi con WPA2-AES

---

## Configuración implementada en ASA

Entre los puntos trabajados en el firewall se configuró:

- **Interfaces y zonas**
  - `inside`
  - `dmz`
  - `outside`

- **NAT**
  - PAT para salida de la red interna hacia Internet
  - Static NAT para publicación de servicios específicos

- **ACLs**
  - Políticas aplicadas por interfaz
  - Permisos para ICMP, DNS y HTTP según necesidad

- **MPF**
  - Inspección de tráfico como:
    - DNS
    - FTP
    - HTTP
    - ICMP

- **Acceso administrativo**
  - SSH solo permitido desde la red de TI

---

## Lo que funcionó correctamente

✅ Comunicación entre segmentos internos  
✅ Funcionamiento de VLANs y switching  
✅ Enrutamiento interno mediante SVIs en el core  
✅ DHCP para clientes internos  
✅ DHCP relay  
✅ Salida de la red interna mediante PAT  
✅ Conectividad entre red interna, router y ASA  
✅ Segmentación DMZ  
✅ Restricción de SSH hacia ASA solo desde VLAN TI  
✅ Conectividad por ping hacia servidores DNS y WEB  

---

## Problema pendiente / aprendizaje

Aunque logré llegar por **ping** a los servidores **DNS** y **WEB**, no conseguí completar correctamente el acceso al **servidor web por dominio desde un PC cliente**.

### Qué intenté
- Permitir tráfico HTTP mediante ACL
- Validar conectividad IP hacia los servidores
- Probar resolución y acceso desde clientes

### Qué quedó pendiente por revisar
- Publicación completa del servidor WEB mediante **Static NAT**
- Validación de la **ACL en la interfaz correcta**
- Revisión de la resolución DNS y registros apuntando a la IP pública correcta
- Verificación del flujo completo:
  - cliente → DNS
  - resolución de nombre
  - acceso HTTP hacia la IP publicada del servidor web

### Observación técnica
Una de las hipótesis es que el servidor DNS sí fue publicado, pero el servidor WEB todavía requiere revisar mejor su **traducción estática** y/o su publicación correcta desde `outside`, para que el acceso por nombre funcione de extremo a extremo.

Este punto quedó como parte del aprendizaje del laboratorio y será el siguiente ajuste a trabajar.

---

## Aprendizajes principales

Este laboratorio me ayudó a entender mejor:

- La lógica de seguridad por zonas en ASA
- La diferencia entre permitir tráfico con ACL y realmente publicar un servicio correctamente
- Cómo conviven switching, routing, DHCP, DNS, NAT y firewalling en una misma topología
- La importancia de validar no solo conectividad IP, sino también resolución de nombres y flujo de aplicación

---

## Herramientas usadas

- Cisco Packet Tracer
- Cisco ASA 5506-X
- Switches Cisco 2960 / 3560
- Router Cisco 2911

---

## Autor

Laboratorio realizado como práctica personal para fortalecer conocimientos en:

- Networking
- Switching & Routing
- Cisco ASA
- Seguridad perimetral
- Diseño básico de red empresarial
