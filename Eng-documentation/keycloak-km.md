# Configuración de Keycloak como keymanager. #

En el siguiente archivo se explicara la configuracion de keycloak como keymanager para la implementacion en la aplicacion WSO2 API Manager. 

> Todos los nombres utilizados en este instructivo pueden ser adaptados para cada deploy dependiendo del ambiente en el que se vaya a utilizar. 

[[_TOC_]]

---
## Configuracion keycloak. ##

### **Creacion de un nuevo Realm**

Para la configuracion de Keycloak en esta implementacion primero se debe crear un nuevo realm exclusivo para el producto (WSO2 API Manager).

> Dentro del ambiente de {ambiente} se  creo un Realm llamado ```"Wso2{ambiente}"```.

se debe ir a la pestaña de la izquierda ```"Select realm"``` y seleccionar ```"Add Realm"```.

![keycloak -> realm](img/keycloak-key-realm.PNG)

### **Creacion del Cliente**

En las siguientes imagenes se mostrara como crear el cliente ```"apim-client"``` para poder utilizar Keycloak como keymanager asi generar tokens de acceso.

Dentro de la pestaña de ```"Clients"``` crear un nuevo cliente con el boton ```"Create"``` 

**ClientID:** 

``` apim-client ```


### **Configuracion del Cliente**


![keycloak -> client -> apim client](img/keycloak-key-apimclient.PNG)

Se encontrara con los campos ```"root URL"```, ```"valid redirect URL"```, ```"admin URL"``` y ```"web origin"```. Estas son los campos escenciales para que esta configuracion funcione. 


**Root URL:** 

``` https://apim.(ambiente).client.com/ ```

**valid redirect URL:** 

```https://apim.(ambiente).client.com:9443/``` 
```https://apim.(ambiente).client.com/```

**admin URL:** 

```https://apim.(ambiente).client.com/```

**web origin:** 

```https://apim.(ambiente).client.com```


#### **Credenciales del Cliente**

Dentro de la pestaña credenciales dentro del cliente (```"apim-client"```, en este caso) se debe generar el ```“Secret”``` sino esta ya generado, el clientID y el Client Secret hay que copiarlos, asi se puede configurar el keymanager del lado de la aplicacion WSO2 API Manager.

![keycloak -> client -> apim client -> credentials](img/keycloak-key-apimclient-credential.PNG)

#### **Scope del Cliente (apim-client)**

Recordar ingresar a la pestaña de Scope y deshabilitar el full scope del cliente. 


#### **Service account roles del Cliente (apim-client)**

Dentro de la pestaña  default client scope agregarle el Rol ```"Admin"``` al cliente si no hay ya un rol configurado que cumpla esta funcion.

![settings del client-apim service acount role](img/keycloak-key-apimclient-sar.PNG)

---
## Configuracion wso2. ##

El certificado de keycloak para la correcta comunicacion entre  ```keycloak <-> WSO2 API Manager ```, ya se encuentra en el keystore ```"client-trustore.jks"``` dentro del ambiente de {ambiente} se utilizo esta nomenclatura {cliente}-trustore.jks siendo ```"client-trustore.jks"``` el nombre final del archivo. 

> Como el certificado ya esta incluido dentro del jks que se encuentra en el repositorio se puede verificar la existencia del certificado y la correcta configuracion utilizando la herramienta ```"KeyStore Explorer"```

![imagen de certificado usando keystore client](img/keycloak-key-certs.PNG)

logearse en ```https://<<apim.URL>>/admin```

en las pestañas que se encuentran a la izquierda esta la pestaña de KeyManagers aqui se muestran todos los keymanagers que WSO2 esta utilizando en el momento.

![imagen de /admin > keystore > add keystore](img/keycloak-key-publisher-add.PNG)

1. ir a la pestaña de key manager y hacer click en add key manager.
2. seleccionar keycloak como el tipo de keymanager.
3. agregar la well known url de keycloak e importar la configuracion.
4. importar el ID y Secret del cliente ```"apim-client"``` que configuramos anteriormente dentro de keycloak.


**Well known URL:**

```https://{KeyCloak URL}/realms/{reaml}/.well-known/openid-configuration```

```https://keycloak.{ambiente}.client.com/realms/wso2{ambiente}/.well-known/openid-configuration```

![imagen donde se agrega la wellknown url e import](img/keycloak-key-publisher-import.PNG)