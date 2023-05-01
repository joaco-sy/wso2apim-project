# config 

El Rol config contiene las tasks necesarias para la configuracion final del producto wso2 api manager.  


Requirements:
------------
Los pre requisitos necesarios para la ejecucion de este rol ses que la ejecucion de {ambiente} apim install haya sido correcta.


Task:
--------------
Las task utilizadas en este rol son:

- config.yml

      - Selecciona Hostname
      - Configuraciones de wso2 - {{ host1 }} - Subo el archivo al repository/conf/
      - Configuraciones de wso2 - {{ host2 }} - Subo el archivo al repository/conf/
      - Configuraciones logs de wso2 - {{ grouping }} - Subo el archivo al repository/conf

      
cada task esta comentada para mayor claridad.


Handlers:
--------------
Los handles contienen la opcion *listen* que es utilizado con *notify* para la ejecucion del handler, estos son:

- restart_wso2
- esperar_TM 9711 (espera a que el puerto 9711 de los *traffic manager* esten habilitados)
- esperar_CP 9443 (espera a que el puerto 9443 de los *Control plane* esten habilitados)

Files:
--------------

- deployment-[ cp - gw - tm ]-[ 1 - 2 ].toml 

      archivos de configuracion para los diferentes nodos 
      
- log4j2.properties

      archivo de configuracion de logs 

<!-- 
Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

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