# qa apim install

El Rol qaapiminstall contiene las tasks necesarias para la instalacion de los pre requisitos para el funcionamiento del producto wso2 api manager.  


Requirements:
------------
Los pre requisitos necesarios para la instalacion es el .zip del producto wso2 api manager. Las demas configuraciones, files necesarios ya se encuentran en la carpeta /qapim-install/files o seran descargados con las task correspondientes especificas a esa instalacion. 


Task:
--------------
Las task utilizadas en este rol son:

- conf-wso2-certs-client.yml

      Copiar los archivos JKS
      
- conf-wso2-filebeat.yml

      Copiar la configuracion yml de filebeat 
      
- conf-wso2-java.yml

      Agregar las variables de entorno de Java
      
- conf-wso2-services.yml

      Copiar la configuracion del servicio
      
- inst-wso2-db-mssql.yml

      copia el controlador JDBC para SQL Server
      
- inst-wso2-java.yml

      Descargar e instala el paquete de Java Amazon Corretto 
      
- inst-wso2-logs-filebeat.yml

      Descargar e instala el paquete de Filebeat
      
- inst-wso2-product.yml

      Descargar el archivo ZIP de WSO2 en la carpeta temporal (cambiar) 
      

cada task esta comentada para mayor claridad.


Handlers:
--------------
Los handles contienen la opcion *listen* que es utilizado con *notify* para la ejecucion del handler, estos son:

- restart filebeat
- restart_wso2
- esperar_TM 9711 (espera a que el puerto 9711 de los *traffic manager* esten habilitados)
- esperar_CP 9443 (espera a que el puerto 9443 de los *Control plane* esten habilitados)

Files:
--------------

- client_gov_ar.xml 

      breve descripcion del file 
      
- client-primary-keystore.jks 

      breve descripcion del file 

- client-trustore.jks 

      breve descripcion del file 

- conf-filebeat.yml 

      breve descripcion del file 

- mssql-jdbc-*.jar 

      breve descripcion del file 

- wso2-*.service

      breve descripcion del file  

<!-- 
Role Variables
--------------
A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.


Dependencies
------------
A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.


Example Playbook
----------------
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }


License
-------
BSD


Author Information
------------------
An optional section for the role authors to include contact information, or a website (HTML is not allowed).
-->