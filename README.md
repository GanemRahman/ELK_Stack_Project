# ELK Stack Project
## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the **_yml_** and **_config_** files may be used to install only certain pieces, such as Filebeat.

* [My First Playbook](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Docker/pentest.yml "My First Playbook")
* [Hosts](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/hosts.yml "Hosts File")
* [Ansible Configuration](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/ansible.cfg "Ansible Configuration File")
* [Ansible ELK Installation and VM Configuration](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/ELK_Stack/install-elk.yml "ELK Installation and VM Configuration file")
* [Filebeat Config](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Filebeat/filebeat-config.yml "Filebeat Configuration File")
* [Filebeat Playbook](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Filebeat/filebeat-playbook.yml "Filebeat Playbook")
* [Metricbeat Config](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Metricbeat/metricbeat-config.yml "Metricbeat Configuration File")
* [Metricbeat Playbook](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Metricbeat/metricbeat-playbook.yml "Metricbeat Playbook")

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
What aspect of security do load balancers protect? 
  -  **Load balancers protect the servers from becoming overloaded with traffic. They reroute live traffic from one server to another. This is useful in case a server becomes victim to a DDoS attack or otherwise becomes unavailable.**

What is the advantage of a jump box?
 -  **The jump box prevents VMs from being exposed via a public IP Address. This allows us to monitor and log on using a single box. Also it restricts the IP addresses which are able to communicate with the Jump Box.**

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **_network_** and **_system logs_** .
What does Filebeat watch for?
  -  **Filebeat watches for log files and collects events. It forwards them either to Elasticsearch or Logstash for indexing**  

What does Metricbeat record?
  -  **Metricbeat records statistical data from the OS and services running on the servers. It collects and ships them to the output that you specify, such as Elasticsearch or Logstash.**

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name       | Function | IP Address | Operating System |
|------------|----------|------------|------------------|
| Jump Box   | Gateway  | 10.0.0.7   | Linux            |
| Web-1      |  DVWA    | 10.0.0.8   | Linux            |
| Web-2      |  DVWA    | 10.0.0.9   | Linux            |
| ELK-VM     | Server   | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **_Jump-Box-Provisioner_** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- **_Workstation MY Public IP through TCP 5601._**

Machines within the network can only be accessed by **_Workstation and Jump-Box-Provisioner through SSH Jump-Box._**.

Which machine did you allow to access your ELK VM? What was its IP address?
- **_Jump-Box-Provisioner IP : 10.0.0.7 via SSH port 22_**

A summary of the access policies in place can be found in the table below.
| Name                 | Publicly Accessible | Allowed IP Addresses                    |
|----------------------|---------------------|-----------------------------------------|
| Jump-Box-Provisioner | Yes                 | 74.65.245.48 (Workstation IP on SSSH 22)|
| Web-1                | No                  | 10.0.0.7 via SSH port 22                |
| Web-2                | No                  | 10.0.0.7 via SSH port 22                |
| ELK-VM               | No                  | Workstation MY Public IP using TCP 5601 |

