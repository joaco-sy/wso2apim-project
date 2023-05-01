#  APIM installation

This Role "Installation" has the task necessary for the installation and the pre-requirements.


## Requirements:

The pre-requirements are:
- wso2apim-product.zip named "WSO2.zip"
- JDK Corretto 
- database-driver
- .rpm needed


## Tasks:

Las task utilizadas en este rol son:

- conf-wso2-certs-client.yml

      Copy JKS Files
      
- conf-wso2-filebeat.yml

      Copy filebeat configuration 
      
- conf-wso2-java.yml

      Add Java Environment Variables
      
- conf-wso2-services.yml

      Copy service configuration
      
- inst-wso2-db-mssql.yml

      Copy driver JDBC SQL Server
      
- inst-wso2-java.yml

      Download and install Java Amazon Corretto 
      
- inst-wso2-logs-filebeat.yml

      Download and install Filebeat
      
- inst-wso2-product.yml

      Download and install ZIP  WSO2
      


### Handlers:

The handles have the option *listen* and *notify*:

- restart filebeat
- restart_wso2
- wait_TrafficManager 9711
- wait_ControlPlane 9443

### Files:

- client-primary-keystore.jks 
- client-trustore.jks 
- conf-filebeat.yml 
- mssql-jdbc-*.jar 
- wso2-*.service