# Ansible learning
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
 
 
 
 ## Ansible Tasks
 
 ### Ansible commands
 
 One can run Ansible commands in one of the 2 ways below
 
 1. Ad-hoc approach (Run Ansible commands in our terminal window)
 
     Ad-hoc commands are a good learning and experimentation as well and can used for things that one does not want to write 
     a playbook for.

     An ad-hoc command follows the following synthax: ```ansible <target> -m <module-name> -a <arguments> ```

     ```target:``` remote hosts 
     
     ```module:``` commands that can be executed on the remote host. to view all module run the following command ```$ ansible-doc -l ```
     

     ```
        $ nano /tmp/test1  then Add the following line "From source"
        Check that the file was created
        $ cd /tmp/
        $ ansible demo_hosts -m copy -a "src=/tmp/test1 dest=/tmp/test1"
     ```
 
 2. Playbook (A playbook is a set of sequential tasks)
 
 ### Ansible Facts 
 
 Facts are details or piece of information collected from remote hosts,  
 information derived from talking with remote systems. They can be 
 used for grouping nodes by the type of OS they are running or filtering 
 nodes based on the amount of RAM they have.
 
 Facts are: IP Adresses, RAM, OS ...
 
 Ansible gathers context before running tasks, this context is part of 
 the facts collected by Ansible.
 
 Ansible uses facts, which are mainly system environment information and 
 uses these facts to check the states and verify whether it needs 
 to change anything (on the remote hosts) in order to get a desired outcome.
 
 This makes it really safe to run Ansible playbook over and over again.
 
 Facts are collected using ```setup```
 
 ```$ ansible <HOST_NAME> -m setup``` gets details of all hosts
 ```$ ansible <HOST_NAME> -m setup -a "gather_subset=network, virtual" ```

 ### Ansible Variables 
 ### Ansible Sections
 
 
 
  

  
  
 ## References
 
 1. http://lifeonubuntu.com/ubuntu-missing-add-apt-repository-command/ (Environment)
 2. https://medium.com/@jezhalford/ansible-custom-facts-1e1d1bf65db8 (Ansible Facts)
 
 
 

