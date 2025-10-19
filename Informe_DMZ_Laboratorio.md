# Informe de configuración de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

> Tiene como objetivo desarrollar competencias en ciberseguridad defensiva mediante la implementación de una Zona Desmilitarizada (DMZ) en Cisco Packet Tracer.
A través de la configuración de routers, switches, servidores y ACLs, se aprenderá a aislar servicios críticos, controlar el tráfico entre redes internas y externas, y evitar exponer servicios públicos de manera segura y controlada.

**Ejemplo:**  
Configurar una DMZ segura usando un router Cisco ISR, aplicando NAT y ACLs para controlar el tráfico entre LAN, DMZ y red externa.

---

### 2. Topologí­a implementada
<img width="970" height="512" alt="image" src="https://github.com/user-attachments/assets/65ca8f9c-7653-4f53-8c13-d62eab053ff5" />

Cantidad de redes: 3

    Red LAN interna (192.168.1.0/24)

    Red DMZ (192.168.2.0/24)

    Red Externa (200.1.1.0/24)

Dispositivos usados:

    1 Router Cisco ISR 2911 (Router_FW – actúa como cortafuegos y punto de interconexión entre redes)

    3 Switches Cisco 2960 (SW_Internal, SW_DMZ, SW_External)

    1 Servidor Web (Server-PT Web_DMZ) en la DMZ

    1 PC Interno (PC_Internal) en la LAN

    1 PC Externo (PC_External) en la red simulada de Internet

LAN (Red Interna)
        Zona privada y segura donde se encuentran los equipos de los usuarios internos. Solo tiene acceso controlado a Internet mediante NAT y está protegida por las ACLs del router.

DMZ (Zona Desmilitarizada)
        Área intermedia y parcialmente expuesta, destinada a alojar servicios públicos. Permite conexiones desde el exterior únicamente a servicios específicos, sin comprometer la seguridad de la red interna.

Red Externa (Internet)
        Simula el acceso desde Internet. Los equipos en esta red pueden interactuar con los servicios de la DMZ, pero no tienen acceso directo a la red interna.


### 3. Plan de direccionamiento IP

| Dispositivo             | IP              | Máscara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             | 192.168.1.10     | 255.255.255.0     | 192.168.1.1       |
| Server_DMZ              | 192.168.2.10     | 255.255.255.0     | 192.168.2.1       |
| PC_External             | 192.168.3.10     | 255.255.255.0     | 192.168.3.1       |
| Router_FW Gi0/0 (LAN)   | 192.168.1.1      | 255.255.255.0     |    -              |
| Router_FW Gi0/1 (DMZ)   | 192.168.2.1      | 255.255.255.0     |    -              |
| Router_FW Gi0/2 (Ext)   | 192.168.3.1      | 255.255.255.0     |    -              |


### 4. Configuración aplicada y verificaciones realizadas

> Primero se configura la direccionamiento IP. Luego se configura el Router2 cpmo aparece a continuación

    denable
    configure terminal
    hostname Router_FW
    
    interface GigabitEthernet0/0
    ip address 192.168.1.1 255.255.255.0
    ip nat inside
    no shutdown
    exit
    
    interface GigabitEthernet0/1
    ip address 192.168.2.1 255.255.255.0
    ip nat inside
    no shutdown
    exit
    
    interface GigabitEthernet0/2
    ip address 192.168.3.1 255.255.255.0
    ip nat outside
    no shutdown
    exit
    
    end
    write memory
    
> Luego se configura la conectividad básica

        ping 192.168.1.1   
        ping 192.168.2.1   
        ping 192.168.3.1   
        
> Se configura el NAT estático

        ip nat inside source static 192.168.2.10 192.168.3.1

> Pruebas iniciales de acceso web

    Desde PC_External, acceder a http://192.168.3.1
    Desde PC_Internal, acceder a http://192.168.2.10

> Finalmente

        Configure terminal
        access-list 120 permit tcp 192.168.3.0 0.0.0.255 host 192.168.3.1 eq 80
        access-list 120 deny ip any any
        interface GigabitEthernet 0/2
        ip access-group 120 in
        end 
        wr

        Configure terminal
        access-list 110 permit tcp 192.168.1.0 0.0.0.255 192.168.2.10 0.0.0.0 eq 80
        access-list 110 permit tcp 192.168.2.10 0.0.0.0 192.168.1.0 0.0.0.255 established
        access-list 110 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
        access-list 110 permit ip any any !
        interface GigabitEthernet 0/1
        ip access-group 110 in
        end
        wr

### 6. Conclusiones y recomendaciones

> Diseñar y asegurar una red segmentada aplicando correctamente los conceptos de DMZ, NAT y ACLs en un entorno simulado como Cisco Packet Tracer.

> Así mismo, comprendí cómo aislar servicios críticos en una zona intermedia para proteger la red interna, y cómo utilizar NAT estático para publicar un servidor web hacia el exterior de forma controlada.


### 7. Capturas de evidencia
Enlace al pdf con imágenes: https://drive.google.com/file/d/1EwuPqALnXtRX9FjOXHXndYAS1IkcivYF/view?usp=sharing

