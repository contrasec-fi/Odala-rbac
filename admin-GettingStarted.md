# 1 Getting started
This guide shows the requirements for machine, which container system is used and how to configure the components for containers. This material includes a short [video for Admins](https://youtu.be/eG-4yHqA_xs) going over the UI configuration so we recommend viewing that after docker-compose setup.

## 1.1 Requirements
This guide setup is made with virtual machine with Docker-Compose. Below there are the requirements for setup.


### 1.1.1 Host machine requirements:
- OS: Linux CentOS
- RAM: 4GB
- CPU: 2
- Disk space: 16GB
- Let's Encrypt SSL Certificates for HTTPS connection between client and server
- Ports 80 & 443 to be opened

## 1.1 Docker
Docker-Compose is used in this guide.

### 1.2 Install Docker-Compose
1. Download & Install latest stable version of Docker-Compose:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin docker-compose
```
2. Apply permissions:
```
sudo chmod +x /usr/local/bin/docker-compose
```
3. Create symbolic link for Docker-Compose:
```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
4. Test the installation:
```
docker-compose --version
```

# 2 Editing files
To have a working setup for machine, some edits must be done for configuration files:
- `api-umbrella.yml`
- `docker-compose.yml`

## 2.1 Editing the docker-compose.yml file
This guide ...

### 2.1.1 Keyrock
The lines 65-90 in **docker-compose.yml** shows keyrock service's environmental variables and their values. Values with ``*..*`` must be changed for security reasons:
```
65    keyrock:
66            image: fiware/idm:7.8.2
67            hostname: keyrock
68            environment:
69                - IDM_DB_HOST=mysql
70                - IDM_DB_PASS=*DB-PASSWORD*
71                - IDM_DB_USER=root
72                - IDM_ADMIN_USER=*USERNAME*
73                - IDM_ADMIN_EMAIL=*EMAIL*
74                - IDM_ADMIN_PASS=*PASSWORD*
75                - IDM_TITLE=ODALA RBAC
76                - IDM_DB_PORT=3306
77                - IDM_HOST=*accounts.domain.cc*
78                - IDM_DB_NAME=idm
79                - IDM_DB_SEED=true
80                - IDM_DEBUG=true
81                - DEBUG=*
82            depends_on:
83                - mysql
84    mysql:
85            image: mysql:5.7.21
86            hostname: mysql
87            environment:
88                - MYSQL_ROOT_PASSWORD=*DB-PASSWORD*
89            volumes:
90                - ../rbac-mount-files/mysql-data:/var/lib/mysql
```

### 2.1.2 API Umbrella
The lines 40-52 in `docker-compose.yml` shows API Umbrella service's environmental variables and their values. Values with `*..*` must be changed for security reasons:

**NOTE** Make sure to have SSL certificates in `/etc/letsencrypt/live/` folder before proceeding! If folder is empty, certificates are found from `/etc/letsencrypt/archive/` folder.
```
40    api-umbrella:
41        image: fiware/api-umbrella:0.15.5
42        hostname: umbrella
43        ports:
44            - "443:443"
45            - "80:80"
46        environment:
47            - MAXMIND_LICENSE_KEY=**LICENSE**
48        volumes:
49            - ./api-umbrella.yml:/etc/api-umbrella/api-umbrella.yml # configuration file
50            - /etc/letsencrypt/live/*accounts.domain.cc*:/etc/letsencrypt/live/*accounts.domain.cc*/ # SSL certificates
51        depends_on:
52            - umbrella_elasticsearch
```

## 2.2 Editing the api-umbrella.yml file
`api-umbrella.yml` file contains parameters and their values. This configuration file will be mounted into Docker container.
The `hosts` block at line 23 contains SSL parameters and they must be changed. The 41 line must be changed also.
```
23 hosts:
24        - hostname: umbrella.*domain.cc*
25          default: true
26          ssl_cert: /etc/letsencrypt/live/*accounts.domain.cc*/fullchain.pem
27          ssl_cert_key: /etc/letsencrypt/live/accounts.*domain.cc*/privkey.pem
28          http_strict_transport_security: true
.
.
41              host: https://accounts.*domain.cc*
```

# 3 Starting the components
This command starts building containers and starting the components up:
```
docker-compose up -d
```

**NOTE:** If Docker-Compose fails starting containers, make sure to be at folder where `docker-compose.yml' contains!

# 4 Next Steps
Next step is to configure API Umbrella.

[API Umbrella setup](api-umbrella.md)

# 5 References
- [Docker-Compose documentation](https://docs.docker.com/compose/)
- [CentOS documentation](https://docs.centos.org/en-US/docs/)
- [Let's Encrypt documentation](https://letsencrypt.org/docs/) & [Certbot](https://certbot.eff.org/)
