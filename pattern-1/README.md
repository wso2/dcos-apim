# DC/OS Resources for WSO2 API Manager <br/>Deployment Pattern 1

## Quick Start Guide

1. Install a DC/OS cluster on the local machine using the [DC/OS Vagrant](https://github.com/dcos/dcos-vagrant) 
for evaluation purposes or prepare an existing DC/OS cluster for the deployment.

2. Build WSO2 API Manager and WSO2 API Manager Analytics Docker images by 
refering README.md files found in [dockerfiles/apim](../dockerfiles/apim) 
and [dockerfiles/apim-analytics](../dockerfiles/apim-analytics) folders.

3. Push above Docker images to a Docker registry or to the DC/OS nodes. If 
a Docker registry is used, prefix the tags of Docker images with the Docker 
registry hostname in the [DC/OS marathon-applications](marathon-applications/).

4. Download [MySQL Connector for Java v5.1.34](https://downloads.mysql.com/archives/c-j/) 
and copy its JAR file to the following folders:

````bash
cp path/to/mysql-connector-java-5.1.34-bin.jar pattern-1/volumes/apim/repository/components/lib/mysql-connector-java-5.1.34-bin.jar

cp path/to/mysql-connector-java-5.1.34-bin.jar pattern-1/volumes/apim-analytics/repository/components/lib/mysql-connector-java-5.1.34-bin.jar
````

5. Mount a shared network volume to all DC/OS nodes and create four folders 
for MySQL data, MySQL scripts, API-M, and API-M Analytics. If DC/OS Vagrant is used ssh 
into each DC/OS node and create below folders on those hosts:

````bash
/volumes/wso2/mysql-data # a blank folder for persisting MySQL data
/volumes/wso2/mysql-scripts # provides MySQL scripts for creating WSO2 API-M and API-M Analytics databases
/volumes/wso2/apim # provides configurations, extensions, and shares runtime artifacts of the API-M containers
/volumes/wso2/apim-analytics # provides configurations, and extensions to the API-M Analytics containers
````

6. Create a user group called ```wso2``` with the group id 200 on each DC/OS node 
and change the group of above folder to ```wso2```:

````bash
groupadd --system -g 200 wso2
chgrp -R wso2 /volumes/wso2
````

7. Copy the content found in the [volumes](volumes/) folder into the above folders.

8. Install DC/OS CLI by following the instructions found in the 
[DC/OS CLI Installation Guide](https://docs.mesosphere.com/1.10/cli/install/).

9. Log into the DC/OS cluster by executing the below command:

````bash
dcos auth login
````

10. Install Marathon load balancer by executing the below command:

````bash
dcos package install marathon-lb
````

11. Once the Marathon load balancer becomes healthy install MySQL Marathon
application by executing the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/mysql.json
````

Please note that this MySQL Marathon application is only intended for 
evaluation purposes and for enterprise deployments a production ready 
RDBMS would need to be used.

12. Once the MySQL Marathon application becomes healthy, install WSO2 
API Manager Analytics Marathon application by executing the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/wso2apim-analytics.json
````

13. Once the WSO2 API Manager Analytics Marathon application becomes 
healthy install WSO2 API Manager Marathon application by executing 
the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/wso2apim.json
````

14. Now find the IP address of the Marathon load balancer via the DC/OS 
CLI or UI and add the following ```/etc/hosts``` entries in the local machine. 
For an instance if the IP address of the Marathon load balancer is 
```192.168.65.60```, ```/etc/hosts``` entries will need to be defined as follows:

````bash
192.168.65.60 wso2apim
192.168.65.60 wso2apim-gateway
````

15. Open a web browser and visit the WSO2 API Manager Publisher URL:

````bash
https://wso2apim/publisher
````