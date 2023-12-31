# Configuration Management
### Task: Configure a Linux machine with Ansible

- To completed this task I worked with AWS EC2 instance which was provision via the AWS console. Below are the step on how I launched the machine (EC2 instance):
  1. Went to the AWS Management Console.
  2. Navigate to the EC2 service.
  3. Click on "Launch Instances" to launch a new EC2 instance.
  4. Selected my desired Linux distribution as ***`Ubuntu`*** and instance type as ***`t2.micro`***
  5. Configured security groups, and review settings.
  6. Created a key pair to access it securely.
  7. Launch the instance.
 
<img width="1407" alt="Screenshot 2023-07-13 at 3 38 08 AM" src="https://github.com/OloruntobiOlurombi/config-man/assets/40290711/1bf7461d-edbd-4d3c-917e-50459f76ba55">
   
<img width="1402" alt="Screenshot 2023-07-13 at 3 38 44 AM" src="https://github.com/OloruntobiOlurombi/config-man/assets/40290711/3a3d7343-6011-4b09-8323-0daf270a3b9a">

 8. Connect to the EC2 instance.
 9. Update the system packages using the appropriate package manager: 
     ```
     sudo apt update && sudo apt upgrade -y
     ```
 10. Install Ansible on both the EC2 and local host (if not already installed).
     ```
     sudo apt install ansible -y
     ```
- Now that I am done with my configuration it is time to create a playbook wwhich do 
the following:

1. Each new user should have a script called nice-script.sh in their home directory (hint: use "skeleton directory"). The script should list all mounted filesystems.
2. Create the script with the correct command (locally)
3. Copy the script to the remote machine into the correct directory Creates a user with:
 * Username: john
Home: /better-place/john (create /better-place before if needed)
* User ID: 1234
4.User john should be able to run the following command with sudo without a need to provide his password: whoami
5. Install packages:
* Tmux
* vim
6. Install Terraform CLI


#### Let's get started

- Create a playbook file (setup_machine.yml) and edit with the following content:
```
touch setup_machine.yml
```

```
---
- name: Set up new machine
  hosts: all
  become: true
  tasks:
    - name: Create directory for John
      file:
        path: /better-place/john
        state: directory

    - name: Copy nice-script.sh to skeleton directory
      copy:
        src: nice_script.sh
        dest: /etc/skel/nice-script.sh
        mode: 0755

    - name: Create user John
      user:
        name: john
        home: /better-place/john
        uid: 1234

    - name: Allow John to run 'whoami' with sudo without password
      lineinfile:
        dest: /etc/sudoers
        state: present
        regexp: '^john'
        line: 'john ALL=(ALL) NOPASSWD: /usr/bin/whoami'

    - name: Install Tmux and Vim
      apt:
        name: "{{ item }}"
        state: present
      with_items:
        - tmux
        - vim

    - name: Install unzip package
      apt:
        name: unzip
        state: present

    - name: Download Terraform CLI
      get_url:
        url: "https://releases.hashicorp.com/terraform/0.15.4/terraform_0.15.4_linux_amd64.zip"
        dest: "/tmp/terraform.zip"
        mode: 0644

    - name: Extract Terraform CLI
      command: unzip -o /tmp/terraform.zip -d /usr/local/bin

    - name: Set ownership and permissions for Terraform CLI
      file:
        path: "/usr/local/bin/terraform"
        owner: root
        group: root
        mode: 0755
```

- Let's create another file called `nice-script.sh`. The aim of this script is to lists all mounted filesystems. I used the `df` command with the `-h` option to display the mounted filesystems in a human-readable format.

```
touch nice-script.sh
```
- Edit and save the below content to the file called `nice-script.sh` on the local machine. This file will be copied to the remote machine as part of the playbook execution and will be available in each new user's home directory.

 ```
#!/bin/bash

df -h
```

- Next I had to update the permissions of the private key file on my local machine by running the following command:
  
```
chmod 400 /Users/mac/Downloads/Appsilon-task.pem
```
This coomand restricts the permissions of the key file to be readable only the owner.

- Save the playbook file.
- Run the playbook using the following command:
  
```
ansible-playbook -i <EC2-INSTANCE-IP>, -u <SSH-USERNAME> --private-key <PATH-TO-PRIVATE-KEY> setup_machine.yml
```
Please note: Replace <EC2-INSTANCE-IP> with the public IP address of your EC2 instance, <SSH-USERNAME> with the SSH username for your Linux distribution (e.g., "ubuntu" for Ubuntu), and <PATH-TO-PRIVATE-KEY> with the path to the private key associated with your EC2 instance.

<img width="528" alt="Screenshot 2023-07-13 at 12 44 20 AM" src="https://github.com/OloruntobiOlurombi/config-man/assets/40290711/03377fe9-bf83-40a6-a1f9-fc048b0b5e3f">


