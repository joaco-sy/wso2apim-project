# Network Configuration

Before installing WSO2 in a distributed manner, it is necessary to perform network configuration on the nodes.

> All names used in this guide can be adapted for each deployment depending on the environment in which it will be used.

[[TOC]]

---
Important notes about hostnames and certificates:

> All communication must be encrypted with the client certificate. The node names must match the information provided by the certificate. For example, in {environment}, wildcard certificates are for *.{environment}.client.com. Therefore, the DNS names that point to the corresponding hosts must be configured, as the hostnames themselves end in *.client.com (missing subdomain, domain ".gov").

Each of the hosts must have access to haproxy to handle notifications and events against the control plane.

!["Network diagram"](../img/diagrama-arquitectura.png)

---
# Firewall Rules

To request the corresponding firewall rules, it is recommended to use the firewallrules file where the names of each node and service should be replaced with the corresponding IP. The following is a reference for replacement:

- CP1: Control Plane 1
- CP2: Control Plane 2
- GW1: Gateway 1
- GW2: Gateway 2
- TM1: Traffic Manager 1
- TM2: Traffic Manager 2
- HA: Haproxy
- LS: Logstash
- DB: Database

> Once the network configuration is complete, you can proceed with the detailed installation in [Start](../../README.md).
