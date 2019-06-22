Role Name:
=========

This role can be used to easy install Tableau Server.

Requirements:
------------
This role need and will install " wget ".

# Create a password hash using the follow command:
openssl passwd -1 P@ssword2019

# Create a playbook and paste the bellow content:
----------------
Example Playbook

In this case default password for "admserv.tableau" is "P@sswprd2019"

----------------

---
- hosts:
   - tableau
  gather_facts: yes
  become: yes

  tasks:
    user:
      name: admserv.tableau
      shell: /bin/bash
      password: $1$xQKCTLyE$fwNBOWwmHj.OrNs0tGxhy0  # Paste result of your hash password
      comment: local account for tableau
      groups: users

  vars_files:
    - tableau_files/secret_tableau_license_file.yml # Only if you have an Tableau license
    - tableau_files/secret_pwd_admserv_tableau.yml   # Password vault for user " admserv.tableau "
    - tableau_files/secret_pwd_infraestructure.yml   # Password vault for user " infraestructure "

  roles:
    - {role: 'servslack_tableau-installation', tags: 'tableau_installation'}

----------------

# Select with feature you need activate:
================

- name: Activation_Fase_2
# Activate Trial
  command: /opt/tableau/tableau_server/packages/'{{ v20192 }}'/tsm licenses activate -t
# Activate Tableau License
# command: /opt/tableau/tableau_server/packages/'{{ 20191 }}'/tsm licenses activate --license-key '{{ license_file }}'

================

#
# Create a vault password for admserv.tableau
#
# Inside Ansible directory, create a folder asked " tableau_files "
# and run the follow the commands:
ansible-vault create tableau_files/secret_pwd_admserv_tableau.yml
# Content tableau_files/secret_pwd_admserv_tableau.yml:
---
user_password: P@ssword2019
#

ansible-vault create tableau_files/secret_pwd_infraestructure.yml
# Content tableau_files/secret_pwd_infraestructure.yml:
---
user_infrastructure_pwd: P@ssword2019

# Only if you have an Tableau License:
ansible-vault create tableau_files/secret_tableau_license_file.yml
# Content tableau_files/secret_tableau_license_file.yml:
---
license_file: P@ssword2019


================
License: "license (BSD, MIT)"


================
Author Information:
------------------
Name: Weliton Alves de Souza
Linkedin: https://www.linkedin.com/in/weliton-alves-de-souza-aa581220/
