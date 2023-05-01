# Configuration of Keycloak as Identity Provider in WSO2 API Manager

This guide will guide you through the process of configuring Keycloak as an Identity Provider for implementation in the WSO2 API Manager application.

> All names used in this guide can be adapted for each deployment depending on the environment in which it will be used.

[[_TOC_]]

---

<details>
<summary>Images to configure</summary>

![config keycloak idm](../img/config-keycloak-identity-5.png)
![config keycloak idm](../img/config-keycloak-identity-6.png)
![config keycloak idm](../img/config-keycloak-identity-7.png)

</details>


## Keycloak Configuration

> To proceed with this guide, it is necessary to have a Realm dedicated to WSO2.

### Client Configuration

We will create a new Keycloak client that we will use in [WSO2 config](#WSO2-config).

1. Go to Clients and click on Create.
2. Give it a name and add the WSO2 root URL by attaching the /commonauth/ path.
    > ej: \
        > WSO2-login \
        > https:// /commonauth/

3. In the Settings tab, change the Access Type to confidential and save it.
4. In the Roles tab, create four roles:
    > Publisher \
    > Creator \
    > Admin \
    > Subscriber

5. In the Mappers tab, click on Create.
6. Fill in the following fields and save it:
    > Name: Nombre del mapper \
    > Mapper Type: ```User Client Role``` \
    > Client ID: ```WSO2-login``` \
    > Multivalued: ```ON``` \
    > Token Claim Name: (ej: ```mimapper```) \
    > Claim JSON Type: ```String```



### **Configuration of users for the different platforms**

Next, we will proceed to create the roles for the use of the Publisher, Creator, Admin and Subscriber platforms. It is possible to edit and add several roles with different scopes.

> If you need more information to continue with this topic, you can consult the corresponding documentation.

> These names should be different from the ones we indicate, as they are the default values in the documentation and are not considered secure.

> Keep in mind that the same names will be used in WSO2.

Go to the roles section and create three roles with the names for the platforms: publisher, creator, admin, and subscriber. Assign the necessary users to the created roles.

---

## WSO2 Product Configuration

In this section we will be configuring the relationship between WSO2 and Keycloak for OAuth2.0 configuration and to be able to log in with Keycloak in WSO2 with correctly assigned roles.

### **Required Data**
To configure communication with Keycloak, we are going to look for some data in Keycloak that we will use later.

#### In Keycloak
1. Within our realm in Keycloak, we will go to Realm Settings, in the General tab, right-click and copy the link of OpenID Endpoint Configuration.
2. Go to clients and open the client created in [Client Configuration](#Configuración-de-Client).
3. Copy the client name.
4. Copy the secret.

### Identity Provider
> It is important to take into account the relationship of roles and mappers from the Keycloak section, the names must match.

1. Inside the carbon application of WSO2, we will go to the Main side tab within the Identity Providers section and click on Add.
2. Fill in the following data in the Basic Information section:

    > Identity Provider Name: keycloak \
    > Display Name: keycloak \
    > Alias: ```https:// /oauth2/token```
3. Fill in the following data in the Claim Configuration/Basic Claim Configuration section:

    > Identity Provider Claim URIs: (```Add Claim Mapping```)  
    >    | Identity Provider Claim URI | Local Claim URI |
    >    | --- | --- |
    >    | role | http://wso2.org/claims/role |
    >    | preferred_username | http://wso2.org/claims/displayName |
    >    | groups | http://wso2.org/claims/groups |

    > User ID Claim URI: ```preferred_username``` \
    > Role Claim URI: ```name```
4. Fill in the following data in the Role Configuration section:

    > Identity Provider Roles: (```Add Role Mapping```)
    >   | Identity Provider Role | Local Role |
    >   | --- | --- |
    >   | creator | Interna/creator |
    >   | publisher | Internal/publisher |
    >   | subscriber | Internal/subscriber
    >   | admin | Internal/admin |
5. Fill in the following data in the Federated Authenticators/OAuth2/OpenID Connect Configuration section:
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