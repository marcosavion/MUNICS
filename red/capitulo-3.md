# Capitulo 3

Funcionalmente un dispositivo de red tiene 3 planos:

* **Plano de gestión**
  * Trafico enviado y recibido por el dispositivo para su administración
  * SSH, telnet, SNMP
* **Plano de Control**
  * Está relacionado con la toma de decisiones de envío
  * Spanning-Tree-Protocol
  * HSRP, VRRP
* **Plano de datos** o plano de envío
  * Envío de datos de usuario
  * Implementación de las políticas de seguridad de tráfico de usuario

La seguridad de cada plano depende de su configuración y de la de los otros dos





## Seguridad en el plano de Control

### **Objetivos**

* Permite acceso solamente a usuarios autenticados
  * AAA
  * Usuarios locales
  * Contraseñas de linea
* Controlar que pueden hacer sus usuarios en base a sus privilegios
  * Uso de niveles de privilegios
  * AAA
* Cifrar las conexiones de control remota
  * SSHv2, SSL/TLS
* Monitorizar de forma segura:
  * _Syslog_
  * _SNMPv3 y SNMPv2_

### **Buenas prácticas**

* Reforzar directivas de contraseñas
  * Longitud mínima
  * Limitar el número de intentos de login por segundo
* Definir grupos de usuarios mediante Roles
* Utilizar redes diferenciadas para gestionar la infraestructura y restringir las IPs desde las que se pueden inciar sesiones de gestión.
* Deshabilitar servicios no necesarios: HTTP, HTTPS, DHCP, DNS...

## Seguridad en el plano de Gestión

### **Objetivos**

* Limitar el daño que un atacante podría hacer al interactuar con las IPs de un dispositivo
  * Saturar CPU
  * Existen tecnologías como CoPP (Control Plane Policing) y CPPr (Control Plane Protection) que ayudan a conseguir este objetivo
* Controlar la información relacionada con la toma de decisiones de envío
  * Protocolos de enrutamiento&#x20;
    * Autenticación de protocolos de enrutamiento
  * Protocolos de control de bucles de capa 2
    * Limitar el uso de BPDU

### **Buenas prácticas**

* Para proteger la CPU
  * Siempre que sea posible se enruta utilizando mecanismos de caché
  * En el caso de Cisco IOS: Cisco Express Forwarding
  * Si no es posible, finalmente lo enruta “por software” la CPU -> Consumo de recursos
* Para proteger el "camino de los datos"
  * Paquetes relacionados con la toma de decisiones de envío que son recibidos o enviados por los equipos de red (fundamentalmente relacionados con enrutamiento)
    * Consumo de CPU y memoria -> Mecanismo de control -> evitar DoS o DDoS&#x20;
    * Proteger frente a la inyección de información falsa -> autenticación
* **CoPP** (Control Plane Policing): Filtros para cualquier tráfico destinado a las IPs del router
  * Se puede aplicar al tráfico de gestión (e.g. SSH, HTTPS, SSL, SNMP):&#x20;
    * Definir una ratio máxima
    * Descartarlo completamente –
  * Evita que ataques basados en el envío masivo de tráfico no lleguen a la CPU  -> El tráfico se elimina previamente
  * Estas políticas se aplican a la interfaz lógica que conecta el plano de datos con el plano de control » Se aplica independientemente de por dónde entre el tráfico en el dispositivo



