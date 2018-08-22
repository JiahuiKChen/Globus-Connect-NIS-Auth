# Globus-Connect-NIS-Auth
This container creates a Globus File Transfer Endpoint, with ypbind/NIS local authentication.
Dockerhub repo: https://hub.docker.com/r/jiahuikchen/globus-server-nis/
Docker pull command: `
docker pull jiahuikchen/globus-server-nis`

Run this container with:
`docker run --privileged -d -t -P --net=host --env GLOBUS_USER=<globus id> --env GLOBUS_PASSWORD=<globus password> jiahuikchen/globus-server-nis`

Replacing `<globus id>` with the Globus ID/username for the Globus organization account the endpoint will be administered by (username only, without the @globusid.org ending). Also replace `<globus password>` with this account's password.

Start a shell in the container to run some commands: `docker exec -it <container name or ID> /bin/bash` 
Replacing `<container id or name>` with the running container's ID or name, which can be found using `docker ps`.

Restart ypbind so NIS may be used: `systemctl restart ypbind`
This command may take a few minutes to execute. Sometimes there's an error when this command is run, just keep running the command and it will eventually execute.

Create Globus Endpoint: `globus-connect-server-setup`

Enable file transfers: `service globus-gridftp-server restart`

After these commands the endpoint is be able to receive and send files, if NIS is acceptable for local authentication. The endpoint's directories of sharing must also have write/read permissions for file transfers to work.
