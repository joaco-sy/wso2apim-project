# Configuración de Red #

**Antes de instalar WSO2 de manera distribuida, es necesario realizar la configuración de red en los nodos.**

> Todos los nombres utilizados en este instructivo pueden ser adaptados para cada deploy dependiendo del ambiente en el que se vaya a utilizar. 

[[_TOC_]]

---
## Importante sobre hostnames y certificados:

> Toda la comunicación debe estar encriptada con el certificado de client. Los nombres de los nodos deben coincidir con la información proporcionada por el certificado. Por ejemplo, en {ambiente}, los certificados wildcard son para *.{ambiente}.client.com. Por lo tanto, se debe configurar los nombres DNS que apuntan a los hosts correspondientes, ya que los hostnames en sí terminan en *.client.com (falta subdominio, dominio “.gov”).

Cada uno de los hosts debe tener acceso al haproxy para poder manejar notificaciones y eventos contra el control plane.

!["diagrama de red"](img/diagrama-arquitectura.png)

---
## Reglas Firewall ##

Para solicitar las reglas de firewall correspondientes, se recomienda utilizar el archivo [firewallrules](./firewallrules.csv) donde se deben reemplazar los nombres de cada nodo y servicio con la IP correspondiente. A continuación se muestra una referencia para el reemplazo:

- CP1: Control Plane 1
- CP2: Control Plane 2
- GW1: Gateway 1
- GW2: Gateway 2
- TM1: Traffic Manager 1
- TM2: Traffic Manager 2
- HA: Haproxy
- LS: Logstash
- DB: Base de Datos

> Una vez realizada la configuración de red, se puede proceder a la instalación detallada en [Inicio](../README.md).