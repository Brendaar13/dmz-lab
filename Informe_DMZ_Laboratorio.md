# Informe de configuraciÃ³n de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

> Tiene como objetivo desarrollar competencias en ciberseguridad defensiva mediante la implementación de una Zona Desmilitarizada (DMZ) en Cisco Packet Tracer.
A través de la configuración de routers, switches, servidores y ACLs, se aprenderá a aislar servicios críticos, controlar el tráfico entre redes internas y externas, y evitar exponer servicios públicos de manera segura y controlada.

**Ejemplo:**  
Configurar una DMZ segura usando un router Cisco ISR, aplicando NAT y ACLs para controlar el tráfico entre LAN, DMZ y red externa.

---

### 2. TopologÃ­a implementada
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

- Breve descripciÃ³n de la funciÃ³n de cada zona (LAN, DMZ, Externa).



### 3. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambiÃ³).

| Dispositivo             | IP              | MÃ¡scara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             |                  |                   |                   |
| Server_DMZ              |                  |                   |                   |
| PC_External             |                  |                   |                   |
| Router_FW Gi0/0 (LAN)   |                  |                   |                   |
| Router_FW Gi0/1 (DMZ)   |                  |                   |                   |
| Router_FW Gi0/2 (Ext)   |                  |                   |                   |


### 4. ConfiguraciÃ³n aplicada (resumen)

> Resume los comandos o pasos mÃ¡s relevantes que ejecutaste. Usa texto + fragmentos de cÃ³digo cuando sea necesario.

- Interfaces configuradas con `ip address`
- NAT:
```bash
ip nat inside source static 192.168.2.10 192.168.3.1
```
- ACLs:
```bash
access-list 101 permit tcp any host 192.168.3.1 eq 80
access-list 100 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
```



### 5. Verificaciones realizadas

> Describe las pruebas y su resultado. Incluye capturas o salidas de comandos si se puede.

- `ping` desde PC_Internal al router: âœ…
- Acceso web desde PC_External: âœ…
- Bloqueo de acceso desde DMZ a LAN: âœ…


### 6. Conclusiones y recomendaciones

> Â¿QuÃ© aprendiste con este ejercicio? Â¿QuÃ© mejorarÃ­as?

**Ejemplo:**
AprendÃ­ a aplicar NAT y ACLs en un entorno simulado. Recomiendo verificar conectividad bÃ¡sica antes de aplicar reglas de firewall, ya que un error en la IP puede bloquear todo.


### 7. Capturas de evidencia

> Adjunta aquÃ­ (o en un PDF anexo) las capturas solicitadas: pings, navegador, comandos `show`, etc.
