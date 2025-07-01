# Homepage.dev config files


# Installation

**Please read full document before continuing!!**



![alt text](https://i.imgur.com/2KGJcup.png)


## Docker

Download this repository to where you will be hosting your docker config files. Example: /srv/dockerdata/homepage/config
RUN:
```
cd /your/path/to/docker/configs/ | EXAMPLE: /srv/dockerdata/homepage/config | This will be the same as your volume
```
```
git clone https://github.com/SleepingPanda4/HomePageConfig.git
mv HomePageConfig/* ./ && mv HomePageConfig/.env ./
rm -r HomePageConfig
```
#### docker-compose.yaml/portainer stack compose
```
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    env_file: .env #ADD where you are putting config file | EXAMPLE: /srv/dockerdata/homepage/config/.env
    ports:
      - 3000:3000
    volumes:
      - /srv/dockerdata/homepage/config:/app/config # Make sure your local config directory exists | CHANGE THIS IF DIRECTORY IS DIFFERENT | MUST BE THE SAME DIRECTORY YOU DOWNLOADED REPOSITORY TO
      - /var/run/docker.sock:/var/run/docker.sock # (optional) For docker integrations
    environment:
      HOMEPAGE_ALLOWED_HOSTS: 192.168.1.XXX:3000 # ADDRESS FOR HOMEPAGE | EXAMPLE: 192.168.1.XXX:3000 or homepage.myhomedomain.com See gethomepage.dev/installation/#homepage_allowed_hosts

    restart: unless-stopped
```

**You must edit the .env file before you launch the docker. Anytime you change the .env file you must redeploy your docker compose stack.**
**Please go through and make sure all commented variables in "services.yaml" and "docker.yaml" match your variables**


### Remote Docker Sock | docker.yaml config
Change docker.yaml to fit your IP addresses for your docker instance.
```
# FORMAT:
variable-instance-name:
  host: # docker-ip | EXAMPLE 127.0.0.1
  port: 2375 #This will always be the same
```

If running homepage on seperate machine from your dockers you have to run: 

```
sudo systemctl edit docker.service
```

```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```


### Services.yaml
#### Basic Format:
```
- Directory Name:
  - Thing1 Name:
      icon: icon.png
      href: https://app.ip.address:port/
  - Thing2 Name:
      icon: icon.png
      href: https://app.ip.address:port/
# EXAMPLE:
- Plex Server:
    - Plex:
        icon: plex.png
        href: https://app.plex.tv/desktop/
        description: Media Server
    - Tautulli:
        icon: https://avatars.githubusercontent.com/u/34385001?v=4
        href: http://192.168.1.XXX:8181/
        description: Tautulli Server
```
You can make as many directories as you want.
#### Docker Status (Red/Orange/Green Dot) Format:
```
# Goes beneath "description:" tag
        server: my-docker # Change to your docker.yaml variable-instance-name
        container: ContainerName # Change to your container name in docker/Portainer
        statusStyle: dot # Will show Green/Orange/Red status dot

# EXAMPLE:
    - Plex:
        icon: plex.png
        href: https://app.plex.tv/desktop/
        description: Media Server
        server: my-docker # Change to your docker.yaml variable-instance-name
        container: Plex-Media-Server # Change to your container name in docker
        statusStyle: dot # Will show Green/Orange/Red status dot
        widget:
          type: plex
          url: http://192.168.1.XXX:32400 # YOUR PLEX IP
          key: "111111111111111111111111"  # YOUR PLEX API KEY
```
More Info Here: https://gethomepage.dev/configs/settings/#show-docker-stats
#### Portainer Example:
```
    # PORTAINER TEMPLATE | TO EDIT: HIGHLIGHT AND PRESS CTRL+/
        - Portainer Media Server:
            icon: si-portainer-#13BEF9
            href: https://{{HOMEPAGE_VAR_PORTAINER_IP}}:9443/ # CHANGE VARIABLE TO YOUR .env PORTAINER VARIABLE, or REPLACE WITH YOUR IP FOR PORTAINER
            description: Portainer
            server: my-docker # Change to your docker.yaml variable-instance-name
            container: portainer # Change to your container name in docker | DEFAULT WILL BE "portainer"
            statusStyle: dot # Will show Green/Orange/Red status dot
            widget:
              type: portainer
              url: https://{{HOMEPAGE_VAR_PORTAINER_IP}}:9443
              env: 3
              key: {{HOMEPAGE_VAR_PORTAINER_API_KEY}} # CHANGE TO API VARIABLE OR ADD API KEY INSTEAD
```



