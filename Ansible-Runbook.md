<div align="center">
<img src=https://static.wixstatic.com/media/1c706c_a5df0ad56f894928bf858a74ba744b32~mv2.png/v1/fit/w_2500,h_1330,al_c/1c706c_a5df0ad56f894928bf858a74ba744b32~mv2.png width="400" height="200">
 </div>

# <div align="center"> ANSIBLE RUNBOOK </p>

# <div align="center"> DevOps Workshop Training </div>

# <div align="right"> $`\textcolor{brown}{\text{Contact us: }}`$  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; </div>

<div align="right"> T O A C C E L E R A T E Y O U R C A R E E R G R O W T H </div>

### <div align="right"> For questions and more details: </div>

<div align="right"> <img src=https://w7.pngwing.com/pngs/759/922/png-transparent-telephone-logo-iphone-telephone-call-smartphone-phone-electronics-text-trademark-thumbnail.png width="20" height="20"> +91 98712 72900 </div>

<div align="right"> <img src=https://pbs.twimg.com/profile_images/1450734615946219520/jmBHQRRa_400x400.jpg width="20" height="20"> https://www.thecloudtrain.com </div>

<div align="right"> <img src=https://icons.iconarchive.com/icons/martz90/circle/512/email-icon.png width="20" height="20"> support@thecloudtrain.com </div>

<div align="right"> <img src=https://png.pngtree.com/png-vector/20221018/ourmid/pngtree-whatsapp-icon-png-image_6315990.png width="20" height="20"> +91 98712 72900 </div>

#
</br>

## $`\textcolor{red}{\text{NOTE: USE UBUNTU 22.04 VIRTUAL MACHINES FOR ALL THE LABS}}`$

## _Find solutions to your assingment below:_

## Exercise 1: Accomplish below task to complete this exercise:

a) Create two Ubuntu 22.04 compute instances, one for Ansible master and other for slave

### _Solution:_

Same as previous.

b) Install ansible on master Ubuntu GCP instance

### _Solution:_

Refer [**Ansible Installation officical document**](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu) for more options.

i. Switch to ubuntu user. You can use any account that have sudo access also:

`sudo su – ubuntu`

ii. Run below commands one by one on Ansible master server to install ansible

```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

iii. Once installed check the ansible version using below command

`ansible --version`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/07f324b5-d5bd-4537-a1c3-68213d7b9e7e)

c) Establish ssh connectivity between master and slave for **ubuntu** user by copying ssh public key of master to slave authorized\_keys

### _Solution:_

i. Generate ssh keys on ansible master using below command. Use default values for all input:

`ssh-keygen`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/ddc10ade-274f-4556-ae5d-3d8b15a31c49)

ii. Extract the public key and copy it to clipboard:

`cat .ssh/id_rsa.pub`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/64642e87-0386-4049-b3b7-d1e6da1ceada)

iii. Login to slave server with ubuntu user and paste the copied public key content to the **/home/ubuntu/.ssh/authorized\_keys**:

```
sudo su - ubuntu
cd .ssh
vim authoried_keys
```

Cat the file to check if key added properly.

`cat authorized_keys`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/a95c7f31-43fd-42a2-9918-b8efef5c8b0b)

**Note:** _Make sure from the above that the content of master server's file **/home/ubuntu/.ssh/id\_rsa.pub** key content and slave server's file **/home/ubuntu/.ssh/authorized\_keys** content is exactly same, else you will face issues while connecting slave from master_

1. Add the slave node to ansible host file.

### _Solution:_

d) Get the hostname of slave node using below command:

`hostname`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/fc4f8f7f-9404-46d8-a98d-d38c88c20927)

Add this hostname to ansible host file located at /etc/ansible/hosts on ansible master node. Switch to master node and run below commands:

Use vim editor to edit the file and add the slave node hostname to the end of the file

`vim /etc/ansible/hosts`

Cat the file to see if host correctly added. Here grep -v '#' is used to filter commented lines from host file.

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/98e6c77a-007c-4bb7-b633-3fdef4e97bad)

e) Run below ansible commands on slave node:
- i. Ansible ping module to test connectivity of slave from master

### _Solution:_

`ansible -m ping slave`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/138caece-42b0-486c-a2c4-c12212c2b96a)

- ii. Ansible setup module to gather facts

### _Solution:_

Ansible setup module gives you each and every detail of slave node configuration, that's why you are a big json output here:

`ansible -m setup slave`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/915b7596-0bdc-4936-9c9d-c0c66c297dc4)

- iii. Ansible command module to run any linux command on target machine

### _Solution:_

Below example uses hostname command in ansible command module. You can use any command in -a option:

`ansible -m command -a "hostname" slave`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/3faab064-593a-4e69-8954-153784e889d2)

## Exercise 2: Accomplish below task to complete this exercise:

a) Write Ansible playbook with below tasks that will run on slave node:

- i. Check date on the slave node

- ii. Install nginx on the slave node

### _Solution:_

i. Create a yaml file named **first\_playbook.yaml** using vim editor as below:

`vim first_playbook.yaml`

ii. Add the below content

```
---
- hosts: slave
  become: yes
  name: Play 1
  tasks:
    - name: Execute command ‘Date’
      command: date
    - name: Install nginx
	  apt: name=nginx state=latest
```

Cat the file to check the content added correctly: 

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/6cade67d-2143-4823-91ed-539e3e708ae1)

iii. Run playbook using below command and see the things happening on slave node from master server itself. Make sure you have Zero failed task in the Play Recap:

`ansible-playbook first_playbook.yaml`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/d9743205-617e-4312-9dde-21a4bd2a7eb1)

iv. Verify on the slave node if nginx installed successfully:

`dpkg -l |grep nginx`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/e617e39b-01c0-4a63-ba2a-aa8a51272059)

b) Create an Ansible role to perform above set of tasks using roles on the slave node

### _Solution:_

i. Create ansible role template using ansible-galaxy:

`ansible-galaxy init first_role`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/8af96182-cd9a-4e99-bd90-76eb0088a08a)

ii. Install tree binary to check role structure. This is just an optional step to visualize role directory in tree format.

`sudo apt install tree`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/67884568-96b5-41a6-81fc-831f00196f4c)

iii. Use tree to see roles directory structure:

`tree first_role`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/319c3262-4908-426c-93d2-12104a2797bc)

iv. Now edit tasks main.yml file to add tasks or files that you want to execute on slave node as part of this role:

`vim first_role/tasks/main.yml`

Add the below tasks in this file:

```
---

# tasks file for first_role

- include: install.yml
```

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/ec0a2969-8f43-492a-b387-f27384e13df6)

v. Create a task file named install.yml under directory **first\_role/tasks/** that will be called in **first\_role/tasks/main.yml** as defined in previous step:

`vim first_role/tasks/install.yml`

Add the below content:

```
---
- name: Execute command ‘Date’
  command: date
  register: date_output

- name: Print date
  debug: 
    msg: "{{date_output}}"

- name: Install nginx
  apt: name=nginx state=latest
```

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/e35aa2c0-8caf-4f09-a23c-17069964e184)

Here you can see one more task named **Print date**. This is used to print the date that we saved in the variable named **date\_output** in first task **Execute command 'Date'**. This is how we register and print variables in ansible.

vi. Now create a master playbook that will call this role and execute the defined tasks:

`vim roleplay.yaml`

Add below content:

```
---
  - hosts: slave
    become: yes
    roles:
    - first_role
```
![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/5fcf25c1-84e9-4ea1-930a-3089c892fc61)

vii. Executing the master playbook roleplay.yaml will execute the tasks in role first\_role:

`ansible-playbook roleplay.yaml`

![image](https://github.com/vistasunil/CT_DevOps_WS_Module5/assets/37858762/57cb89f8-f4af-4062-bf34-257952c956f1)
