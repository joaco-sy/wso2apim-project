# Configuracion de Keycloak como Identity Provider en WSO2 API Manager

Este instructivo le guiará en el proceso de configuración de Keycloak como Identity Provider para la implementación en la aplicación WSO2 API Manager.

> Todos los nombres utilizados en este instructivo pueden ser adaptados para cada deploy dependiendo del ambiente en el que se vaya a utilizar. 

[[_TOC_]]

---

<details>
<summary> imagenes a configurar</summary>

![config keycloak idm](img/config-keycloak-identity-5.png)
![config keycloak idm](img/config-keycloak-identity-6.png)
![config keycloak idm](img/config-keycloak-identity-7.png)

</details>


## Configuración de Keycloak

> Para proceder con este instructivo es necesario tener un Realm dedicado a wso2

### Configuración de Client

Vamos a crear un nuevo cliente de Keycloak que utilizaremos en [WSO2 config](#WSO2-config)

1. Nos ubicamos en ```Clients``` y clickeamos en ```Create```
2. Le damos un nombre y le damos el root url de wso2 adjuntando la direccion ```/commonauth/```
    > ej: \
        > WSO2-login \
        > https:// /commonauth/

3. Dentro de la pestaña ```Settings``` vamos a cambiar el ```Access Type``` a ```confidential```, guardamos
4. En la pestaña ```Roles``` vamos a crear cuatro roles:
    > Publisher \
    > Creator \
    > Admin \
    > Subscriber

5. Dentro de la pestaña ```Mappers``` hacemos clic en ```Create```
6. Cubrimos los siguientes campos y guardamos:
    > Name: Nombre del mapper \
    > Mapper Type: ```User Client Role``` \
    > Client ID: ```WSO2-login``` \
    > Multivalued: ```ON``` \
    > Token Claim Name: (ej: ```mimapper```) \
    > Claim JSON Type: ```String```



### **Configuración de usuarios para las distintas plataformas**

A continuación, procederemos a crear los roles para la utilización de las plataformas ```Publisher```, ```Creator```, ```Admin``` y ```Subscriber```. Es posible editar y agregar varios roles con distintos alcances. 
> Si necesita más información para continuar con ese tema, puede consultar la documentación correspondiente. 

>Estos nombres deben ser distintos a los que indicamos, ya que son los valores ```por defecto``` en la documentación y no se consideran seguros. 

>Tenga en cuenta que los mismos nombres se utilizarán en WSO2.

Ubíquese en la sección de roles y cree tres roles con los nombres para las plataformas: ```publisher```, ```creator```, ```admin``` y ```subscriber```.
Asigne los usuarios necesarios a los roles creados.

---

## Configuración de Producto WSO2

En esta sección vamos a estar configurando la relación de wso2 con Keycloak para la configuración del oauth2.0 y poder iniciar sesión con Keycloak en wso2 con los roles correctamente asignados

### **Datos requeridos**
Para poder configurar la comunicación con Keycloak, vamos a buscar algunos datos en Keycloak que utilizaremos más adelante.

#### En keycloak
1. Dentro de nuestro realm en Keycloak vamos a dirigirnos a Realm Settings, en la pestaña General vamos a hacer clic derecho y vamos a copiar el link de OpenID Endpoint Configuration
2. Nos dirigimos a clients y abrimos el cliente creado en [Configuración de Client](#Configuración-de-Client)
3. Copiamos nombre del client
4. Copiamos el secret

### Identity Provider
> Importante tener en cuenta la relacion de los roles y mapper de la seccion de keycloak, los nombres deben coincidir

1. Dentro de la aplicacion ```carbon``` de wso2 nos vamos a ubicar en la pestaña lateral ```Main``` dentro de la seccion ```Identity Providers``` clic en ```Add```
2. completar con los siguientes datos en basic information:

    > Identity Provider Name: keycloak \
    > Display Name: keycloak \
    > Alias: ```https:// /oauth2/token```
3. completar con los siguientes datos en la seccion ```Claim Configuration```/```Basic Claim Configuration```:

    > Identity Provider Claim URIs: (```Add Claim Mapping```)  
    >    | Identity Provider Claim URI | Local Claim URI |
    >    | --- | --- |
    >    | role | http://wso2.org/claims/role |
    >    | preferred_username | http://wso2.org/claims/displayName |
    >    | groups | http://wso2.org/claims/groups |

    > User ID Claim URI: ```preferred_username``` \
    > Role Claim URI: ```name```
4. Completar con los siguientes datos en la seccion ```Role Configuration```:

    > Identity Provider Roles: (```Add Role Mapping```)
    >   | Identity Provider Role | Local Role |
    >   | --- | --- |
    >   | creator | Interna/creator |
    >   | publisher | Internal/publisher |
    >   | subscriber | Internal/subscriber
    >   | admin | Internal/admin |
5. Completar con los siguientes datos en la seccion ```Federated Authenticators```/```OAuth2/OpenID Connect Configuration```:
    > | Config | valor |
    > | --- | --- |
    > | Enable OAuth2/OpenID Connect Configuration | ✅ | 
    > | Client Id | ``{Keycloak Client}`` | 
    > | Client Secret:| ```{Keycloak Client SECRET}``` | 
    > | Authorization Endpoint URL | ``https://keycloak.client.com/realms/{REALM}/protocol/openid-connect/auth`` | 
    > | Token Endpoint URL | ``https:// /realms/{REALM}/protocol/openid-connect/token`` | 
    > | Callback Url | ``https:// /commonauth`` |
    > | Userinfo Endpoint URL | ``https:// /realms/{REALM}/protocol/openid-connect/userinfo`` |
    > | Logout Endpoint URL	| ``https:// /realms/{REALM}/protocol/openid-connect/logout`` |

    



