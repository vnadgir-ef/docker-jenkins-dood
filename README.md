![](http://i.imgur.com/KC6TAD3.png)
Jenkins with DooD (Docker outside of Docker)
-------------------------------------------
A [Jenkins](http://jenkins-ci.org) container capable of using [Docker](http://docker.com), so you can Docker while you Docker.

![](http://i.imgur.com/MEFY0F5.gif)

> What is the most resilient parasite? Bacteria? A virus? An intestinal worm? 
> An idea. Resilient... highly contagious. Once an idea has taken hold of the 
> brain it's almost impossible to eradicate. An idea that is fully formed - 
> fully understood - that sticks; right in there somewhere.

*Cobb ("Inception" by Mr. Christopher Nolan), 2010*

## First of all, WTF is DooD supposed to mean?
Long story short, *DooD* is the opposite of [DinD](https://blog.docker.com/2013/09/docker-can-now-run-within-docker/) and whereas the latter includes a whole Docker installation inside of it, the former just uses its underlying host's Docker installation. Don't thank me, thank Mouat for his contribution on this matter.

This Docker container is highly based on the one explained
at the [article by Adrian Mouat](http://container-solutions.com/2015/03/running-docker-in-jenkins-in-docker/) which explains the DooD approach in order to
have a Jenkins container that is able to use [Docker](http://docker.com).

## How to use it
### If you wish to obtain the image, you just have to ...
```bash
docker pull axltxl/jenkins-dood
```

###However, if you wish to build it instead ...
```bash
git clone https://github.com/axltxl/docker-jenkins-dood.git && cd jenkins-dood
docker build -t jenkins-mood .
```

###Now, time to have fun with it...
```bash
docker run -d -v $(which docker):/bin/docker.io \ 
              -v /var/run/docker.sock:/var/run/docker.sock \
              -v /path/to/your/jenkins/home:/var/jenkins_home \
              -p 8080:8080 \
              axltxl/jenkins-dood
```

###Advantages
* No `privileged` mode needed
* Simpler, Jenkins will use it underlying host's Docker installation
* Ability to reuse the image cache from the host
* Any settings in the host's Docker daemon will apply to Jenkins container as well
* Easier to set up, you just need to map the host's Docker executable and daemon socket onto the container

###Disadvantages
* Technically speaking, `jenkins` user has root access to the host
* If you want to manage a complete clean Docker environment inside your Jenkins, this one's not for you

###The `sudo` workaround
According to the reading by Mouat:
> We need to give the jenkins user sudo privileges in order to be able to run 
> Docker commands inside the container. Alternatively we could have added the 
> jenkins user to the Docker group, which avoids the need to prefix all Docker 
> commands with ‘sudo’, but is non-portable due to the changing gid of the group.

The problem with this is that it is actually necessary to `sudo docker` inside the container in order to use Docker, although it seems like a minor thing, it could become an issue for any tools expecting Docker to be available as it is supposed to (Jenkins plugins *cough* *cough*), since they don't expect to `sudo docker` at all. In order to **fix** this and still obtain a fairly smooth Docker experience inside the container, blablabla you'll just have to map you `docker` executable to the container's `/bin/docker.io`.

##Copyright and Licensing
Copyright (c) 2015 Alejandro Ricoveri

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.