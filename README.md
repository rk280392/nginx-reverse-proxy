## CONTEXT

Deploy two web applications running on docker containers inside an EC2 Instance. Map web applications to respective domain names. Make sure they are accessible on secure HTTPS connection only.

## Diagram

![Reverse-proxy-diagram](https://user-images.githubusercontent.com/43488291/189527405-d892f904-32b4-4be8-8432-846bcb5058ff.png)


## Implementation

   1 - site1 and site2 directories contains simple go webapps running on port 30008
   2 - reverse proxy directory containes nginx implementation which is exposed on port 80

