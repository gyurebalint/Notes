
Nanode
ubuntu-eu-central_nanode
psw: Mindenteltudszerni9876
	Users
	radgaul
	bbbb0000
Set up FTP connection from total commander
TODO: Setup ssh and firewall

company, smtp server, host static files

commands
$ logout
$ apt-get update
$ apt-get upgrade

Docker install:
``` cmd
sudo apt remove docker docker-engine docker.io
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```
[

## Starting and Testing Docker
After Docker Engine is installed, start Docker and verify everything is working by running a test image.  
Optionally configure Docker to start when the server boots up. This is recommended if you intend on running a production application within this Docker installation.
Verify Docker is correctly installed by running the “hello-world” image.

    ```
    sudo systemctl start docker
    sudo systemctl enable docker
    sudo systemctl enable containerd
    sudo docker run hello-world
    ```  
If successful, Docker should download and run the hello-world image and output a success message. Among other text, the output should include a message similar to the following:
    ```
    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    ```

Install nginx proxy server
	login: gyurebalint@gmail.com
	pswd: Mindenteltudszerni9876
Portainer
radgaul
Mindenteltudszerni9876

# Portainer
radgaul
Mindenteltudszerni9876

Portainer calls the docker-composer files *stacks*
VaultWarden
gyurebalint@gmail.com
Mindenteltudszerni9876

# .NET application
docker build -t identity -f AspNetCoreIdentity/Dockerfile .

# Vault Hashicorp
version: "1"
services:
    vault:
        image: vault:latest
        container_name: vault
        networks:
          - nginxproxymanager_default
        volumes:
            - /home/docker/vault:/data/
        ports:
            - 8200:80
        restart: unless-stopped
        
networks:
  nginxproxymanager_default:
    external: true
    
Unseal Key: eZvpvrI5cHsmPDMr3w0bObugGa8gRTOJOBqHCg4Mesk=
Root Token: hvs.xkg7yyTkvgTWUfpiD7i2imwR

- Secrets
	- Enabling a Secrets Engine
	- Adding a secret
- Auth
	- Enabling an Auth Method
	- Managing your Auth Method
- Policies
	-   Choosing a policy type
	-   Creating a policy
	-   Deleting your policy
	-   Other types of policies
- Tools
	-   Wrapping data
	-   Lookup wrapped data
	-   Rewrapping your data
	-   Unwrapping your data
How to manage admin side of Vault?
Policies, users/groups, auth etc?

172.104.247.225 hermes.com      hermes