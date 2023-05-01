# Configuration of Keycloak as key manager.

En el siguiente archivo se eIn the following file, we will explain the configuration of Keycloak as a key manager for the implementation in the WSO2 API Manager application.

> All the names used in this guide can be adapted for each deploy depending on the environment in which it will be used.

[[_TOC_]]

---
## Keycloak configuration.

### **Creating a new Realm**

For the configuration of Keycloak in this implementation, first, a new realm must be created exclusively for the product (WSO2 API Manager).

> Within the {environment} environment, a Realm called "Wso2{environment}" was created.

You must go to the left tab "Select realm" and select "Add Realm".

![keycloak -> realm](../img/keycloak-key-realm.PNG)

### **Creating the Client**

The following images show how to create the client "apim-client" to be able to use Keycloak as a key manager to generate access tokens.

Inside the "Clients" tab, create a new client with the "Create" button.

**ClientID:** 

``` apim-client ```


### **Client Configuration**


![keycloak -> client -> apim client](../img/keycloak-key-apimclient.PNG)

You will find the "root URL", "valid redirect URL", "admin URL" and "web origin" fields. These are the essential fields for this configuration to work.


**Root URL:** 

``` https://apim.(ambiente).client.com/ ```

**valid redirect URL:** 

```https://apim.(ambiente).client.com:9443/``` 
```https://apim.(ambiente).client.com/```

**admin URL:** 

```https://apim.(ambiente).client.com/```

**web origin:** 

```https://apim.(ambiente).client.com```


#### **Client Credentials**

Inside the credentials tab within the client ("apim-client", in this case), you must generate the "Secret" if it is not already generated. The ClientID and Client Secret must be copied so that the key manager can be configured on the WSO2 API Manager application side.

![keycloak -> client -> apim client -> credentials](../img/keycloak-key-apimclient-credential.PNG)

#### **Client Scope (apim-client)**

Remember to go to the Scope tab and disable the client's full scope.

#### **Service Account Roles for the Client (apim-client)**

Inside the default client scope tab, add the "Admin" role to the client if there is no already configured role that performs this function.

![settings del client-apim service acount role](../img/keycloak-key-apimclient-sar.PNG)

---
## WSO2 Configuration.

The Keycloak certificate for the correct communication between keycloak <-> WSO2 API Manager is already in the keystore "client-trustore.jks" inside the {environment} environment. This nomenclature was used: {client}-trustore.jks, with "client-trustore.jks" being the final name of the file.

> Since the certificate is already included in the jks that is in the repository, you can verify the existence of the certificate and the correct configuration using the "KeyStore Explorer" tool.

![imagen de certificado usando keystore client](../img/keycloak-key-certs.PNG)

Log in to https://<<apim.URL>>/admin

In the tabs located on the left, you will find the KeyManagers tab where all the KeyManagers that WSO2 is currently using are displayed.

![imagen de /admin > keystore > add keystore](../img/keycloak-key-publisher-add.PNG)

1. Go to the Key Manager tab and click on Add Key Manager.
2. Select Keycloak as the type of KeyManager.
3. Add the well-known URL of Keycloak and import the configuration.
4. Import the ID and Secret of the client "apim-client" that we configured earlier in Keycloak.


**Well known URL:**

```https://{KeyCloak URL}/realms/{reaml}/.well-known/openid-configuration```

```https://keycloak.{ambiente}.client.com/realms/wso2{ambiente}/.well-known/openid-configuration```

![image where the well-known url is added and imported](../img/keycloak-key-publisher-import.PNG)