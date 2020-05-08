## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/RedTeam Enviornment cfb.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of any of the playbook.yml files may be used to install only certain pieces of it, such as Filebeat.

  - my_playbook.yml - this file will install docker, python-pip, and dvwa via ansible

  - elk-playbook.yml - will configure and install the ELK server

  - filebeat-playbook.yml - will install the configured version of the filebeat.yml file

  - metricbeat-playbook.yml - will install the configured version of the metricbeat.yml file

  - In the ansible.cfg file you will find the sysadmin remote user, this can be modified to add any    
    remote user you desire.

  - In the hosts file you will find the IP Adresses of the Elk server and the other webservers, this too 
    can be modified to fit your networks IP schema. 

This document contains the following details:

- Description of the Topology

- Access Policies

- ELK Configuration

  - Beats in Use

  - Machines Being Monitored

- How to Use the Ansible Build


### Description of the Topology


The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.


Load balancing ensures that the application will have high availablity, in addition to restricting access to the network.



- The Load balancer is in place to be the public facing aspect of our Network. This enables the network to have a single point of exposure. Allowing for the safe configuration of access to the local network via inbound security rules. In addition to the security features the Load balancer ensures availability of the servers. 


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files, system settings, and system performance.


-  Filebeat watches the DVWA server for log files and pushes copies of them to the Elk server.

-  Metricbeat records Operating System information from the DVWA servers and reports them to the ELK server.

The configuration details of each machine may be found below.

_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address     | Operating System |
|----------|----------|----------------|------------------|
| Jump Box | Gateway  | 10.0.0.4       | Linux            |
| DVWA-VM1 | Server   | 10.0.0.5       | Linux            |
| DVWA-VM2 | Server   | 10.0.0.7       | Linux            |
| LB       | Public IP| 40.118.190.112 | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Load Balance machine and Jump Box can accept connections from the Internet. Access to this machines is only allowed from the following IP addresses: 

-  whitelisted IP addresse

   24.90.248.205


Machines within the network can only be accessed by the Jump Box via ssh from the Ansiable container on the appropriate local IP Addresses.

- The only machine that has access to the ELK server is the Jump Box via its outside IP to the outside IP of the ELK server.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible             | Allowed IP Addresses |
|----------|---------------------------------|----------------------|
| Jump Box | Yes   via ssh                   | 13.88.186.191.       |
| Home Net | Yes   ELK Public IP @ port 5602 | 24.90.248.205        |
|          |                                 |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this process saves time and can be deployed across multiple instances.

- By configuring an Ansible playbook an administrator is able to deploy as many instances of the ELK server as desired and in a multitude of other networks. Thus saving time and insuring consistency 
  across deployments. In addition to the preceding features the playbooks can be deployed by junior staff freeing up time for senior staff.

The playbook implements the following tasks:

- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

 ** This ELK server was configured on a Standard D2s v3 (2 vcpus, 8 GiB memory) in Azure **

- 1. Installs and configures docker.io
- 2. Installs python-pip
- 3. Downloads and launches the docker DVWA container
- 4. Configures DVWA container
- 5. Increases the available virtual memory and starts the ELK server

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

- (Images/docker_ps.png)

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

- DVWA-VM1 @ 10.0.0.5 and DVQM-VM3 @ 10.0.0.7

We have installed the following Beats on these machines:

- We have successfully installed Filebeat and Metricbeat on the DVMA machines.

These Beats allow us to collect the following information from each machine:

- Filebeat tracks and copies all the logs in the var directory such as system logs. Which can then be displayed in Kibana.

- Metricbeat uses Elasticsearch to track system operations like cpu usage percent. This too is displayed in Kibana. 

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the elk-playbook.yml, ansibel.confi, and the hosts file to /etc/ansible directory.
- Run the playbook, and navigate to the ELK server via ssh to check that the installation worked as expected.
- After this you can navigate to the public IP of the ELK server at port 5601 and you should see the Kiaba landing page ["ELK Pub IP":5601]

_TODO: Answer the following questions to fill in the blanks:_

- _Which file is the playbook? Where do you copy it? 

   Every file and playbook you need is located in the /etc/ansible directory.
   The ELK playbook is titled elk-playbook.yml, the Ansible playbook is titled my_playbook.yml, the Filebeat playbook is titled filebeat-playbok.yml and 
   the filebeat.yml file is where you can do the configuration of filebeat. The Metricbeat playbook is metricbeat-playbook.yml and the configuration file is metricbeat.yml.

- _Which file do you update to make Ansible run the playbook on a specific machine? 

   To point Ansible to a specific machine you must alter the hosts file under the [webservers] line. This is where you will add the new hosts machines IP address.

- How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

  In the hosts file there is a subsection [elkservers] this is where the IP for the ELK server resides and the [webservers] subsection is where the IPs for the VM servers are.

- _Which URL do you navigate to in order to check that the ELK server is running?

   To navigate to the Kibana data display go to the public IP of the ELK server currently configures as 52.170.91.45 at the 5601 port in a browser window. ie http://52.170.91.45:5601

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

--Useful commands during this Process--


 apt-get update - this command should be run on you VMs to ensure they have the 
                  latest version of software

 sudo apt install docker.io - this will install docker

 sudo docker pull cyberxsecurity/ubuntu:bionic - this will download the docker containers

 sudo docker run -ti bionic/ubuntu bash - this will start the docker container - only run this command 
                                          on the initial setup of the docker container

 sudo docker container list -a - this command will list all the docker containers on the current VM

 sudo docker start "container name" - this will stat the container
 
 sudo docker ps - will also list containers

 sudo docker attach "container name" - this will attach you to the container

 ansible -m ping all - to ping the containers in Ansible this will confirm you have access to them

 
                                           