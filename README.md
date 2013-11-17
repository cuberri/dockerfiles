Description
===========

Some [Dockerfiles](http://docs.docker.io/en/latest/use/builder/) for my personal playground

rok4-with-dataset
=================

## base

Builds a container upon the docker `base` image with a [rok4](http://www.rok4.org/) server configured with a default dataset got from [rok4 website](http://www.rok4.org/). The container exposes its port 9000 (rok4 fastcgi mode default port).

Container built and tested with [coreos](http://coreos.com/) (in Vagrant 1.3.4) with the following `docker version` details :

    $ docker version
    Client version: 0.6.6
    Go version (client): go1.1.2
    Git commit (client): 360a694
    Server version: 0.6.6
    Git commit (server): 360a694
    Go version (server): go1.1.2
    Last stable version: 0.6.6
