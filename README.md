# NINO 

This Ansible role NINO (Narrow Input, Narrow Output) is an ETL dedicated to data masking and anonymisation using LINO, PIMO, SIGO, and MIMO 

# Example playbook petstore

```yml 
  roles:
    - name: OB-Live.nino 
      vars:
        entities:
          - name: "owners"
            id_json: "owners.jsonl"
          - name: "pets"
            id_json: "pets.jsonl"
            id_mask: "pets-masking.jsonl"
            yaml_mask: "target/pets-masking.yml" 
```

# Documentation 

This role install the following software

- https://github.com/CGI-FR/LINO
- https://github.com/CGI-FR/PIMO
- https://github.com/CGI-FR/SIGO
- https://github.com/CGI-FR/MIMO
- https://github.com/CGI-FR/pimo-play

Try out the masking options [pimo-play](https://cgi-fr.github.io/pimo-play)

Find out more on the component's [Documentation](https://cgi-fr.github.io/lino-doc/)

# Install 
### ansible on Debian
```bash
sudo apt update -y
sudo apt install git epel-release -y
sudo apt install ansible -y
```

### ansible on RedHat
```bash
sudo dnf update -y
sudo dnf install git epel-release -y
sudo dnf install ansible -y
```

### NINO and its dependencies 
```bash
# ansible-galaxy role remove OB-Live.nino
ansible-galaxy role install OB-Live.nino
ansible-galaxy collection install community.docker
cd ~/.ansible/roles/OB-Live.nino/
ansible-playbook installation.yml -K
```

### Run test example - petstore
```sh 
ansible-playbook -i tests/inventory tests/petstore-test.yml --connection=local -c ansible.cfg
```

# Role Variables 

```yml
# vars file for nino
lino_version: "3.6.1"
pimo_version: "1.31.3"
sigo_version: "0.4.0"
mimo_version: "0.8.0"

# install variables 
workspace: "business"
install_dir: "/usr/lino"
fifos_dir: "/tmp/lino"
tables: "{{ workspace }}/tables.yml"
relations: "{{ workspace }}/relations.yml"

```

# Dependencies 

```yaml
- name: gantsign.golang 
- name: arillso.ca_certificates 
- name: geerlingguy.docker 
```


# License 

GNU GPLv3

# Author Information 

[https://github.com/OB-Live](https://github.com/OB-Live)


