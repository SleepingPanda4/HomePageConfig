# Homepage.dev config files


# Installation

## Docker

Download this repository to where you will be hosting your docker config files. Example: /srv/dockerdata/homepage/config
RUN:
```
cd /your/path/to/docker/configs/ | EXAMPLE: /srv/dockerdata/homepage/config | This will be the same as your volume
git clone https://github.com/SleepingPanda4/HomePageConfig.git
mv HomePageConfig/* ./
rm -r HomePageConfig
```
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
      #NOT NEEDED IF USING .env FILE!
      HOMEPAGE_ALLOWED_HOSTS: 192.168.1.1:3000 # required, may need port. See gethomepage.dev/installation/#homepage_allowed_hosts

    restart: unless-stopped
```

**You must edit the .env file before you launch the docker. Anytime you change the .env file you must redeploy your docker compose stack.**
**Please go through and make sure all commented variables in "services.yaml" and "docker.yaml" match your variables**



Change docker.yaml to fit your IP addresses for your docker instance. If running homepage on seperate docker instance you have to run: 

```
sudo systemctl edit docker.service
```

```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```



