# How to use this image
- Since no two users of Keepalived are likely to configure it exactly alike, this image does not come with any default configuration.
- Please refer to "https://keepalived.org/doc/" documentation on the subject of configuring Keepalived for your needs.
- Docker repository https://hub.docker.com/r/nghiahl/keepalived
## Create a ```Dockerfile```
```
FROM nghiahl/keepalived:2.2.8
COPY path/to/config /etc/keepalived/keepalived.conf
```
## Build the container
```
$ docker build -t local/keepalived:v2.2.8 .
```
## Run the container
```
$ docker run -d --name keepalived local/keepalived:v2.2.8
```
