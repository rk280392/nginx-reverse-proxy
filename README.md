## Context

Deploy two web applications running on docker containers inside an EC2 Instance. Map web applications to respective domain names. Add letsencrypt SSL certificates so that sites are accessible on HTTPS.

## Diagram

![Reverse-proxy-diagram](https://user-images.githubusercontent.com/43488291/189527405-d892f904-32b4-4be8-8432-846bcb5058ff.png)

## Theory

Reverse proxy is a server implementation which is placed in front of web servers. It forwards client requests to those servers depending on defined conditions in its implementation. Basically it hides servers from end users, which increases security, performance and reliability.

## Implementation:

   - site1 and site2 directories contains simple go webapps running on port 30008

   - reverse proxy directory contains nginx implementation which is exposed on port 80

   - nginx-certbot directory runs dummy nginx and certbot containers which are responsible for getting the SSL certificates.


###   Steps:

In order to allow HTTPS connection we will need to add certificates. This is bit tricky in case of docker containers.

Please refer this article for more info:

https://pentacent.medium.com/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71

The idea is to create a dummy certificate to start nginx inside docker container, then run certbot to create new valid certificates.

Certificates will be stored in /data/certbot/ directory. We will later mount this directory while running nginx-reverse-proxy

```shell
      1 - git clone https://github.com/rk280392/nginx-reverse-proxy.git
      2 - cd nginx-reverse-proxy/nginx-certbot
      3 - sudo cp -r data /data
      4 - Update domain names in init-letsencrypt.sh
      5 - Update domain names in /data/nginx/app.conf
      6 - sudo ./init-letsencrypt.sh
```

After successfully running these, you should have generated certificates. It will be stored in /data/certbot/conf directory by default if not changed.

```shell

      7 - Run `docker stop <container-id>`  To stop the container created above as we don't need it after certificate is generated. 
      8 - cd ../site1
      9 - docker-compose up --force-recreate -d
     10 - cd ../site2
     11 - docker-compose up --force-recreate -d
     12 - `docker ps` Should show both the sites up and running
     13 - cd ../reverse-proxy
     14 - Update the docker-compose.yaml file with correct volume mounts.
     15 - Update the default.conf with correct domain names, proxy_pass and ssl certificates.
     16 - docker-compose up --force-recreate -d
```

If everything works fine, you can open the site on https://<domain-name> and it should work fine.
