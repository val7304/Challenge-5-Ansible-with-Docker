# # Challenge 6 - Ansible with Docker

Date limite de remise du challenge: le 27 février 2021


## Task 1: Building an Ansible Docker Container

Durée estimée : 30 minutes

L'objectif ici est de produire une image Docker contenant le software Ansible. Cette image est surtout utile pour faire de l'Ansible à partir de Windows.

Travail demandé:

-   Builder le Dockerfile suivant pour produire une image locale. Démarrer un conteneur à partir de cette image et vérifier que Ansible est bien opérationnel. Vérifier que l'image contient bien SSH et python.

```shell
FROM ubuntu:18.04
RUN apt-get update && \
 apt-get install --no-install-recommends -y software-properties-common && \
 apt-add-repository ppa:ansible/ansible && \
 apt-get update && \
 apt-get install -y ansible
RUN echo '[local]\nlocalhost\n' > /etc/ansible/hosts
```

---
### Mise en pratique

* Create a file named Dockerfile (for me, I created in C:\DevOps-Workspace\challenge-6\task1 place)
* Built Dockerfile:

	    student@ROME3-5 MINGW64 /c/DevOps-Workspace/challenge-6/task1
        $ docker build -t task-ansible .
        [+] Building 11.1s (8/8) FINISHED
         => [internal] load build definition from Dockerfile                      0.0s 
         => => transferring dockerfile: 32B                                       0.0s 
         => [internal] load .dockerignore                                         0.0s 
         => => transferring context: 2B                                           0.0s 
         => [internal] load metadata for docker.io/library/ubuntu:18.04          11.0s 
         => [auth] library/ubuntu:pull token for registry-1.docker.io             0.0s 
         => [1/3] FROM docker.io/library/ubuntu:18.04@sha256:ea188fdc5be9b25ca04  0.0s 
         => CACHED [2/3] RUN apt-get update &&  apt-get install --no-install-rec  0.0s 
         => CACHED [3/3] RUN echo '[local]\nlocalhost\n' > /etc/ansible/hosts     0.0s 
         => exporting to image                                                    0.0s 
         => => exporting layers                                                   0.0s 
         => => writing image sha256:5fe5de11cb969d3f9a52a2cef76d819390a0bb57aec8  0.0s 
         => => naming to docker.io/library/task-ansible                           0.0s 

* Check the image name: 
student@ROME3-5 MINGW64 /c/DevOps-Workspace/challenge-6/task1$ docker images
    REPOSITORY                                TAG        IMAGE ID       CREATED          SIZE
    task-ansible                              latest     5fe5de11cb96   52 minutes ago   298MB

* Run a container from image: 

	    student@ROME3-5 MINGW64 /c/DevOps-Workspace/challenge-6/task1$ docker run --rm -it task-ansible
	    #view directory
	    root@bdf3d98cffd2:/# ll
	    total 84
	    drwxr-xr-x   1 root root 4096 Feb 22 14:44 ./
	    drwxr-xr-x   1 root root 4096 Feb 22 14:44 ../
	    -rwxr-xr-x   1 root root    0 Feb 22 14:44 .dockerenv*
	    drwxr-xr-x   2 root root 4096 Jan 18 21:03 bin/
	    drwxr-xr-x   2 root root 4096 Apr 24  2018 boot/
	    drwxr-xr-x   5 root root  360 Feb 22 14:44 dev/
	    drwxr-xr-x   1 root root 4096 Feb 22 14:44 etc/
	    drwxr-xr-x   2 root root 4096 Apr 24  2018 home/
	    drwxr-xr-x   1 root root 4096 May 23  2017 lib/
	    drwxr-xr-x   2 root root 4096 Jan 18 21:03 lib64/
	    drwxr-xr-x   2 root root 4096 Jan 18 21:02 media/
	    drwxr-xr-x   2 root root 4096 Jan 18 21:02 mnt/
	    drwxr-xr-x   2 root root 4096 Jan 18 21:02 opt/
	    dr-xr-xr-x 171 root root    0 Feb 22 14:44 proc/
	    drwx------   2 root root 4096 Jan 18 21:03 root/
	    drwxr-xr-x   1 root root 4096 Jan 21 03:38 run/
	    drwxr-xr-x   1 root root 4096 Jan 21 03:38 sbin/
	    drwxr-xr-x   2 root root 4096 Jan 18 21:02 srv/
	    dr-xr-xr-x  13 root root    0 Feb 22 14:44 sys/
        drwxrwxrwt   1 root root 4096 Feb 22 13:50 tmp/
        drwxr-xr-x   1 root root 4096 Jan 18 21:02 usr/
        drwxr-xr-x   1 root root 4096 Jan 18 21:03 var/

 Check if ansible is installed: 

    root@bdf3d98cffd2:/# ansible --version
    ansible 2.9.18
    config file = /etc/ansible/ansible.cfg
    configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python2.7/dist-packages/ansible    
    executable location = /usr/bin/ansible
    python version = 2.7.17 (default, Sep 30 2020, 13:38:04) [GCC 7.5.0]

Ansible install Python in the same installation

Check if openssh is installed:

    root@bdf3d98cffd2:/# openssl version
    OpenSSL 1.1.1  11 Sep 2018

 




