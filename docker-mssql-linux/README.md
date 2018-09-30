# docker-sandbox/docker-mssql-linux
Create or compose a container running SQL Server on linux host.

## Tools

- Windows 10 or Windows Server 2016
- Docker for Windows (Switched to Linux container)
- VS Code

## Run as single container

### Docker
A docker file is your recipe to instruct docker engine how to build and run your container. This is Dockerfile.txt.

```docker
FROM microsoft/mssql-server-linux:latest

ENV SA_PASSWORD=MyD3f@ultP@ssw0rd1357!
ENV ACCEPT_EULA=Y
ENV MSSQL_PID=Express

EXPOSE 1433
```

### Build
```console
docker build . -t "rdagumampan/mssql-sandbox" -f dockerfile.txt
docker images --all
```
![docker-images](http://www.rdagumampan.com/images/2018/09/docker-mssql-linux-01.png)

### Run
```console
docker run -d -p 1433:1433 --name mssql-sandbox rdagumampan/mssql-sandbox
docker ps --all
```

Note your container id like this **8cd0b350ccb5**.

![docker-images](http://www.rdagumampan.com/images/2018/09/docker-mssql-linux-02.png)

```console
docker inspect rdagumampan/mssql-sandbox
docker inspect -format='{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 8cd0b350ccb5
```

### Test
```console
docker exec -it 8cd0b350ccb5 "bash"
```

```bash
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'MyD3f@ultP@ssw0rd1357!'
```

```bash
> SELECT @@SERVERNAME, @@VERSION;
> GO
> QUIT
```

```bash
> CREATE DATABASE SqlServerLinux;
> GO
> CREATE TABLE [dbo].[Users] (Id INT IDENTITY(1,1) PRIMARY KEY, FirstName NVARCHAR(32), LastName NVARCHAR(255), Age INT);
> GO
> QUIT
```

### Push
```console
docker login --username=rdagumampan
docker push rdagumampan/mssql-sandbox
```

### Cleanup
```console
docker ps --all
docker stop 8cd0b350ccb5
docker rm 8cd0b350ccb5
docker rmi rdagumampan/mssql-sandbox
```

## Run as compose
A compose file allows you provision a complete infrastructure for your application. You can create a db server, web app server, and api app server and contain them in dedicated virtual network. Refer to docker docs for more details. 

```docker
version: '3'

services:
  sql2017:
    image: rdagumampan/mssql-sandbox
    hostname: 'dockersql2017exp'
    container_name: dockersql2017exp
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "1433:1433"
    tty: true
```

### Compose (Run as administrator mode)
```console
docker-compose -f docker-compose.yml up
```

### Access outside your host

- On Windows 10, open up port 1433 IN/OUT
- On Windows Server, open up port 1433 IN/OUT
- On Azure NSG, open up port 1433 IN/OUT

![docker-images](http://www.rdagumampan.com/images/2018/09/docker-mssql-linux-03.png)

### References

- https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker
- https://www.red-gate.com/simple-talk/sysadmin/containerization/provisioning-sql-server-instances-docker/
- https://hub.docker.com/r/microsoft/mssql-tools/
