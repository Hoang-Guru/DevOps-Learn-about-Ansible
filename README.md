# 1. Tìm hiểu về Ansible
- [Link](https://viblo.asia/p/phan-1-tim-hieu-ve-ansible-4dbZNxv85YM)

# 2. Video 1 : Ansible Tutorial for DevOps Engineers in Hindi | Easy Setup // DevOps Bootcamp Day 7
- Launch EC2 : ansible_master
	+ ubuntu
	+ allow 22
- Launch 3 more like this
	+ ansible_server_1
	+ ansible_server_2
	+ ansible_server_3
- Connect ansible_master 
```
sudo apt update
sudo apt install ansible

```
- Create key and get in .ssh ( we will take key in key_pair to copy on it )
```
vim ansible_key
```
- Then 
```
sudo ssh -i -/.ssh/ansible_key ubuntu@ip_server1
```
- Then check
```
cat /etc/ansible/hosts
mkdir ansible
cd ansible/
vim hosts
```
```
# in hosts
[servers]
server1 ansible_host=ip_server1
server2 ansible_host=ip_server2
server3 ansible_host=ip_server3

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```
- Then
```
ansible-inventory --list -y -i /home/ubuntu/ansible/hosts
ansible all -m ping -i /home/ubuntu/ansible/hosts
# check again
ansible all -m ping -i /home/ubuntu/ansible --private-key=-/.ssh/ansible_key
# check again
cd
chmod 700 ~/.ssh
chmod 600 ~/ssh/ansible_key
ansible all -m ping -i /home/ubuntu/ansible --private-key=-/.ssh/ansible_key

```
- Then next - check other module
```
ansible all -a "free -h" -i /home/ubuntu/ansible/hosts --private-key=-/.ssh/ansible_key
ansible servers -a "uptime" -i /home/ubuntu/ansible/hosts --private-key=-/.ssh/ansible_key

```

# 3. Video 2 : Deploying Ansible Playbooks as a DevOps Engineer // DevOps Bootcamp Day -8 (Hindi)
- First , Check
```
ansible-inventory --list -y -i /home/ubuntu/ansible/hosts

ansible all -m ping -i /home/ubuntu/ansible --private-key=-/.ssh/ansible_key

```
- Then start with playbook
```
mkdir playbook
cd playbook
vim create_file.yml
```
	+ Content :
```
---
-   name: This playbook will create a file 
    hosts: all
	become: true
	tasks:
	- name: Create a file
	  file:
		path: /home/ubuntu/subscribe.txt
		state: touch
```
- Then
```
ansible-playbook create_file.yml -i /home/ubuntu/ansible/hosts --private-key=-/.ssh/ansible_key
```
- Check file in all ansible_server_...
- Then create new user 
```
cd /ansible/playbook
vim create_user.yml
```
- Content in create_user.yml
```
---
- name: this plaubook will create a user
  hosts: all
  become:true
  tasks:
  - name: to create user name traiwithshubham
    user: name=tainwithshubhum
```
- Then
```
ansible-playbook create_user.yml -i /home/ubuntu/ansible/hosts --private-key=-/.ssh/ansible_key

```
- Then check in all ansible_server
```
cat /etc/passwd
```
- Check in google : playbook ansible -> to design on demand
- Similar with Install Docker : check in ansible playbook or ...
- Create a install_docker.yml
```
---
- name: This playbook will install Docker
  hosts: all
  become:true
  tasks:
  - name: Add Docker GPG apt key
    apt_key:
	  url: https://download.docker.com/linux/ubuntu/gpg
	  state: present
  - name: Add Docker
    apt_repository:
      repo: deb https://download.docker.com/linux/ubuntu focal stable
      state: present
  - name: Update apt and install docker-ce
    apt:
      name: docker-ce
      state: latest
      update_cache: true

  - name: Install Docker Module for Python
    pip:
      name: docker
```

	+ Link to tutorial - [Collection Index](https://docs.ansible.com/ansible/latest/collections/index.html)
	+ Link 2 , [tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-ansible-to-install-and-set-up-docker-on-ubuntu-20-04)
	+ Link [trainwithshubham](https://www.trainwithshubham.com/blog/ansible-playbooks)
- Then 
```
ansible-playbook install_docker.yml -i /home/ubuntu/ansible/hosts --private-key=-/.ssh/ansible_key

# Update if neccessary
ansible all -m apt -a "upgrade=yes update_cache=yes cache_valid_time=86400" --become -i /home/ubuntu/ansible/hosts --private-key=-/.ssh/ansible_key
```
