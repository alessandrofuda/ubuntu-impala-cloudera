# Ubuntu with Cloudera Impala ODBC Driver
Ubuntu Linux machine ready for Cloudera/Impala db connection testing.


> ## To Create && Start from anywhere
> ```docker run -it alessandrofuda/ubuntu-impala``` (create & start new container)

## To Start existing container (already created)
```
# find container-id
docker ps -a

docker container start <container-id>

# is started ?
docker ps 

# go inside..
docker exec -it <container-id> bash
```

## To fixing and Push running container to Docker Hub
```
# get container ID
docker ps -a

# create new image on local machine
docker commit <container-ID> ubuntu_impala:latest

# tag for docker hub
docker tag ubuntu_impala:latest alessandrofuda/ubuntu_impala:210128.0 (example)

# push to docker hub
docker push alessandrofuda/ubuntu_impala:210128.0
```

## Image building procedure:

``` docker run -it --name <optional-name> ubuntu:18.04  ```

Inside docker container:
```
FROM: ubuntu:18.04

apt update && apt upgrade -y
apt install -y unixodbc
apt install nano
apt install -y sasl2-bin && apt install -y libsasl2-dev # for Cyrus-sasl libs

# get .deb Driver file (by Cloudera) && import in root docker container
# install drivers
dpkg -i clouderaimpala2.6.11.1011-2.deb

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/cloudera/impalaodbc/lib/64

export ODBCINI=/opt/cloudera/impalaodbc/Setup/odbc.ini && \
export ODBCSYSINI=/opt/cloudera/impalaodbc/Setup && \
export CLOUDERAIMPALAINI=/opt/cloudera/impalaodbc/lib/64/cloudera.impalaodbc.ini

cp /opt/cloudera/impalaodbc/Setup/odbc.ini ~


# optional
apt install git # --> export to github
```

## Specs:
- ubuntu 18.04 
-  64bit 
- UnixODBC
- libsasl libraries: cyrus-sasl, cyrus-sasl-gssapi, cyrus-sasl-plain
- Driver installed: clouderaimpalaodbc v. 2.6.11.1011-2 
- Driver installation path: /opt/cloudera/impalaodbc/
- Configuration File: ~/odbc.ini
- Kerberos authentication


## References
Driver: https://www.cloudera.com/downloads/connectors/impala/odbc/2-6-11.html

Doc: https://docs.cloudera.com/documentation/other/connectors/impala-odbc/2-6-11/Cloudera-ODBC-Driver-for-Impala-Install-Guide.pdf
