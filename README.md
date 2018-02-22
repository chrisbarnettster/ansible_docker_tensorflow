# ansible_docker_tensorflow

Basic details for setting up ansible, docker and tensorflow

:bowtie: I run a [Scientific Linux](https://www.scientificlinux.org/) 7 system - this is based on RedHat Linux.
Everything I do is with Scientific Linux 7 in mind. Apologies other Linux afficionados. Please clone and mod to suit your preferences. 

:sunny: I don't expect this to break your system. Nevertheless, run at your own risk. 

:white_check_mark: the main steps are indicated by check marks

## Ansible 

:book: Resources:
- https://www.ansible.com/resources/get-started
- http://docs.ansible.com/ansible/latest/index.html

:white_check_mark:

`sudo yum install epel-release -y`

`sudo yum install ansible -y`

:book:
By default the ansible hosts file is empty (/etc/ansible/hosts). Do cool stuff here if you like (http://docs.ansible.com/ansible/latest/intro_inventory.html)
Localhost works by default
For example ping your localhost 
`ansible localhost -m ping`
expected output - ping success!:

:white_check_mark: 
```
$ ansible localhost -m ping
 [WARNING]: Could not match supplied host pattern, ignoring: all

 [WARNING]: provided hosts list is empty, only localhost is available

localhost | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}

```

## Docker and nvidia-docker install

:white_check_mark: If you want to use sudo permission:
```
ansible-playbook playbooks/docker.playbook --ask-become-pass

ansible-playbook playbooks/nvidia-docker.playbook --ask-become-pass

```
If you are logged in as root:
```
ansible-playbook docker.playbook
ansible-playbook nvidia-docker.playbook
```

### test out docker hello-world
:white_check_mark: Try out docker hello world for fun:
`sudo docker run hello-world`


### run on other hosts
if you know a bit about ansible, run on for example 10 hosts in parallel in the testing group 
ansible-playbook gpu.playbook -f 10 --extra-vars "variable_host=testing"

## Tensorflow: pull and run docker images

:white_check_mark: CPU only tensorflow
```
sudo docker run -it -p 8888:8888 tensorflow/tensorflow
```

:white_check_mark: GPU supported
```
sudo nvidia-docker run -it -p 8888:8889 tensorflow/tensorflow:latest-gpu
```


## tensorflow
Open a browser and browse to the links shown when starting the docker tensorflow image. 
These link will look similar to the one below:
```
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8888/?token=f1db8a46748acf11eb3373b81c20f0ed6a56dd3be5e2166f
```

:white_check_mark: Open up the notebooks
```
google-chrome localhost:8888
google-chrome localhost:8889
```

If you're not familiar with Jupyter see https://jupyter.readthedocs.io/en/latest/running.html

:white_check_mark: Play with existing notebooks from tensorflow and upload the example notebook in notebooks/tensorflow_notebook.ipynb (available from https://raw.githubusercontent.com/chrisbarnettster/ansible_docker_tensorflow/master/notebooks/tensorflow_notebook.ipynb)


# Issues
## errors installing nvidia-docker
e.g.
```
TASK [Install nvidia docker] **********************************************************************************************************************
failed: [localhost] (item=[u'nvidia-docker2']) => {"changed": false, "item": ["nvidia-docker2"], "msg": "Failure talking to yum: failure: repodata/repomd.xml from libnvidia-container: [Errno 256] No more mirrors to try.\nhttps://nvidia.github.io/libnvidia-container/centos7/x86_64/repodata/repomd.xml: [Errno -1] repomd.xml signature could not be verified for libnvidia-container"}
```
The nvidia repo has several sections and multiple GPG key requirements. I haven't got this just right yet, so ansible is supposed to ignore GPG keys. If this doesn't happen then `sudo yum install nvidia-docker2 --nogpgcheck`

## docker breaks after nvidia install
this happened to me when testing on a system with no nvidia card.
after removing nvidia-cuda2 I still wasn't able to restart docker
see https://github.com/moby/moby/issues/23089
One fix is to remove or move /var/lib/docker and reinstall docker

```
mv /var/lib/docker /var/lib/docker.old
ansible-playbook playbooks/docker.playbook --ask-become-pass
```
# Reference material and reading
- https://www.tensorflow.org/programmers_guide/using_gpu
- https://learningtensorflow.com/lesson10/
