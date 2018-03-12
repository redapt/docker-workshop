## Docker install

This document will show you how to install Docker on Linux (either Debian-based or Red Hat-based).

### Debian-based distros

*Note: For this install, I will be using Ubuntu 16.04 LTS (Xenial Xerus). Docker requires a 64-bit version of Ubuntu as well as a kernel version equal to or greater than 3.10. My system satisfies both requirements.*

* Setup the docker repo to install from:
```
$ sudo apt-get update -y
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
$ echo "deb <nowiki>https://apt.dockerproject.org/repo ubuntu-xenial main</nowiki>" | sudo tee /etc/apt/sources.list.d/docker.list
$ sudo apt-get update -y
```

Make sure you are about to install from the Docker repo instead of the default Ubuntu 16.04 repo:
```
$ apt-cache policy docker-engine
```

The output of the above command show look something like the following:
```
docker-engine:
  Installed: (none)
  Candidate: 1.11.2-0~xenial
  Version table:
     1.11.2-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.11.1-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.11.0-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
```

* Install docker:
```
$ sudo apt-get install -y docker-engine
```

### Red Hat-based distros

*Note: For this install, I will be using CentOS 7 (release 7.2.1511). Docker requires a 64-bit version of CentOS as well as a kernel version equal to or greater than 3.10. My system satisfies both requirements.*

* Install Docker (the fast way):
```
$ sudo yum update -y
$ curl -fsSL <nowiki>https://get.docker.com/</nowiki> | sh
```

* Install Docker (via a yum repo):
```
$ sudo yum update -y
$ sudo pip install docker-py
$ cat << EOF > /etc/yum.repos.d/docker.repo
[dockerrepo]
name=Docker Repository
baseurl=<nowiki>https://yum.dockerproject.org/repo/main/centos/7/</nowiki>
enabled=1
gpgcheck=1
gpgkey=<nowiki>https://yum.dockerproject.org/gpg</nowiki>
EOF

$ sudo rpm -vv --import <nowiki>https://yum.dockerproject.org/gpg</nowiki>
$ sudo yum update -y
$ sudo yum install docker-engine -y
```

===Post-installation steps===

*Note: The following steps should be run on either your Debian-based or Red Hat-based distros.*

* Check on the status of docker:
```
$ sudo systemctl status docker

● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2016-07-12 12:31:08 PDT; 6s ago
     Docs: https://docs.docker.com
 Main PID: 3392 (docker)
   CGroup: /system.slice/docker.service
           ├─3392 /usr/bin/docker daemon -H fd://
           └─3411 docker-containerd -l /var/run/docker/libcontainerd/docker-containerd.sock --runtime docker-runc --start-timeout 2m
```

* Make sure the docker service automatically starts after a machine reboot:
```
$ sudo systemctl enable docker
```

* Execute docker without `sudo`:
```
$ sudo usermod -aG docker $(whoami)
```
Log out and log back in to use docker without `sudo`.

* Check that docker has been successfully installed and configured:
```
$ docker run hello-world
...
This message shows that your installation appears to be working correctly.
...
```

That's it! You should now have Docker successfully installed.
