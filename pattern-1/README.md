# DC/OS Resources for WSO2 API Manager Deployment Pattern 1

## Quick Start Guide

1. Build WSO2 API Manager and WSO2 API Manager Analytics Docker images by refering README.md files found in [dockerfiles/apim](../dockerfiles/apim) and [dockerfiles/apim-analytics](../dockerfiles/apim-analytics) folders.

2. Push above Docker images to a Docker registry or to the DC/OS nodes. If a Docker registry is used, prefix the tags of Docker images with the Docker registry hostname in the [DC/OS marathon-applications](marathon-applications/).

3. Download [MySQL Connector for Java v5.1.34](https://downloads.mysql.com/archives/c-j/) and copy its JAR file to the following folders:

````bash
cp path/to/mysql-connector-java-5.1.34-bin.jar pattern-1/volumes/apim/repository/components/lib/mysql-connector-java-5.1.34-bin.jar

cp path/to/mysql-connector-java-5.1.34-bin.jar pattern-1/volumes/apim-analytics/repository/components/lib/mysql-connector-java-5.1.34-bin.jar
````

4. Mount a shared network volume to all DC/OS nodes and create three folders for API-M, API-M Analytics and MySQL containers:

````bash
/volumes/wso2/mysql
/volumes/wso2/apim
/volumes/wso2/apim-analytics
````

5. Copy the content found in the [volumes](volumes/) folder into above folders.

6. Install DC/OS CLI by following the instructions found in the [DC/OS CLI Installation Guide](https://docs.mesosphere.com/1.10/cli/install/).

7. Log into DC/OS cluster by executing the below command:

````bash
dcos auth login
````

8. Install Marathon load balancer by executing the below command:

````bash
dcos package install marathon-lb
````

9. Once the Marathon load balancer becomes healthy install MySQL DC/OS service by executing the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/mysql.json
````

Please note that this MySQL Marathon application is only intended for evaluation purposes and for enterprise deployments a production ready RDBMS would need to be used.

10. Once the MySQL Marathon application becomes healthy, install WSO2 API Manager Analytics Marathon application by executing the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/wso2apim-analytics.json
````

11. Once the WSO2 API Manager Analytics Marathon application becomes healthy install WSO2 API Manager Marathon application by executing the below command:

````bash
dcos marathon app add pattern-1/marathon-applications/wso2apim.json
````

11. Now find the IP address of the Marathon load balancer via the DC/OS CLI or UI and add an /etc/hosts entry in the local machine for pointing the hostname ```wso2apim``` for that IP address.

12. Open a web browser and visit the WSO2 API Manager Publisher URL:

````bash
https://wso2apim/publisher
````