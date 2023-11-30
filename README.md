# docker compose for nginx proxy manager, postgres, pgadmin and a random wiki

This project demonstrates how to start nginx proxy manager, postgres, pgadmin and a random wiki within docker.

# Start services

Edit your /etc/hosts and add

```
127.0.0.1 pgadmin.example.com wiki.example.com
```

then start the containers:

```bash
docker compose up -d
```

# Post installation steps

The nginx proxy manager doesn't support autodiscovery for docker based http services.

So you have to configure this by yourself.

## Option 1 - UI

Open http://localhost:8080/ and login with

```
Email:    admin@example.com
Password: changeme
```

complete the setup process.

### Setup pgadmin

Click "Proxy Host" and add the following information:

```
Domain Names: pgadmin.example.com
Scheme: http
Forward Hostname: pgadmin
Forward Port: 80
```

### Setup wiki

Click "Proxy Host" and add the following information:

```
Domain Names: wiki.example.com
Scheme: http
Forward Hostname: wiki
Forward Port: 3000
```

## Option 2 - shell

Execute

```bash
docker run --rm -v $PWD/data/nginx-proxy-manager/:/mnt/dbfile alpine/sqlite /mnt/dbfile/database.sqlite "INSERT INTO proxy_host VALUES(1,'2023-11-30 19:04:19','2023-11-30 19:04:19',1,0,'[\"pgadmin.example.com\"]','pgadmin',80,0,0,0,0,0,'','{\"letsencrypt_agree\":false,\"dns_challenge\":false,\"nginx_online\":true,\"nginx_err\":null}',0,0,'http',1,'[]',0,0);"
docker run --rm -v $PWD/data/nginx-proxy-manager/:/mnt/dbfile alpine/sqlite /mnt/dbfile/database.sqlite "INSERT INTO proxy_host VALUES(2,'2023-11-30 19:08:02','2023-11-30 19:08:02',1,0,'[\"wiki.example.com\"]','wiki',3000,0,0,0,0,0,'','{\"letsencrypt_agree\":false,\"dns_challenge\":false,\"nginx_online\":true,\"nginx_err\":null}',0,0,'http',1,'[]',0,0);"
```

to create the proxy host entries for nginx proxy manager.

# Login to pgAdmin

Access pgAdmin via http://pgadmin.example.com and use the credentials:

```
Email: root@oglimmer.de
Password: postgres
```

# Login to wiki

Go to http://wiki.example.com and complete the setup process.

