## Setup FusionDirectory

Go to https://{{ fqdn}}/fusiondirectory and follow the instructions



Create a random vault password
```
openssl rand -base64 32 > ~/.ansible_vault_pass
```

Copy, populate and encrypt secrets.yml
```
cp secrets.yml.example secrets.yml
vi secrets.yml
ansible-vault encrypt secrets.yml --vault-password-file ~/.ansible_vault_pass
```

Run the Playbook
```
ansible-playbook -i [development|staging] ldap.yml --vault-password-file ~/.ansible_vault_pass
```

Show the secret vars
```
ansible-vault view secrets.yml --vault-password-file ~/.ansible_vault_pass
``

If you changed your rootpw, you need to remove the slapd service on the server manually for the new rootpw to be picked up. This has also helped me in other hopeless situations.
```
sudo aptitude purge slapd
```

