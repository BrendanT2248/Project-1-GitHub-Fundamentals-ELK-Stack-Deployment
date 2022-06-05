# Project-1-GitHub-Fundamentals-ELK-Stack-Deployment
Project 1 of Cybersecurity Bootcamp. Github Fundamentals, ELK Stack Deployment to Azure platform &amp; Previous Bash Scripts from Linux Units
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Project 1 Network Diagram](https://github.com/BrendanT2248/Project-1-GitHub-Fundamentals-ELK-Stack-Deployment/blob/main/Images/Project%201%20Network%20Diagram.drawio.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Project 1 Network Diagram file may be used to install only certain pieces of it, such as Filebeat.

  - [Filebeat Playbook YAML File](https://github.com/BrendanT2248/Project-1-GitHub-Fundamentals-ELK-Stack-Deployment/blob/main/Ansible/Filebeat%20Playbook.yml.txt)
  - [Filebeat Configuration YAML File](https://github.com/BrendanT2248/Project-1-GitHub-Fundamentals-ELK-Stack-Deployment/blob/main/Ansible/Filebeat%20Configuration.yml.txt)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly functional, in addition to restricting high-traffic to the network.
- What aspect of security do load balancers protect?
  - Load balancers help prevent servers from overloading with traffic as well as optimising productivity, which will in-turn ensure the maximum uptime for the servers the load balancer is serving.
  - This also adds a level of security by rerouting traffic from one server to another. This thereby causes it to eliminate single points of failure, since all servers should be working at a similar level and directing similar levels of traffic. This can help prevent an attack such as  DDoS attack, since traffic needs to go through the load balancer, it will balance out that traffic regardless. 

- What is the advantage of a jump box?
  - A jump box is a system on a network used to access and manage devices in a seperate security zone. A jump box or server is a hardened and moniterewd device that spwans different security zones and provides a controlled means of access between them. 
  - The main advantage of a jump box is the fact that when you need to roll updates out to multiple machines, if a jump box is used, you only need to roll out updates through the jump box itself. Any tools in place for the SAN (Storage Area Network) are on the jump box, making it much easier to roll out security updates. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.

### What does Filebeat watch for?
  - Filebeat is a lgihtweight shipper for forwarding and centralising log data. It monitors the log files that you specify and forwards them through to Elasticsearch/Logstash for indexing. 
### What does Metricbeat record?
  - Metricbeat takes the metrics and statistics from programs you specify. It then collects and ships them to the output that you specifiy, such as Elasticsearch and Logstash. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                    | Function | IP Address                                       | Operating System     |
|-------------------------|----------|--------------------------------------------------|----------------------|
| Jump-Box-Provisioner    | Gateway  | 10.0.0.4 (private) // 20.248.193.199 (public)    |     Linux            |
| Project-1-VM-BT (ELK-VM)| Server   | 10.1.0.4 (private) // 52.243.75.219 (public)     |     Linux            |
| Web-1                   | Server   | 10.0.0.5 (private)                               |     Linux            |
| Web-2                   | Server   | 10.0.0.6 (private)                               |     Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 115.128.3.32 (local PC IP address)

Machines within the network can only be accessed by the Jump-Box-Provisioner.

Which machine did you allow to access your ELK VM? What was its IP address?
- The Jump-Box-Provisioner machine. It's private IP address is 10.0.0.4. Machines within the network can only be accessed via this machine. 

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible    | Allowed IP Addresses |
|----------------------|------------------------|----------------------|
| Jump Box             | No                     | 115.128.3.32         |
| Project-1-VM-BT (ELK-VM)| No                  | 10.0.0.4             |
| Web-1                | No                  | 10.0.0.4             |
| Web-2                | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?
  - This allows you to deploy to multiple servers using a single playbook. This is because the playbook contains several commands in order which will complete the entire rollout, from downloading, installing and configuring certain services. 

The playbook implements the following tasks:
- Install docker.io
- Install python3-pip
- Install docker container
- Increases virtual memory
- Increases maximum VM map count to 262144
- Launches the docker container - ELK

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps command](https://github.com/BrendanT2248/Project-1-GitHub-Fundamentals-ELK-Stack-Deployment/blob/main/Images/docker%20ps%20status%20on%20elk%20machine%202.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 (10.0.0.5)
- Web-2 (10.0.0.6)

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat:
  - Filebeat collects log data from specific files on machines. Examples of log data that Filebeat can collect are audit logs, deprecation logs, gc logs, server logs and slow logs. 
- Metricbeat:
  - Metricbeat helps monitor servers and other specified machines by collecting metrics from the system and services running on the server. It can collect logs from services such as Apache, HAProxy and System logs, 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

#### Filebeat
- Copy the filebeat-configuration.yml file to /etc/ansible/roles.
- Update the configuration file to include the webservers (Web-1 and Web-2) and ELK-VM (Project-1-VM-BT) private IP addresses.
- Run the filebeat playbook, located in /etc/ansible/files, and navigate to ELK-VM (Project-1-VM-BT) to check that the installation worked as expected. Can navigate to it's public IP address in a browser - 
http://52.243.75.219:5601/

By navigating to the ELK-VM's public IP in a browser, we are able to see the GUI of Kibana - 

![Kibana Status]

We can then confirm the status of Filebeat. If the configuration file and playbook were ran successfully, we should see this - 

![Filbeat Status](https://github.com/BrendanT2248/Project-1-GitHub-Fundamentals-ELK-Stack-Deployment/blob/main/Images/filebeat%20status.PNG) 

#### Metricbeat
- Copy the metricbeat-configuration.yml file to /etc/ansible/roles.
- Update the configuration file to include the webservers (Web-1 and Web-2) and ELK-VM (Project-1-VM-BT) private IP addresses.
- Run the metricbeat playbook, located in /etc/ansible/files and navigate to ELK-VM (Project-1-VM-BT) to check that the installation worked as expected. Can navigate to it's public IP address in a browser - 
http://52.243.75.219:5601/

![Metricbeat Status](https://github.com/BrendanT2248/Project-1-GitHub-Fundamentals-ELK-Stack-Deployment/blob/main/Images/metricbeat%20status.PNG)

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
