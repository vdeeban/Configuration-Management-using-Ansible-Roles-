# Configuration-Management-using-Ansible-Roles-
Configure Ansible Roles
For the demo, I will be using a sample playbook divided into different roles: install, configure, and service YAML files. But you can use your own Ansible playbook too.

Step 1) Visit the tasks folder under your newly created roles by using the cd command as shown below. You can also list the files present here in this directory by running the ll command.

$ cd etc/ansible/roles/roledemo/tasks/
Step 2) After visiting the tasks folder of your ansible role, you need to edit the main.yml file. Run the vi or vim command to create or edit this file, as shown in the image below.

$ vim main.yml
Ansible Roles tasks
After running the above command, an editor will be shown in front of your screen with some space to fill your content. To fill content in this file, press key A and use arrow buttons to take your cursor wherever you want. After you are done with the update, press the Esc key, then type :wq and finally press Enter to save your file. I have imported three tasks in the main.yml file that I will create in the next step.

Step 3) Now, I will create some other YAML files inside the same tasks folder. To create new YAML files, run the below vi commands one by one and fill the tasks in each file.

$ vi install.yml
$ vi cofigure.yml
$ vi service.yml
--- 
#tasks for install.yml file 
- name: Install httpd Package 
  yum: name=httpd state=latest
--- 
#tasks for configure.yml file 
- name: Copy httpd configuration file
copy: src=files/httpd.original dest=/etc/httpd/conf/httpd.conf
- name: Copy index.html file
copy: src=files/index.html dest==/var/www/html
notify:
- restart roledemo
--- 
#tasks for service.yml file 
- name: Start and Enable httpd service
service: name=httpd state=restarted enabled=yes
I have filled some demo tasks in the above 3 YAML task files. After filling the content in each YAML file, save it using the same steps mentioned in the previous step.

Step 4) As mentioned in the notifying section of the configure.yml file, we also need to update the main.yml file of handlers to restart the service when there is a change.

$ cd..
$ vi handlers/main.yml
---
# handlers file for etc/ansible/roles/roledemo
- name: restart roledemo
  service: name=httpd state=restarted
Step 5) The above-created role’s task is to copy some files from the sources to the destination mentioned in the configure.yml file. Visit the directory with name files using the command given below.

$ cd 
$ cd etc/ansible/roles/roledemo/files/
$ vi httpd.original
$ vi index.html
Fill content in the above files and save it. Inside the index.html file, you can fill in any message or layout.

Step 6) Now, we have successfully created a simple Ansible role. To check all the files and directories, we have created till now. Rerun the tree command.

$ cd
$ tree etc/ansible/roles/roledemo
Ansible Roles
This is the final tree of the directories and files in our Ansible role.

Execute Ansible Roles
After creating the role, we need to create a playbook YAML file to run. Follow the steps below for testing our Ansible role.

Step 1) Visit the ansible directory and run the below commands to create a playbook YAML file with any name.

$ cd
$ cd etc/ansible/
$ vi setup.yml
---
# setup.yml as a ansible playbook
- hosts: localhost
  roles:
 - roledemo
Enter your hostname here and mention the role name to execute in the setup.yml file.

Step 2) To check if there is an error in your playbook file, you can run the below command. If there are no errors in your playbook and ansible roles, you can run your playbook.

$ ansible-playbook setup.yml --syntax-check
$ ansible-playbook setup.yml
After using the above commands, your ansible tasks are executed one by one, and all the execution updates are shown on your screen.

Step 3) To check the result of the index.html file, you can run the below command.

$ elinks http://localhost
Here, localhost is my default hostname. After completing all the steps, Ansible Role with some simple execution tasks is created successfully.
