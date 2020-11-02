# scripts
Linux Scripts and Ansible Scripts from my CyberClass
## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

![](https://github.com/buzzbirds/scripts/blob/main/diagrams/Project01.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

! ../scripts/ansible/playbook_files/install-elk.yml
! ../scripts/ansible/playbook_files/pentest-DVWA.yml
! ../scripts/ansible/playbook_files/filebeat-playbook.yml
* ../scripts/ansible/playbook_files/metricbeat-playbook.yml
* ../scripts/ansible/configuration_files/ansible.txt
* ../scripts/ansible/ configuration_files/hosts.txt
* ../scripts/ansible/ configuration_files/filebeat-config.yml
* ../scripts/ansible/ configuration_files/metricbeat-config.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

## Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- What aspect of security do load balancers protect?
The off-loading function of the load balancer defends against DDoS attacks.  

- What is the advantage of a jump box?
  A jump box provides administrators a secure original point to connect to other servers.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network and system metrics.
- What does Filebeat watch for?
Filebeat monitors log files or locations, collects log events, and forwards them to an indexing utility.

- What does Metricbeat record?
Metricbeat collects metrics from operating system and services running on servers and forward them to an indexing utility.

 
The configuration details of each machine may be found below.

| Name         |    Function       | IP Address    | Operating System        |
|--------------|-------------------|---------------|-------------------------|
| Jump Box     | Gateway           | 10.0.0.4      | Linux                   |
| Web1         | DVMA Server       | 10.0.0.5      | Linux                   |
| Web2         | DVMA Server       | 10.0.0.6      | Linux                   |
| Elk_VM       | Elk Server        | 10.1.0.4      |  Linux                  |

##Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
The Local Workstaions public IP address.
Elk-VM Kabania service can accept connections from the Internet through the Elk-VM-nsg (network security group).
Web1 and Web2 DVWA service can accept connection from the Internet through the Red-Team-LB limited by the RedTeam-SG (network security group).

Machines within the network can only be accessed by Ansible.
- Which machine did you allow to access your ELK VM? 
Ansible container is the only machine allowed to access Elk-VM from within the virtual network.
- What was its IP address?
Ansible container’s IP address is 172.17.0.1.  
A summary of the access policies in place can be found in the table below.

| Name           | Publicly Accessible      | Allowed IP Addresses                     |
|----------------|--------------------------|------------------------------------------|
| Jump Box       | Yes                      | Local Workstation public IP address      |
| Elk-VM         | No                       | 172.17.0.0/24                            |
| Elk-VM         | Yes                      | Local Workstation public IP address      |
| Web1           | No                       | 10.0.0.4                                 |
| Web2           | No                       | 10.0.0.4                                 |


# Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?
Ansible automation allows development and testing of tasks through simple code before releasing the update to production.  

 
The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Install docker.io
- Install python3-pip
- Install docker module
- Increase virtual memory
- Download and launch elk container with specified ports

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![] (https://github.com/buzzbirds/scripts/linux/docker_ps_output.png)

# Target Machines & Beats
- This ELK server is configured to monitor the following machines:
The Elk server is configured to monitor IP addresses, 10.0.0.5 and 10.0.0.6.

- We have installed the following Beats on these machines:
Filebeat adn metricbeat are installed on DVWA servers, Web1 and Web2.

- These Beats allow us to collect the following information from each machine:
Filebeat monitors log files or locations, collects log events, and forwards them to an indexing utility.
Metricbeat collects metrics from operating system and services running on servers and forward them to an indexing utility.

# Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install_elk.yml file to /etc/ansible.
- Update the install_elk.yml file to include the hosts, remote user, and published_ports.
- Run the playbook, and navigate to the elk server to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it? 
The playbook file is install_elk.yml.   The file is copied to /etc/ansible.

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
The hosts file should be updated to make Ansible run the playbook on a specific machine.  Update the playbooks’ hosts and remote_user entries, to specify which machine receives which services. 

- _Which URL do you navigate to in order to check that the ELK server is running?
http://<elk server public IP>:5601/app/kibana#/home

## As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

- ssh into the jump box form your local workstation
ssh RedAdmin@<jump_box_public_IP_address>

- List Docker containers to obtain the Ansible container name
sudo docker container list-a

- Starting the Ansible container 
sudo docker container start <Ansible container name>

- Access the Ansible container shell
sudo docker container attach <Ansible container name>

- Create the following files within the designated directories.  Copy and paste the contents of each file from the Github repo.  The repo file paths are listed above.
nano /etc/ansible/ansible.cfg
nano /etc/ansible/hosts  
nano /etc/ansible/files/filebeat-config.yml
nano /etc/ansible/files/metricbeat-config.yml

nano /etc/ansible/pentest-DVWA.yml
nano /etc/ansible/install_elk.yml
nano /etc/ansible/roles/filebeat-playbook.yml
nano /etc/ansible/roles/metricbeat-palybook.yml

- Modify configuration files in their appropriate directories
Optional Update ansible.cfg line 107 with the default remote_user 
 nano /etc/ansible/ansible.cfg

Optional Update hosts file line 18 - Ex 2: A collection of hosts section – to include [webservers] and [elkservers]
 nano /etc/ansible/hosts  

Update filebeat-confg.yml line 1805 with Elk server private IP address
 nano /etc/ansible/files/filebeat-config.yml

Update metric-config.yml line 62 with Elk servicer private IP address
 nano /etc/ansible/files/metricbeat-config.yml

Verify pentest-DVWA.yml, filebeat-playbook.yml, metricbeat-playbook.yml, and install_elk.yml hosts and remote_user entries are set correctly in accordance with the hosts file, ansible.cfg or virtual machine user name (if not the default remote_user).
 nano /etc/ansible/pentest-DVWA.yml
 nano /etc/ansible/install_elk.yml
 nano /etc/ansible/roles/filebeat-playbook.yml
 nano /etc/ansible/roles/metricbeat-palybook.yml

- Run the Ansible playbooks
 ansible-playbook /etc/ansible/pentest-DVWA.yml
 ansible-playbook /etc/ansible/install_elk.yml
 ansible-playbook /etc/ansible/roles/filebeat-playbook.yml
 ansible-playbook /etc/ansible/roles/metricbeat-palybook.yml

- Connect to webserver through a web browser
http://<Elk server Pulic IP address>/app/kibana#/home
http://<Load Balancer Public IP address>/setup.php
