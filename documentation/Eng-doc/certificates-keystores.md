# Configuración de Certificados y de los keystores. 

En el siguiente documento se explica la configuracion de los certificados y los keystores para la implementacion en la aplicacion WSO2 API Manager. 

[[_TOC_]]

---

## Informacion complementaria

<details>
<summary> 1. Explicación del funcionamiento de los JKS</summary>

wso2carbon.jks: Este keystore contiene el key pair y se usa de forma predeterminada para el almacenamiento de certificados.

client-truststore.jks: Este keystore se almacenan las claves de confianza predeterminados que contiene los certificados utilizados en la comunicación SSL.

> wso2carbon.jks y client-truststore.jks, son los nombres originales de los keystores que vienen de fabrica con el producto WSO2. Al algregarles los certificados y las keys de los difenretes ambientes de client, se decidio cambiarles el nombre a: client-primary-keystore.jks = wso2carbon.jks y client-truststore.jks = client-truststore.jks

</details>

## Configuración de certificados 

El keystore explorer es un programa que nos permite vkeycloakalizar con mayor facilidad los certificados dentro de un keystore. Utilizaremos este programa para una mayor facilidad y vicibilidad al explicar el funcionamiento, pero tambien se agregara una seccion de configuracion por consola.

keystores y contrase;as del wso2 configurados en el toml 



### Configuracion Keystores (keystore Explorer)


![asd](../img/keystores-certificate-client.png)

![asd](../img/keystores-certificate-gateway.png)

![asd](../img/keystores-certificate-trustore.png)

<!-- 
### Configuracion Keystores (Consola)
-->

## Configuración de archivos TOML

![Esplicacion de la configuracion del toml](../img/keystores-toml.png)

### a
#### a
indentacion