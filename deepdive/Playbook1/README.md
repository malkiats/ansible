# Copy / Install / User / Fileinline

All servers: 
Edit /etc/hosts to include the following entry: 
- ansible.xyzcorp.com 169.168.0.1 
- Install elinks 
- Create the user xyzcorp_audit 
- Copy the files /home/ansible/motd and /home/ansible/issue to /etc/ 

Network servers: 
- Install nmap-ncat 
- Create the user xyzcorp_network 

SysAdmin servers: 
- Copy /home/ansible/scripts.tgz from the control node to /mnt/storage
