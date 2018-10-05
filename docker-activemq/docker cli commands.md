
netstat  -ano|find ":<PORT_NO>"

[System.IO.Directory]::GetFiles("\\.\\pipe\\") | Select-String "docker"


Setting up portainer
-----------------
Fixing volume
ERROR: for docker_docker_1  Cannot create container for service docker: b'Mount denied:\nThe source path "\\\\var\\\\run\\\\docker.sock:/var/run/docker.sock"\nis not a valid Windows path'

ERROR: for docker  Cannot create container for service docker: b'Mount denied:\nThe source path "\\\\var\\\\run\\\\docker.sock:/var/run/docker.sock"\nis not a valid Windows path'
ERROR: Encountered errors while bringing up the project.
Information

$Env:COMPOSE_CONVERT_WINDOWS_PATHS=1
Restart CLI!

Attach absolute path
ERROR: for f9d809c6b9b4_docker-activemq_portainer_1  Cannot create container for service portainer: invalid volume specification: '7a93e4752c0dcceeb8ed915b25ee6302ab98e8c4085edd13f4f5fe359d0ba2f9:
iner_data/data portainer/portainer:rw': invalid mount config for type "volume": invalid mount path: 'portainer_data/data portainer/portainer' mount path must be absolute                           

