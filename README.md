# docker-sandbox
A collection of docker files for daily developer needs

## Build
docker build . -t "rdagumampan/mssql-sandbox" -f dockerfile.txt
docker images

## Run
docker run -d -p 1433:1433 --name mssql-sandbox rdagumampan/mssql-sandbox
docker ps

docker inspect rdagumampan/mssql-sandbox
docker inspect -format='{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 8cd0b350ccb5

## Run your first T-SQL query
docker exec -it 8cd0b350ccb5 "bash"
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Denmark2018'

> SELECT @@SERVERNAME, SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),SERVERPROPERTY('MachineName'),SERVERPROPERTY('ServerName');
> GO
> QUIT

> CREATE DATABASE SqlServerLinux;
> GO
> CREATE TABLE [dbo].[Users] (Id INT IDENTITY(1,1) PRIMARY KEY, FirstName NVARCHAR(32), LastName NVARCHAR(255), Age INT);
> GO
> QUIT

## Push
docker login --username=rdagumampan
docker push rdagumampan/mssql-sandbox

# Cleanup
docker ps
docker stop 8cd0b350ccb5
docker rm 8cd0b350ccb5

## Compose (Run as administrator mode)
docker-compose -f docker-compose.yml up

## Access outside your host
On Windows 10, open up port 1433 IN/OUT
On Windows Server, open up port 1433 IN/OUT
On Azure NSG, open up port 1433 IN/OUT

#References
https://hub.docker.com/r/microsoft/mssql-tools/
https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker
