# Ansible Playbook to setup LDAP on Ubuntu


## Installation

* Clone the repository, replace <institution> with the actual name of your institution

        git clone  https://github.com/ubuntunet/unIDa_LDAP.git <institution>-ldap
        cd <institution>-ldap

### Inventory File

Create the inventory file for your institution, for more information: http://docs.ansible.com/ansible/intro_inventory.html

        cp inventories/template inventories/<institution>

Open the inventory file with your favorite editor and change the ansible_host and ansible_user to your server environment. Don't forget to again replace <institution>.

### Variables File

Create the variables file for your institution, more information: http://docs.ansible.com/ansible/playbooks_variables.html

        cp group_vars/template group_vars/<institution>

Open the variable files in your favorite editor and adapt the values to your setup.


### Secrets File

Some values - passwords, credentials - are sensitive and should never be submitted to the Github repository. They are therefore stored in a file called secrets.yml, which is being ignored by Github.

Create the secrets.yml file

        cp group_vars/secrets.yml.example group_vars/serets.yml

Open the secrets.yml file and add the sensitive values.

There are many ways to create random passwords/passphrases/salt, I prefer to use openssl for this task. You can replace 12 with a higher number for longer strings.

        openssl rand -base64 12


### Run the playbook

        ansible-playbook -i inventories/<institution> ldap.yml 

If you want to try it out locally, and you have Vagrant/Virtualbox installed, the following command will run the playbook using the development inventory/variabels.

        vagrant up --provision



----


## Activate LDAP Account Management (LAM)

LAM is a webfront end for LDAP with a rich feature set (https://www.ldap-account-manager.org/)

* Enable the 'lam' role by uncommenting it in ldap.yml
* (Re)Play the playbook
* Go to https://{{ fqdn}}



## Trouble Shooting

If you changed your rootpw, you need to remove the slapd service on the server manually for the new rootpw to be picked up. This has also helped me in other hopeless situations.
```
sudo aptitude purge slapd
```

