# Configuración de Keycloak, RBAC (Rol based access control). #

RBAC es un modelo de seguridad que restringe el acceso solo a usuarios autorizados por Roles dentro de client. En esta implementación WSO2 API Manager se encarga de manejar los scopes y el acceso a claims específicas de cada API. El rol que cumple Keycloak es el de Identity Provider, este se encarga de la creación de usuarios, configuración de roles y creación de clientes para la generación de Tokens JWT para el consumo de APIS.


> Todos los nombres utilizados en este instructivo pueden ser adaptados para cada deploy dependiendo del ambiente en el que se vaya a utilizar.

[[_TOC_]]

---
## Información de complementaria:

<details>
<summary> 1. Explicación del funcionamiento del RBAC</summary>

### funcionamiento del RBAC

Estos diagramas simplifican el funcionamiento del rol access control

Ejemplo Roles:

|          |          | 
| ---      | ---      | 
| Usuarios | Roles    |
| user 1   | tsiglas1 | 
| user 2   | tsiglas2 | 

Cuando se habilita el RBAC se va a necesitar la configuración de roles en Keycloak (mas adelante en el documento se explica cómo hacerlo). En este ejemplo se habilitaron los roles ```"tsiglas 1 y 2"``` asignados a los usuarios ```user 1 y 2```. Se configura el RBAC a la API ```"Siglas"``` para que solo tengan acceso los usuarios del Rol ```"Tsiglas1"```

![Diagrama de funcionamiento roles](img/diagrama-RBAC.png)

<!-- 
Seccion de explicacion del RBAC + scope (en pausa hasta hacer demo para demis)
Ejemplo Scope:

|           |           |                |
| ---       | ---       | ---            |
| Usuarios  | Roles     | Scope          |
| user 1    | tsiglas1  | siglas-group-1 |
| user 2    | tsiglas1  | siglas-group-2 |
| user 3    | tsiglas2  | siglas-group-1 |

La siguiente funcionalidad que se puede configurar son los ```"Scopes"``` con estos podemos restringir a diferentes usuarios dentro de un mismo rol el acceso a diferentes ```claims``` de la API. En este ejemplo se habilito el rol ```tsiglas 1``` asignado a los ususarios ```user 1 y 2``` con ```scope``` diferente, ```siglas-group-1 y 2``` y el rol ```"tsiglas 2"``` para el ```user 3```. 

En este ejemplo el ```user 1``` tiene acceso a la ```API Siglas``` tambien a los claims ```POST``` y ```DELETE```. El ```user 2``` tambien tiene acceso a la ```API Siglas``` pero a diferentes claims cuando intente utilizar claims a los que no tenga acceso este sera rechazado.

El ```user 3``` esta para ejemplificar que aunque un usuario tenga un ```scope``` que le habilite a utilizar ciertos claims, no pertence al rol que esta autorizado a consumir la API.  

![config keycloak RBAC](img/diagrama-RBAC-claim.png) 
-->
</details>

<details>
<summary>2. Información para entender la conexión entre keycloak - devportal</summary>

### Keys y Secrets de aplicacion "Devportal". 

Cada vez que uno necesita crear una aplicación/keys para que sea subscriba a una API, se genera un ClientID y secret en keycloak. Esto se puede ver en las siguientes imágenes, donde se crean las nuevas aplicaciones y donde se generan las keys necesarias. Producción y Sanbox no comparten keys, sino que generan ClientID diferentes.

> Uno puede crear un cliente con su propio ClientID en Keycloak y sus propios Secrets. Estos son intercambiables entre sí.

> En la sección de Configuración WSO2 se encuentra la sección de creación de aplicaciones.

![devportal -> apps](img/keycloak-rbac-app.PNG)

![devportal -> apps -> keys](img/keycloak-rbac-app-keys.PNG)

</details>

---

## Configuración de Keycloak. 

### **Creación de rol**

Se debe ingresar al realm correspondiente de la implementación necesaria, en este caso se verá que el realm se llama ```wso2{ambiente}``` esto se puede aplicar a cualquier realm. 

Dentro de la pestaña de ```Roles``` tenemos la opción de crear nuevos Roles para el realm. Aquí seleccionaremos ```Add Role```.

![keycloak -> roles](img/keycloak-rbac-roles.PNG)

Si vamos a la configuración del Rol ```Action -> Edit```. Aqui se nos presentara la pestaña ```Users in Role``` aqui podremos ver los usuarios pertenecientes al Rol.

Para agregar usuarios al Rol correspondiente deberemos ir a:

- Realm "Wso2{ambiente}"
    - Manage
        - users / groups

Aquí podremos agregar a usuarios o directamente a grupos enteros.

> se podrán agregar grupos enteros, pero esto dependerá de la configuración de los User Stores que Keycloak tenga configurado. ver [keycloak como Identity provider](./keycloak-idm.md).


![keycloak -> roles -> users in role](img/keycloak-rbac-roles-users.PNG)


### **Creación de nuevo client scope**

Dentro de la pestaña de ```Client Scope``` tenemos la opción de crear nuevos Client Scope para el realm. Aqui seleccionaremos ```Create```.

![keycloak -> roles -> Cleint Scope](img/keycloak-rbac-clientscope.PNG)

Si vamos a la configuracion del Client Scope ```Action -> Edit```. Aqui se nos presentara la pestaña ```Mappers / Scope```.

```Scope```: veremos los Roles asignados a este Client scope

```Mappers```: los mappers es información que ira encriptada dentro del JWT que vayamos a crear. 

- configure
    - Client Scope
        - {Client scope name}
            - Scope / mappers


En la pestaña Mappers deberemos agregar el mapper llamado ```"Scope"```, luego editar este Mapper.
 

|          |           |
| ---      | ---       |
| ID   | {ClientID}  |
| Name   | Scope  |
| Mapper Type | User Realm Role     |
| Token Claim Name   | scope  |
| Claim JSON Type  | string  |


> La información se autogenera, pero es recomendable revisar antes de continuar.

pasando a la configuración de la pestaña Scope seleccionaremos el Rol creado anteriormente. Que contiene los usuarios Autorizados para consumir la API. 

> Debemos quitar todos los Roles y solo dejar el autorizado.

![keycloak->roles->CleintScope->scope](img/keycloak-rbac-clientscope-scope.PNG)

### **Configuración del Cliente**

El ```cliente``` a configurar está relacionado con la aplicación creada desde el ```DevPortal``` cuando creamos las keys para el Keycloak, esta misma está relacionada con la API cuando está subscrita a una aplicación.

> se puede realizar la configuración con un cliente de keycloak pre creado o con un cliente creado automáticamente al generar las keys de una aplicación dentro del devportal. Dependiendo de la elección de como uno quiera realizar la configuración también se adjunta como crear un Cliente en keycloak.

<details>
<summary>Creacion de Client Keycloak</summary>

### Creación de Client Keycloak

Seleccionar el realm correspondiente, ir a la pestaña de clients y seleccionar ```"Create"``` 

![keycloak->client->addclient](img/keycloak-rbac-client-create.PNG)

</details>

> para más información ver al principio del documento información complementaria o el documento [keycloak como KeyManager](./keycloak-km.md).

Dentro de la misma pestaña de Settings deberemos buscar el campo Acces Type y seleccionar ```"confidential"``` como el tipo de acceso.

![keycloak->client->settings](img/keycloak-rbac-client-settings.PNG)

También se deberá establecer la root URL como la URL del gateway WSO2.

Root URL:
```https://apim.{ambiente}.client.com/ ```

> esta URL de ejemplo solo se aplica para ambiente de {ambiente}

En la pestaña ```"scopes"```, primero deshabilitar ```Full scope Allowed``` y eliminar todos los valores de ```Assigned Roles``` y solo agregar el client scope creado en los pasos anteriores.

Luego en la pestaña ```"Client scopes"```, deshabilitar todos los ```Assigned Default Client Scopes ``` y dejar el como ```"Assigned Optional Client Scope"``` al Client Scope correspondiente a este ClientID.


---
## Configuración WSO2

### Configuración DevPortal

#### **Configuración de una nueva aplicación**

Como se explicó anteriormente las aplicaciones están creadas por el DevPortal están subscritas a una API y la misma aplicación conectada a Keycloak. 

Ingresamos a /devportal y creamos una nueva aplicacion. La ```shared Quota for Aplication Tokens``` se puede volver a configurar. 

> ver documento [Managment Token Quotas](./WorkInProgress.md).

![devportal -> apps](img/keycloak-rbac-app.PNG)

![devportal -> apps -> keys](img/keycloak-rbac-app-keys.PNG)

Dentro de la pestaña de Production/Sanbox keys encontraremos el campo Consumer Key y consumer secret. Aquí copiaremos el ```Secret``` y el ```ClientID``` correspondiente. 

> seleccionar la pestaña correspondiente a Keycloak (keycloak-{ambiente} en este ejemplo)

#### **Subscripción**

En la pestaña de suscripción podremos suscribir a la API correspondiente dándole al botón ```+ Subscribe APIS```


---
### Configuración Publisher

#### **configuración scope**

desde la pag inicial del /publisher para configurar los scopes y realizar la relación entre los scopes de WSO2 y Keycloack se debe ir a la pestaña scopes y generar un nuevo scope con el mismo nombre que el Rol generado en Keycloak. 

![publisher-scope](img/keycloak-rbac-publisher-scope.PNG)

<!-- 
Seccion de explicacion del RBAC + scope (en pausa hasta hacer demo para demis)

**configuracion API**

Para habilitar el RBAC se debe realizar la ultima configuracion desde el /publisher.

seleccionando la API que queremos restringir con RBAC, desde: 
- Overview 
    - portal configuration
        - Basic info

Podremos configurar Developer Visibility y restringirlo por roles
> aclaracion: esta restriccion es de roles internos de la aplicacion como ej: Internal/publisher, mas sobre la configuracion de roles y usuarios con diferentes UserStores en:  [keycloak como Identity provider](./keycloak-idm.md).

![config keycloak RBAC](img/config-keycloak-RBAC-10.PNG)
-->

#### **Configuración roles**

Dentro de la pestaña Resources: 
- Overview 
    - API configuration
        - Resources

podemos ver los diferentes claims con los que se pueden interactuar. En cada uno de estos, nosotros podemos configurar y/o restringir el acceso utilizando los scopes que creamos.

![publisher-scope-API-resources](img/keycloak-rbac-pusblisher-api-resources.PNG)

![publisher-scope-API-resources-claim](img/keycloak-rbac-pusblisher-api-resources-claim.PNG)
---
> no olvidar que con cada cambio que se realice en la API se necesitara realizar un deploy de la nueva revisión de la API. 

![config keycloak RBAC](img/keycloak-rbac-pusblisher-api-deploy.PNG)



<!--
Seccion de explicacion del RBAC + scope (en pausa hasta hacer demo para demis)


Los usuarios, Roles y Client Scoupe que se utilizaran en esta demostracion seran:

|          |           |                    |
| ---      | ---       | ---                |
| Usuarios | Roles     | Client Scopes      |
| kuser1   | tsiglas1  | siglas-test-user-1 |
| kuser2   | tsiglas2  | siglas-test-user-2 |

> los usuarios utilizados son usuarios locales de Keycloak, este instructivo esta pensado para mostrar como se realiza la configuracion. Pero se pueden usar Usuarios de diferentes userstores si estuviecen configurados. 

> ver [keycloak como Identity provider](./keycloak-idm.md).


<!-- imagenes de user client y roles. 
![config keycloak RBAC](img/config-keycloak-RBAC-1.PNG)
![config keycloak RBAC](img/config-keycloak-RBAC-2.PNG)
![config keycloak RBAC](img/config-keycloak-RBAC-3.PNG) 
->

1. Al crear los Roles se encuentra la opcion de agregar usuarios, aqui asignaremos el usuario kuser1 al rol correspondiente. en este caso tsiglas1. haremos lo mismo con tsiglas2.
2. al crear Clients Scoupes se encuentra la pesta;a scoupe aqui asignaremos el Rol necesario para poder utilizar el scope. siglas siglas-test-user1 Scope Mappings

|                       |                             |
| ---                   | ---                         |
| Client Scopes         | Assigned/Effective Roles    |
|  siglas-test-user-1   | tsiglas1                    |
|  siglas-test-user-2   | tsiglas2                    |

> Ya teniendo la configuracion de los Roles y Client Scope, se puede seguir a la configuracion del Client (ClientId )

### Configuración de Client - Keycloak.
---

|                        |                    |                |
| ---                    | ---                | ---            |
| ClientID               | Client Scopes      | Assigned Roles |
| aplication-devportal   | siglas-test-user-1 | tsiglas1       |
| aplication-devportal-2 | siglas-test-user-2 | tsiglas2       |

> recordar que para configurar un scope se debe deshabilitar el full scope del cliente.

para asignar un client scope y roles, se debe ir a la pestaña Scope para asignar los roles con los usuarios que estaran autorizados para consumir la API. desde la pestaña 

![config keycloak RBAC](img/config-keycloak-RBAC-5.PNG)

.

![config keycloak RBAC](img/config-keycloak-RBAC-4.PNG)

---
## Configuración de /publisher. 


### configuracion scope /publisher

desde la pag inicial del /publisher para configurar los scopes y realizar la relacion entre los scopes de WSO2 y Keycloack se debe ir a la pestaña scopes y generar un nuevo scope con el mismo nombre que el Rol generado en Keycloak 

![config keycloak RBAC](img/config-keycloak-RBAC-11.PNG)


### configuracion API /publisher
Para habilitar el RBAC se debe realizar la ultima configuracion desde el /publisher.

seleccionando la API que queremos restringir con RBAC, desde: 
- Overview 
    - portal configuration
        - Basic info

Podremos configurar Developer Visibility y restringirlo por roles
> aclaracion: esta restriccion es de roles internos de la aplicacion como ej: Internal/publisher, mas sobre la configuracion de roles y usuarios con diferentes UserStores en:  [keycloak como Identity provider](./keycloak-idm.md).


![config keycloak RBAC](img/config-keycloak-RBAC-10.PNG)

Configuracion roles 

Dentro de la pestaña Resources: 
- Overview 
    - API configuration
        - Resources

podemos ver los diferentes claims con los que se pueden interactuar. En cada uno de estos, nosotros podemos configurar y/o restringir el acceso utilizando los Roles y scopes que creamos.

![config keycloak RBAC](img/config-keycloak-RBAC-12.PNG)



![config keycloak RBAC](img/config-keycloak-RBAC-13.PNG)

---
> no olvidar que con cada cambio que se realice en la API se necesitara realizar un deploy de la nueva revicion de la API. 
![config keycloak RBAC](img/config-keycloak-RBAC-14.PNG)
--> 