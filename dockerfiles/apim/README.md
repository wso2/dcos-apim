# WSO2 API Manager Dockerfile for DC/OS
This section defines step-by-step instructions for building the WSO2 API Manager 2.1.0 Docker image for DC/OS.

## How to build the Docker image
1. Checkout this repository into your local machine using the following git command.
```
git clone https://github.com/wso2/dcos-apim.git
```

2. Switch to the required release tag and then to the API-M Dockerfile folder:
```
cd dcos-apim/
git tag # view existing tags
git checkout tags/<version> # checkout the required release tag
cd dockerfiles/apim
```

>The local copy of the above directory will be referred to as `APIM_DOCKERFILE_HOME` from this point onwards.

3. Add the JDK and WSO2 API Manager distributions to `<APIM_DOCKERFILE_HOME>/files`
- Download [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
and copy that to `<APIM_DOCKERFILE_HOME>/files`.
- Download the WSO2 API Manager 2.1.0 distribution (http://wso2.com/api-management/try-it/)
and copy that to `<APIM_DOCKERFILE_HOME>/files`. <br>
>Please refer to [WSO2 Update Manager documentation](https://docs.wso2.com/display/ADMIN44x/Updating+WSO2+Products)
in order to obtain latest bug fixes and updates for the product.

4. Build the Docker image.
- Navigate to `<APIM_DOCKERFILE_HOME>` directory. <br>
  Execute `docker build` command as shown below.
    + `docker build -t wso2am-dcos:2.1.0 .`

## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
