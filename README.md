# Project_12_Ansible_config_mgt_2

>>### Project 12 Ansible Refactoring and Static Assignments (Imports and Roles)

># Project Description
- This project was a continuation of project 11, while making some improvements to the code (Refactoring) using imports function of ansible and roles.

>## Project Breakdown

 1. Installed the copy artifact plugin on jenkins console and created directory on server in home directory 
    - sudo mkdir /home/ubuntu/ansible-config-artifact
![](/Screenshots/1.%20Creating%20ansible-config-artifact%20directory.png)

![](/Screenshots/2.%20Installing%20copy%20artifact%20plugin.png)

2. Creating a new freestyle project 
![](/Screenshots/3.%20Creating%20new%20freestyle%20project.png)

3. Configuring Jenkins to save artifacts to created directory 
![](/Screenshots/4.%20Configuring%20builds%20to%20keep%20.png)

![](/Screenshots/5.%20configuring%20build%20triggers%20to%20ansible%20project.png)

4. Refactoring Ansible code 
    - created site.yml file and updated it with the import command 
    ---
    - hosts: all
     import_playbook: ../static-assignments/common.yml
    - configured a task to remove wireshark from target servers if already installed by creating static-assignments directory and creating common-del.yml file and updating it with the code :
![](/Screenshots/12.%20using%20import_playbook%20command.png)

    - ---
  - name: update web, nfs and db servers
     -   hosts: webservers, nfs, db
     -   remote_user: ec2-user
     -   become: yes
     -   become_user: root
     -   tasks:
  - name: delete wireshark
    yum:
     -   name: wireshark
     -   state: removed

- name: update LB server
     -   hosts: lb
     -   remote_user: ubuntu
     -   become: yes
     -   become_user: root
  tasks:
  - name: delete wireshark
    apt:
     -  name: wireshark-qt
     -  state: absent
     -  autoremove: yes
     -  purge: yes
     -  autoclean: yes
5. Running the plyabook using :
    - ansible-playbook -i inventory/dev.yml playbooks/site.yaml
![](/Screenshots/11.%20Wireshark%20not%20present%20on%20server.png)

6. Launched two servers and created roles 
![](/Screenshots/13.%20creating%20roles%20directory.png)

7. Updating uat.yml file with hosts information 
![](/Screenshots/13.%20Updating%20inventory%20file%20with%20webserver%20hosts%20.png)

8. uncommneting roles path in **/etc/ansible/ansible.cfg** and pointing it to created directory **/home/ubuntu/ansible-config-mgt/roles**
![](/Screenshots/14.%20Uncommenting%20roles_path%20in%20ansible%20config%20file(1).png)

9. configured the tasks to run on the webserver in the main.yml file and pointed to the GitHub application 
![](/Screenshots/15.%20updating%20main.yml%20file%20to%20perform%20tasks%20.png)

10. referenced the role created by creating an assignment for the webservers :

        - ---
        - hosts: uat-webservers
          roles:
            - webserver
11. saved configurations and ran the command :

    - sudo ansible-playbook -i /home/ubuntu/ansible-config-mgt/inventory/uat.yml /home/ubuntu/ansible-config-mgt/playbooks/site.yaml

![](/Screenshots/16.%20site%20running%20on%20webserver.png)

