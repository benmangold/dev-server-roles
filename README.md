dev-server-role
=========

ephemeral ubuntu development environment configs

postgresql, docker, nodejs 12, oh-my-zsh

a role used by [benmangold/dev-server](https://github.com/benmangold/dev-server)

Makefile
--------

Export AWS Credentials before running `make` commands for local dev

Example command, exporting creds from LastPass notes `access-key-id` and `secret-access-key` with jq:

```bash
export AWS_ACCESS_KEY_ID=$(lpass show access-key-id --json | jq -r '.[0].note')
export AWS_SECRET_ACCESS_KEY=$(lpass show tf-secret-access-key --json | jq -r '.[0].note')

```

```bash
make init # initialize terraform

make apply # create ephemeral EC2 instance for role dev. 

# NOTE: wait 30s after apply before running ansible
make ansible # run ansible role tasks against EC2

make connect # connect to EC2 via ssh

make destroy # destroy EC2

```


Role Variables
--------------

```text
git_username: git-user # username for git config

git_email: email@email.com # email for git config

```

Example Playbook
----------------

```fs
/ansible
  playbook.yml
  requirements.yml
  variables.yml
 
```

playbook.yml
```ansible
---

- name: Provision Ubuntu
  hosts: all
  gather_facts: yes
  become: yes

  vars_files:
    - ./variables.yml

  tasks:
    - name: Dev Env Install
      import_role:
        name: dev-server-role

```

requirements.yml
```ansible
---

- src: https://github.com/benmangold/dev-server-role
  version: v0.0.1

```

variables.yml
```ansible
---

git_username: git-username
git_email: email@email.com

```