# ELK-PROJECT
ELK PROJECT (README, DIAGRAM, ANSIBLE)
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/96204048/146290798-82e20ad6-100a-4866-aa32-b7b49b76dc53.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible file may be used to install only certain pieces of it, such as Filebeat.

 filebeat-playbook.yml
---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:
  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.6.1-amd64.deb
  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml
  - name: enable and configure system module
    command: filebeat modules enable system
  - name: setup filebeat
    command: filebeat setup
  - name: start filebeat service
    systemd:
      name: filebeat
      state: started
      enabled: yes

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


 Metricbeat-playbook.yml
---
- name: Install metric beat
  hosts: webservers
  become: true
  tasks:
  
  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb
    
  - name: install metricbeat
    command: dpkg -i metricbeat-7.6.1-amd64.deb
    
  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml
    
  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker
   
  - name: setup metric beat
    command: metricbeat setup
    
  - name: start metric beat
    systemd:
      name: metricbeat
      state: started
      enabled: yes


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting overlaod to the network.
What aspect of security do load balancers protect? What is the advantage of a jump box? 
(Loadbalancing ensures that the application will be highly responsive, in addition to restricting overload to the network. It also protects againsts DoS and DDoS attacks. The advantage of a jumpe box ensures that all machines in the network wont be exposed to the public).

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
What does Filebeat watch for? 
(Filebeat watches for log files,collects them, and forwards them to elasticsearch or logstash
What does Metricbeat record? 
(Metricbeat records metric data from your system and services like CPU usage,memory,etc).

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| WEB-1    |Web-server| 10.0.0.5   | Linux            |
| WEB-2    |Web-server| 10.0.0.6   | Linux            |
| WEB-3    |Web-server| 10.0.0.7   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Add whitelisted IP addresses 
(107.223.89.29)

Machines within the network can only be accessed by ssh.
Which machine did you allow to access your ELK VM? What was its IP address? 
(JumpBox Provisonar 20.124.241.19)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible? 
(It is quick and easy to set up and use. Tasks are easily done by writing a playbook and running it. You dont need to install any extra software or have any kind of firewall).

The playbook implements the following tasks:
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
-Install docker.io
-Install pip3
-Install Docker python module
-Increase virtual memory
-Download and launch a docker


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://user-images.githubusercontent.com/96204048/146293586-b7f3ca44-4afd-4207-aee2-be6e3d6297af.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
List the IP addresses of the machines you are monitoring 
(web-1 10.0.0.5, web-2 10.0.0.6)

We have installed the following Beats on these machines:
Specify which Beats you successfully installed 
(Filebeats and Metricbeats) 

These Beats allow us to collect the following information from each machine:
In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc. 
(For Filebeats it collects data from the system, being able to track system events. For Metricbeats it collects system/service metric data, such as uptime)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it? (Elk-playbook.yml, you copying it in /etc/ansible)
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? (you go to /etc/ansible/hosts and nano config file to update which specific machine. You can specify by going by accessing the hosts file and allowing which webserver you would like)
- _Which URL do you navigate to in order to check that the ELK server is running? ([VM_Public_IP:5601/app/kibana])

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
