---
layout: post
title: MSSQL in Docker
category: Docker
tags: Docker
keywords: Docker
published: true
---

# MSSQL in Docker

## First install Docker
Download docker.dmg and install on Mac
> REF: <https://docs.docker.com/docker-for-mac/>


## Install `MSSQL for linux`
https://hub.docker.com/r/microsoft/mssql-server-linux/


### Download images
 
```
docker login https://registry-1.docker.io/v2/
```
- Username: f########0  
- Password: \<t######a\>

```
docker pull microsoft/mssql-server-linux
```
 
## Run MSSQL container

```
docker run   -e 'ACCEPT_EULA=Y'   -e 'MSSQL_SA_PASSWORD=1A2B3C4D5E6f'  -p 1433:1433  -v mssql_volume:/var/opt/mssql   -d microsoft/mssql-server-linux:latest
```

### Note: 
- This creates a MSSQL container with SA password = **1A2B3C4D5E6f**
- This also creates a data volume container to host data, so even we stop and remove the mssql container, the data volume is still there


### Useful CMD
```
docker image ls  # list images downloaded
docker container ls - a   # list all containers
docker container stop <UID> 
docker container prune
docker volume ls   # view data volume container
```
> REF: https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker?view=sql-server-linux-2017#persist
 
 
## Connect by sqlcmd
```
docker exec -it aac5   /opt/mssql-tools/bin/sqlcmd   -S localhost   -U sa   -P 1A2B3C4D5E6f
```
 
### From outside of container
```
sqlcmd -S 127.0.0.1 -U sa -P 1A2B3C4D5E6f
```
```sql
> select @@version; 
> go
```