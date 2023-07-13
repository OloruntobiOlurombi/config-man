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





