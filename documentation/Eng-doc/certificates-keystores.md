# Configuration of Certificates and Keystores.

The following document explains the configuration of certificates and keystores for implementation in the WSO2 API Manager application.

[[_TOC_]]

---

## Additional Information

<details>
<summary> 1. Explanation of JKS functioning </summary>

wso2carbon.jks: This keystore contains the key pair and is used by default for storing certificates.

client-truststore.jks: This keystore stores the default trust keys that contain the certificates used in SSL communication.

> wso2carbon.jks and client-truststore.jks are the original names of the keystores that come with the WSO2 product. When adding the certificates and keys of different client environments, it was decided to change their names to: client-primary-keystore.jks = wso2carbon.jks and client-truststore.jks = client-truststore.jkss

</details>

## Certificate Configuration

The Keystore Explorer is a program that allows us to easily visualize the certificates within a keystore. We will use this program for greater ease and visibility in explaining the operation, but a console configuration section will also be added.

Keystores and passwords of the WSO2 configured in the TOML.


### Keystore Configuration (Keystore Explorer)


![asd](../img/keystores-certificate-client.png)

![asd](../img/keystores-certificate-gateway.png)

![asd](../img/keystores-certificate-trustore.png)


## TOML File Configuration

![TOML Configuration explanation](../img/keystores-toml.png)

### a
#### a
indentacion