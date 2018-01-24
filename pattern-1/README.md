# DC/OS Resources for WSO2 API Manager Deployment Pattern 1

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

5. Mount a shared network volume to all DC/OS nodes and create three folders 
for API-M, API-M Analytics and MySQL containers. If DC/OS Vagrant is used ssh 
into each DC/OS node and create below folders on those hosts:

````bash
/volumes/wso2/mysql
/volumes/wso2/apim
/volumes/wso2/apim-analytics
````

6. Copy the content found in the [volumes](volumes/) folder into the above folders.

7. Install DC/OS CLI by following the instructions found in the 
[DC/OS CLI Installation Guide](https://docs.mesosphere.com/1.10/cli/install/).

8. Log into the DC/OS cluster by executing the below command:

````bash
dcos auth login
````

9. Install Marathon load balancer by executing the below command:

````bash
dcos package install marathon-lb
````

10. Once the Marathon load balancer becomes healthy install MySQL Marathon
application by executing the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/mysql.json
````

Please note that this MySQL Marathon application is only intended for 
evaluation purposes and for enterprise deployments a production ready 
RDBMS would need to be used.

11. Once the MySQL Marathon application becomes healthy, install WSO2 
API Manager Analytics Marathon application by executing the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/wso2apim-analytics.json
````

12. Once the WSO2 API Manager Analytics Marathon application becomes 
healthy install WSO2 API Manager Marathon application by executing 
the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/wso2apim.json
````

13. Now find the IP address of the Marathon load balancer via the DC/OS 
CLI or UI and add an /etc/hosts entry in the local machine pointing the 
hostname ```wso2apim``` for that IP address.

14. Open a web browser and visit the WSO2 API Manager Publisher URL:

````bash
https://wso2apim/publisher
````