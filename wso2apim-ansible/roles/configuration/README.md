# Configuration 

The Configuration role contains the necessary tasks for the final configuration of the WSO2 API Manager product.


## Requirements:
The prerequisites necessary for the execution of this role are that the execution of installation has been successful.

## Task:

This is a list of tasks performed by the "config" role:

- config.yml:
      - Selects a hostname.
      - Performs WSO2 configurations for host1 and uploads the file to repository/conf/.
      - Performs WSO2 configurations for host2 and uploads the file to repository/conf/.
      - Performs WSO2 logs configurations for grouping and uploads the file to repository/conf.

      
Each task is commented for greater clarity.

### Handlers:
Handlers contain the listen option which is used with notify for handler execution. These are:

- restart_wso2
- wait_TM 9711 (wait for port 9711 of the traffic manager to be enabled)
- wait_CP 9443 (wait for port 9443 of the Control plane to be enabled)

### Files:
--------------

- deployment-[ cp - gw - tm ]-[ 1 - 2 ].toml
> Configuration files for different nodes 
      
- log4j2.properties
> Log configuration file
