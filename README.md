## CONTEXT

Deploy two web applications running on docker containers inside an EC2 Instance. Map web applications to respective domain names. Make sure they are accessible on secure HTTPS connection only.

## Diagram



## Implementation

   1 - site1 and site2 directories contains simple go webapps running on port 30008
   2 - reverse proxy directory containes nginx implementation which is exposed on port 80


