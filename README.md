# docker-workshop
Introduction to Docker

**Docker** is an open-source project that automates the deployment of applications inside software containers. Quote of features from docker web page:
> Docker containers wrap up a piece of software in a complete filesystem that contains everything it needs to run: code, runtime, system tools, system libraries – anything you can install on a server. This guarantees that it will always run the same, regardless of the environment it is running in. [Docker.com](https://www.docker.com/what-docker)

## Docker directives

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

This section will describe some of the common Dockerfile commands (or "directives").

### USER

```
$ cat << EOF > Dockerfile
# Non-privileged user entry
FROM centos:latest
MAINTAINER xtof@example.com

RUN useradd -ms /bin/bash xtof
USER xtof
EOF
```

*Note: The use of `MAINTAINER` has been deprecated in newer versions of Docker. You should use `LABEL` instead, as it is much more flexible and its key/values show up in `docker inspect`. From here forward, I will only use `LABEL`.''

```
$ docker build -t centos7/nonroot:v1 .
$ docker exec -it <container_name> /bin/bash
```

We are user "xtof" and are unable to become root. The workaround (i.e., how to become root) is like so:

```
$ docker exec -u 0 -it <container_name> /bin/bash
```

*NOTE: For the remainder of this section, I will omit the `$ cat << EOF > Dockerfile` part in the examples for brevity.*
