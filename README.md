
# WSO2 API Manager Installation with Ansible

This project has as its objective the distributed installation and configuration of WSO2 API Manager. Using Keycloak as an Identity provider and Ansible as an automation engine.

## Requirements

<details>
<summary>Cluster</summary>

- 2 Control Plane Servers
- 2 Gateway Servers
- 2 Traffic Manager Servers

</details>

<details>
<summary>Servers</summary>

- Meet the requirements mentioned in the section [Network Requirements](./doc/requisitos%20de%20red.md).
- RHEL 8 or +
- 2 cores o +
- 4GB RAM o +
</details>

<details>
<summary>User's machine</summary>

- Ansible Install
- Internet Access
- Access to the 6 nodes
- Access to the repository
</details>


## Installation

Once we meet the requirements, we've ready to continue the installation:

### Files 

clone the repository on the user's machine where you will execute the playbook. Inside the folder /wso2apim-asible/roles/installation/files we will download all the files that we need.

JDK11.rpm:
```
JDK Corretto 11: https://corretto.aws/downloads/resources/11.0.18.10.1/java-11-amazon-corretto-devel-11.0.18.10-1.x86_64.rpm
```

WSO2.zip:
```
WSO2 4.1.0: https://product-dist.wso2.com/products/api-manager/4.1.0/wso2am-4.1.0.zip
```

To start the Installation use this command:
```bash
# ansible-playbook -i inventory-prod.yml main-install.yml
```

This playbook will:
- Download WSO2 product
- Download Java Correto
- Copy all files in the nodes
- Install Java Correto SDK
- Unzip WSO2 file in /opt/wso2
- Copy driver for the database
- Copy the keystores necessary "truststore" y "primary-keystore"
- Install and enable the Filebeat service
- Copy the files for the WSO2 services
- Execute the first rol configuration for wso2 nodes

### Advance Configuration

- [Role based access control configuration](./doc/keycloak-RBAC.md)

- [LDAPS configuration](./doc/ldaps.md)

- [keycloak configuration as an identity provider](./doc/keycloak-idm.md)

- [keycloak configuration as an KeyManager](./doc/keycloak-km.md)

- [keystores](./doc/certificates-keystores.md)
