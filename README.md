== Ansible Grant/Revoke SSH Access ==
This is a Ansible Playbook to automate the process of granting / revoking SSH access to a group of servers instances to a new user

== Repository ==
* https://github.com/luigimolinaro/ssh-bastion-host

== Pre-requisites ==
* You must have Ansible 2.3 installed.
* Git PULL from here https://github.com/luigimolinaro/ssh-bastion-host

== Server & Installation Path ==
Ansible playbook is installed on Retelit Network under server retelit12.retelit.it (10.200.7.147) under /root/ssh-access-manager

== Inventory files ==
in the ansible playbook we definied all the HCS server interested on update : (inventory/hcs/hosts)
 [hcs_milano_physic]
 #Controller CASCADING
 10.30.164.94 	#02.05_U02_2288HV5_01_Controller_01
 10.30.164.27 	#01.04_U02_2288HV5_01_Controller_02
 10.30.164.33 	#02.04_U02_2288HV5_01_Controller_03
 #CloudService
 10.30.164.60	#02.05_U05_2288HV5_02_AdvancedMGMT_01 
 10.30.164.112	#01.04_U05_2288HV5_02_AdvancedMGMT_02
 10.30.164.38	#02.04_U05_2288HV5_02_AdvancedMGMT_03
 #Net-Service
 10.30.164.108	#02.05_U08_2288HV5_03_NetService_01
 10.30.164.61	#02.05_U08_2288HV5_03_NetService_02
 
 #Controller CASCADED
 10.30.169.132	#Cascaded_Opensatck01
 10.30.169.133   #Cascaded_Opensatck02
 10.30.169.69    #Cascaded_Opensatck03
 
 10.30.169.225	#KVM01
 10.30.169.161   #KVM02
 10.30.169.76    #KVM03
 10.30.169.170   #KVM04
 10.30.169.71    #KVM05
 10.30.169.59    #KVM06
 10.30.169.198   #KVM07
 10.30.169.108   #KVM08
 10.30.169.87    #KVM09
 10.30.169.98    #KVM10
 10.30.169.61    #KVM11
 
 [hcs_milano_physic:vars]
 ansible_python_interpreter=/usr/bin/python
 ansible_ssh_user=fsp
 ansible_ssh_pass=Cloud12#$
 ansible_become=True
 ansible_become_method=su
 ansible_become_user=root
 ansible_become_pass=Huawei@CLOUD8!
Please Note vars associated to "hcs_milano_physic" [hcs_milano_physic:vars] in order to manage new scenario's.

== Run Playbook ==
* Add hosts in <code>inventory/{hcs,retelit-server}/hosts</code> file. See the given hosts file to add hosts.
* Add public key in <code>roles/public-keys/</code>. e.g if we grant access for john doe user, copy and rename public key into that path like <code>roles/public-keys/luigi_molinaro/luigi_molinaro.id_rsa.pub</code>
* Run command <code>ansible-playbook -i inventory -e "access={grant|revoke} ssh_user={SSH_USER} user={USER}" --tag={del_user|add_user}</code>

== Example ==

=== Add user to HCS infastructure ===
 #GRANT
 ansible-playbook -i inventory -e "access=grant ssh_user=luigi_molinaro user=fsp" hcs.yml

 #REVOKE
 ansible-playbook -i inventory -e "access=revoke ssh_user=luigi_molinaro user=fsp" hcs.yml

=== Add user to HCS and add user to the Bastion Host ===
 #GRANT AND ADD USER
 ansible-playbook -i inventory -e "access=grant ssh_user=luigi_molinaro user=fsp hcs.yml" --tag=add_user

 #REVOKE AND DELETE USER
 ansible-playbook -i inventory -e "access=revoke ssh_user=luigi_molinaro user=fsp hcs.yml" --tag=del_user
 [[Categoria:MultiCloud]]
 [[Categoria:Procedure cloud]]
 [[Categoria:Procedure COS]]
 [[Categoria:Operation]]

=== User Bastion Host as Jump Server ===
 $ ssh -J [user]@retelit12.retelit.it [user]@[target host]
Example : 
 ssh -J l.molinaro@10.200.7.147 fsp@10.30.164.108
