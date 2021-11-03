# PROJECT WITH EMBEDDED ROLES

site.yml
roles/
  common/
    files/
    templates/
    tasks/
    handelers/
    vars/
    defaults/
    meta/
   webservers/
    files/
    templates/
    tasks/
    handlers/
    vars/
    defaults/
    meta/
    
# site.yml
---
- hosts: web
  roles:
     - common
     - webservers

http://galaxy.ansible.com
