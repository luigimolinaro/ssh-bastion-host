Ansible Grant/Revoke SSH Access
This is a Ansible Playbook to automate the process of granting / revoking SSH access to a group of servers instances to a new user

== Repository ==
* https://github.com/luigimolinaro/ssh-bastion-host

== Pre-requisites ==
* You must have Ansible 2.3 installed.
* Git PULL from here https://github.com/luigimolinaro/ssh-bastion-host

== Server & Installation Path ==
Ansible playbook is installed on Retelit Network under server retelit12.retelit.it (10.200.7.147) under /root/ssh-access-manager


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

