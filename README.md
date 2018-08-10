# Ansible_learning
step by step Ansible with MS Azure 

## Getting started 

### What is Ansible?

Ansible is an automation configuration management and provisioning tool, which can be used to deploy applications, manage systems
and write ```infrastructure as code (Is the term used to manage infrastructure using code base.)```. 

### Install Ansible on Ubuntu VM 

Commands: 
  ```
  $ ssh <username>@<vm public ip>
  $ sudo apt-get install software-properties-common
  $ sudo apt-add-repository ppa:ansible/ansible
  $ sudo apt-get update
  $ sudo apt-get install ansible
  ```
  
  
 ### Host configuration
 
 ```
  $ nano /etc/hosts
  
  Add the following 
  
  <host_private_ip> <host_name> e.g. 10.X.X.X anyName
  
 ```
 
 Then specify ansible hosts
 
 ```
 $ nano /etc/ansible/hosts
  
  Add the following:
  
  [demo_hosts]
  
  node1 ansible_user="<username>" e.g. node1 ansible_user=djobukata
 ```
 
 ### Test Ansible to all Hosts
 
 ```
 $ ansible -m ping all
 ```
 Expected:
 
 ```
 node1 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
 ```
  
 ### Test if you can ssh into node1 vm 
 
 ```$ ssh <remote>@<ip>```
 
 You might get the below error
  
 ```
  he authenticity of host 'xxx.xxx.xxx.xx2 (xxx.xxx.xxx.xx2)' can't be established.
  ECDSA key fingerprint is SHA256:IGCMk6GAHSY1kFsKVejOSWT33KZVZ8bVY3RUZXTZ.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added 'xxx.xxx.xxx.xx2' (ECDSA) to the list of known hosts.
  Permission denied (publickey).
  
  Note: This error, just tells you that you don\'t have access to ssh from host to vm
 ```
 
 How to fix the above error
 
 1. Generate a new public/private rsa by running
  ```$ ssh-keygen -t rsa ```
  
  Output: 
  
  ```
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/demo/.ssh/id_rsa.
    Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:7K+IGCMk6GAHSY1kFsKVejOSWT33KZVZ8bVY3RUZXTZ demo@ansible
    The key's randomart image is:
    +---[RSA 2048]----+
    |.=o     . .... EX|
    |Bo o     o +...+.|
    |B.. .   . o .    |
    |*+ .   + .       |
    |B = . + S .      |
    | + = o . .       |
    |. . o *  ...     |
    | o   + +. ...    |
    |  .   o.    ..   |
    +----[SHA256]-----+
  ```
  
 2. Copy the public to remote vm 
 
 ```
  $ ssh-copy-id <remote>@<ip>
  Note: This command will copy the newly generated public key to your remote server(vm)
  
 ```
 The above command might result in an error and this is how one would go about fixing it:
 
 Error:
 
 ```
  /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/demo/.ssh/id_rsa.pub"
  /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
  /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
  Permission denied (publickey).
  
 ```
 
 Fix: 
 
 
 
  

  
  
 ## References
 
 1. http://lifeonubuntu.com/ubuntu-missing-add-apt-repository-command/
 
 
 

