# NINO 

NINO stands for Narrow Input, Narrow Output.  It is an Ansible role for LINO, PIMO, SIGO and is a ETL dedicated to data masking and anonymisation 


# Documentation 

- Find out more on the component's [Documentation]https://cgi-fr.github.io/lino-doc/
- Try out the masking options [pimo-play](https://cgi-fr.github.io/pimo-play)

## Install
```sh 
ansible-galaxy role remove OB-Live.nino
ansible-galaxy role install OB-Live.nino
```

## Run example - petstore
```sh 
ansible-playbook ~/.ansible/roles/OB-Live.nino/petstore.yml -K
```

# Role Variables 

```yml
# vars file for nino
lino_version: "3.6.1"
pimo_version: "1.31.3"
sigo_version: "0.4.0"

# install variables 
workspace: "business"
install_dir: "/usr/lino"
tables: "{{ workspace }}/tables.yml"
relations: "{{ workspace }}/relations.yml"

```

# Dependencies 

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

# Example Playbook : petstore

```yml
---
- name: LINO Petstore Example
  hosts: localhost
  connection: local
  gather_facts: false
  vars:
    #  Business variables
    workspace: "example/petstore"
    source_db: "postgresql://localhost:5432/jhpetclinic?sslmode=disable -P ADMIN -u jhpetclinic"
    target_db: "postgresql://localhost:5433/jhpetclinic?sslmode=disable -P ADMIN -u jhpetclinic"

    ### fifo files
    fifo_pets: "/tmp/pets.jsonl"
    fifo_owners: "/tmp/owners.jsonl"
    fifo_pets_masking: "/tmp/pets-masking.jsonl"

  tasks:
    - include_role:
        name: OB-Live.nino
        tasks_from: install

    - include_role:
        name: OB-Live.nino
        tasks_from: connect

    - include_role:
        name: OB-Live.nino
        tasks_from: fifos
      loop:
        - "{{ fifo_pets_masking }}"
        - "{{ fifo_pets }}"
        - "{{ fifo_owners }}"

    # Owners
    - include_role:
        name: OB-Live.nino
        tasks_from: push
      vars:
        idName: "owners"
        pipeIn: "{{ fifo_owners }}"

    - include_role:
        name: OB-Live.nino
        tasks_from: pull
      vars:
        idName: "owners"
        pipeOut: "{{ fifo_owners }}"

    # Pets
    - include_role:
        name: OB-Live.nino
        tasks_from: push
      vars:
        idName: "pets"
        pipeIn: "{{ fifo_pets }}"

    - include_role:
        name: OB-Live.nino
        tasks_from: pull
      vars:
        idName: "pets"
        pipeOut: "{{ fifo_pets }}"
```

# License 

BSD

# Author Information 

Please check the underlying software

- https://github.com/CGI-FR/LINO
- https://github.com/CGI-FR/PIMO
- https://github.com/CGI-FR/SIGO

