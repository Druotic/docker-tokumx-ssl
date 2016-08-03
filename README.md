# Overview: TokuMX+SSL

  A dockerfile/docker image for running the community version of TokuMX within  
  a container, compiled with TLS/SSL enabled. Ubuntu is used for the base  
  image. This is intended for testing purposes only, NOT production use.

# Usage

### Pull the image

```
docker pull druotic/tokumx-ssl
```

### Run the container

  For necessary config file options, see toku/mongo docs. But, at a bare minimum,  
  you'll likely want to include `sslOnNormalPorts` and `sslPEMKeyFile`.

Sample usage:
```
docker run -e TOKU_HUGE_PAGES_OK=1 -p 27017:27017 -v `pwd`/tokumx-config:/etc/tokumx-config -v `pwd`/certs:/var/private/ssl/certs druotic/tokumx-ssl --config /etc/tokumx-config/tokumx.conf
```

# Building

### Ensure plenty of space for compiling Toku

  I used Fedora 22 when building this image and found that there was a 10G  
  limit on containers by default. This was necessary to increase that  
  limit. This may not be required depending on your ditribution and aufs  
  vs device mapper, etc. You can check `docker info` to see if your "Base  
  Device Size" is 10G. Note: The resulting image will not be 10G+.

```
docker daemon --storage-opt dm.basesize=20G
```

### Build the image

Sample build (with tagging):

```
docker build --build-arg VERSION=2.0.2 -t druotic/tokumx-ssl:2.0.2 .
```
