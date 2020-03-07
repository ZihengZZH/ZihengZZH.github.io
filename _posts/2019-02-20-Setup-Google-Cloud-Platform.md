---
layout: post
title: "How to setup Googld Cloud Platform"
date: 2019-02-20
---

## create a Google Cloud Platform account


## create a Virtual Machine (VM) instance


## generate ssh keys (public / private)

```
ssh-keygen -t rsa -f ~/.ssh/<KEY_FILENAME> -C <USERNAME>
```
where:
* ```<KEY_FILENAME>``` is the name for the SSH key files
* ```<USERNAME>``` is the user for whom the SSH key will be applied

## locate an SSH key

* public key file ```~/.ssh/<KEY_FILENAME>.pub```
* private key file ```~/.ssh/<KEY_FILENAME>```


## format and copy public key to GCP

```ssh-rsa <KEY_VALUE> google-ssh {"userName":"<USERNAME>"}```

where:
* ```<KEY_VALUE>``` is the public key value
* ```<USERNAME>``` is the user for this specific SSH key

copy to metadata


### add to known hosts

just in case

```ssh-keyscan -H xxx.xxx.xxx.xxx >> ~/.ssh/known_hosts/```


## access the vm with private key

```ssh -i <PATH_TO_PRIVATE_KEY> <USERNAME>@<EXTERNAL_IP_ADDRESS>```
where:
* ```<PATH_TO_PRIVATE_KEY>``` is the path to the private SSH key file
* ```<USERNAME>``` is the user for this specific SSH key
* ```<EXTERNAL_IP_ADDRESS>``` is the external IP address for the VM instance


## check free space on VM

```$ df -k```

## install conda

```
$ cd /tmp
$ curl -O https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
$ sha256sum Anaconda3-2019.03-Linux-x86_64.sh
$ bash Anaconda3-2019.03-Linux-x86_64.sh
```

remember to add conda bin to PATH and run
```$ source ~/.bashrc```

```
conda update conda
conda update anaconda
conda install python=3.6
conda install tensorflow
conda install keras
```