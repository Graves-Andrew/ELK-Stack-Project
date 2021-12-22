## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Network_Diagram](Diagrams/Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - (_main/YAMLs/pentest.md_)
  - (_main/YAMLs/install-elk.md_)
  - (_main/YAMLs/filebeat-playbook.md_)
  - (_main/YAMLs/metricbeat-playbook.md)


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly secure, in addition to restricting access to the network. The load balancer allows the three machines (web-1, web-2, web-3) to have the same IP address, but only give access to port 80 to connect with DVWA. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system metrics. 
- Filebeat is used for monitoring connections and keep logs, recording everything from ssh connections to the type of browser that is connecting. With filebeat troubleshooting, watching for suspicious activity, and forensics are easier because the logs are easily accessible through Kibana. 
- Metricbeat records system metrics. CPU usage, memory, amount of traffic going to specific machines are all recorded via Metricbeat. This is helpful for giving notice suspicious activity and forensics.


The configuration details of each machine may be found below.

| Name     | Function     | IP Address | Operating System |
|----------|--------------|------------|------------------|
| Jump-Box | Gateway      | 10.0.0.4   | Linux            |
| Web-1    |   DVWA       | 10.0.0.5   | Linux            |
| Web-2    |   DVWA       | 10.0.0.6   | Linux            |
| Web-3    |   DVWA       | 10.0.0.7   | Linux            |
| ELK-VM   | Data Logging | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 66.89.247.217
- 47.183.197.110

Machines within the network can only be accessed via the docker container on the Jump-Box machine.
- 20.118.203.5

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 66.89.247.217, 47.183.197.110   |
| Web-1    | No                  | 20.118.203.5                    |
| Web-2    | No                  | 20.118.203.5                    |
| Web-3    | No                  | 20.118.203.5                    |
| ELK-VM   | No                  | 20.118.203.5                    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because all or specific groups of machines can be updated simultaniously without the need of access each one by one.

The playbook implements the following tasks:
- Downloading docker.io
- Installing python3-pip, so that the machine can interpret the docker module
- Install the docker module
- Change the base virtual memory to allow for more data
- Tell docker to use the increased memory
- Download and launch a docker web container which also uses specific ports
- Enable the docker service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat allows for the logging of connection to the machines. This records everything that a user does including downloads, and user location. You can expect to see even error codes that users recieve, their location, and bytes used.
- Metricbeat records metrics of the three machines. This ensures that we can monitor CPU usage, memory, and bytes simultaniously. It can also give alert when one machine is being used too much or has a spike of activity.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Update the `/etc/ansible/hosts` file to include the three machines' IP addresses and place them in a group and the Elk server in its own group (line 20)
	- Find `#[webservers]` at line `20`, remove the `#` and add the three ip address underneath as so...
		- e.g. 
		`[webservers]`
		`#alpha.example.org`
		`#beta.example.org`
		`#192.168.1.100`
		`#192.168.1.110`
			`10.0.0.5 ansible_python_interpreter=/usr/bin/python3`
			`10.0.0.6 ansible_python_interpreter=/usr/bin/python3`
			`10.0.0.7 ansible_python_interpreter=/usr/bin/python3`
			` `
			`[elk]`
			`10.1.0.4 ansible_python_interpreter=/usr/bin/python3`
			
- Make sure to use the `webservers` group for the Filebeat and Metricbeat and use `ELK`for the elk install

#### DVWA

1. Copy the [Pentest Playbook](Ansible/pentest.md) (ensure that you make a .yml for the playbook) file to `/etc/ansible/`
2. Update the playbook with the group name from `hosts` (usually webservers).
3. Run command `ansible-playbook /etc/ansible/pentest.yml`

#### ELK
1. Copy the [Elk Playbook](Ansible/install-elk.md) (ensure that you make a .yml for the playbook) file to `/etc/ansible/`
2. Update the playbook with the group name from `hosts` (usually ELK).
3. Run command `ansible-playbook /etc/ansible/install-elk.yml`

#### Filebeat
1. Copy the [Filebeat Playbook](Ansible/filebeat-playbook.md) (ensure that you make a .yml for the playbook) file to `/etc/ansible/roles`
2.  run: `curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml`
3. Update the (_main/YAMLs/filebeat_config.md_) file to include the correct ip addresse for ELK machine (lines 1106 and 1806).
4. Update the playbook with the group name from `hosts` (usually webserves).
5. Run command `ansible-playbook /etc/ansible/roles/filebeat-playbook.yml`
6. Run the playbook, and navigate to `http://<ELK-IP-address>:5601/app/kibana` to check that the installation worked as expected.

#### Metricbeat
1. Copy the [Metricbeat Playbook](metricbeat-playbook.md) (ensure that you make a .yml for the playbook) file to `/etc/ansible/roles`
2. run: `curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat`
3.Update the (_main/YAMLs/metricbeat_config.md_) file to include the correct ip addresse for ELK machine (lines 1106 and 1806).
4. Update the playbook with the group name from `hosts` (usually webserves).
5. Run command `ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml`
6. Run the playbook, and navigate to `http://<ELK-IP-address>:5601/app/kibana` to check that the installation worked as expected.
