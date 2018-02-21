# ansible_docker_tensorflow

Basic details for setting up ansible, docker and tensorflow

I run a Scientific Linux 7 system - this is based on RedHat Linux.
Everything I do is with Scientific Linux 7 in mind. Apologies other Linux afficionados. Please clone and mod to suit your preferences. 

## ansible 
TODO

## docker and nvidia-docker install

```
ansible-playbook docker.playbook
ansible-playbook nvidia-docker.playbook

```
### run on other hosts
if you know a bit about ansible, run on for example 10 hosts in parallel in the testing group 
ansible-playbook gpu.playbook -f 10 --extra-vars "variable_host=testing"

## pull and run docker images
CPU only tensorflow
```
sudo docker run -it -p 8888:8888 tensorflow/tensorflow
```
GPU supported
```
sudo nvidia-docker run -it -p 8888:8889 tensorflow/tensorflow:latest-gpu
```


## tensorflow
browse to jupyter 
play with existing notebooks from tensorflow and upload the example notebook in notebooks/tensorflow_notebook.ipynb

# Reference material and reading
- https://www.tensorflow.org/programmers_guide/using_gpu
- https://learningtensorflow.com/lesson10/
