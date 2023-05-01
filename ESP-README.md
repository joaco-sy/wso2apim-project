# Instalación de WSO2 distribuida con Ansible

Este proyecto tiene como objetivo la instalación distribuida y configuracion de WSO2 usuarios desde Active Directory utilizando keycloak como Identity provider utilizando Ansible como medio de automatización.

[[_TOC_]]


## Requisitos

<details>
<summary>Cluster</summary>

- 2 servidores para Control Plane
- 2 servidores para Gateway
- 2 servidores para Traffic Manager
</details>

<details>
<summary>Servidores</summary>

- Cumplir con los requisitos mencionados en el apartado [requisitos de red](./doc/requisitos%20de%20red.md).
- RHEL 8 o posterior
- 2 cores o más
- 4GB RAM o superior
</details>

<details>
<summary>Maquina del usuario</summary>

- Ansible instalado
- Acceso a internet
- Acceso a los 6 nodos
- Acceso al repositorio de git para poder clonarlo
</details>

## Instalación

Una vez cumplidos los requisitos, continuar con la instalacion:

## Archivos

Hacer el clon de este repositorio en la maquina local desde donde se va a ejecutar el playbook
dentro de la carpeta /roles/apim-install/files/ vamos a descargar dos archivos, por un lado la version de JDK y por otro el wso2, ambos archivos deben tener el nombre indicado para el correcto funcionamiento de los playbooks

JDK11.rpm:
```
JDK Corretto 11: https://corretto.aws/downloads/resources/11.0.18.10.1/java-11-amazon-corretto-devel-11.0.18.10-1.x86_64.rpm
```

WSO2.zip:
```
WSO2 4.1.0: https://product-dist.wso2.com/products/api-manager/4.1.0/wso2am-4.1.0.zip
```
 
Para comenzar con la instalación, siga los siguientes pasos:

1. Clone el repositorio en su máquina local.
2. Acceda al directorio `wso2-ansible`.
3. Ejecute el siguiente comando para instalar WSO2 en los servidores:
```bash
# ansible-playbook -i inventory-prod.yml main-install.yml
```

Este comando realizará las siguientes tareas:
-   Descarga el paquete de WSO2
-   Descarga Java Correto
-   Copia todos los archivos necesarios a cada host
-   Instala Java Correto SDK
-   Descomprime el archivo WSO2 en /opt/wso2
-   Copia el driver de la base de datos
-   Copia los jks truststore y primary-keystore
-   Instala y activa servicio de filebeat
-   Genera archivos de servicio WSO2
-   Ejecuta Configuracion inicial de rol de nodo WSO2

¡Listo! Ahora debería tener una instalación distribuida de WSO2 utilizando Ansible.

## Configuraciones posteriores

En este apartado ingresamos distintos instructivos para la utilizacion del producto:

- [Configuracion de Scope](./doc/keycloak-RBAC.md)

- [Configuracion de LDAPS](./doc/ldaps.md)

- [Configuracion de keycloak como identity provider](./doc/keycloak-idm.md)

- [Configuracion de keycloak como KeyManager](./doc/keycloak-km.md)

- [Configuracion de keystores y certificados](./doc/certificates-keystores.md)