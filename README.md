# ELK Stack Project
## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/ELK_Stack_Project_Network_Diagram.png)

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
| Jump-Box-Provisioner | Yes                 | 74.65.245.48 (Workstation IP on SSH 22)|
| Web-1                | No                  | 10.0.0.7 via SSH port 22                |
| Web-2                | No                  | 10.0.0.7 via SSH port 22                |
| ELK-VM               | No                  | Workstation MY Public IP using TCP 5601 |

### Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually.
What is the main advantage of automating configuration with Ansible?  
  - **_Ansible lets you simply and swiftly utilize multi-tier applications using a YAML playbook._**
  - **_There's no need to write custom code to automate systems._**
  - **_Ansible figures out how to get systems to the correct state the user or organization needs them to be in._**

The playbook implements the following tasks:
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
- Specify a different group of machines:
      ```yaml
        - name: Config elk VM with Docker
          hosts: elk
          become: true
          tasks:
      ```
  - Install Docker.io
      ```yaml
        - name: Install docker.io
          apt:
            update_cache: yes
            force_apt_get: yes
            name: docker.io
            state: present
      ``` 
  - Install Python-pip
      ```yaml
        - name: Install python3-pip
          apt:
            force_apt_get: yes
            name: python3-pip
            state: present

          # Use pip module (It will default to pip3)
        - name: Install Docker module
          pip:
            name: docker
            state: present
            `docker`, which is the Docker Python pip module.
      ``` 
  - Increase Virtual Memory
      ```yaml
       - name: Use more memory
         sysctl:
           name: vm.max_map_count
           value: '262144'
           state: present
           reload: yes
      ```
  - Download and Launch ELK Docker Container (image sebp/elk)
      ```yaml
       - name: Download and launch a docker elk container
         docker_container:
           name: elk
           image: sebp/elk:761
           state: started
           restart_policy: always
      ```
  - Published ports 5044, 5601 and 9200 were made available
      ```yaml
           published_ports:
             -  5601:5601
             -  9200:9200
             -  5044:5044   
      ```
- The following screenshots displays the result of running `docker ps` and `docker ps -a` after successfully configuring the ELK instance.  

ELKserver
---------
![DockerPS](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/Docker/ELK-VM_sudo_docker_ps.png "ELKserver")  

Jump-Box-Provisioner
--------------------
![DockerPS](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/Docker/Jump-Box-Provisioner-sudo_docker_ps-a.png "Jump-Box-Provisioner")  

Web-1
-----
![DockerPS](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/Docker/Web-1_sudo_docker_ps_-a.png "Web-1")  

Web-2
-----
![DockerPS](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/Docker/Web-2_sudo_docker_ps_-a.png "Web-2")  

## Using the Playbook  
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

- Verify the Public IP address to see if it has changed. [What Is My IP?](https://www.whatismyip.com/)
- If changed then update the Security Rules that uses the Public IPv4 address

SSH into the control node and follow the steps below:

- Copy the **_yml_** file to the  **_ansible folder._**
- Update the **_config_** file to include **_remote users and ports._**
- Run the playbook, and navigate to **_Kibana [(Your Static Public IP Address on your ELK-VM):5601 (in my case it is 52.190.250.118:5601)]_** to check that the installation worked.  

  -  [Kibana Welcome Page](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/ELK_Configurations/Welcome_to_Kibana.png "Welcome to Kibana")

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring  
   - Web-1: 10.0.0.8  
   - Web-2: 10.0.0.9  

We have installed the following Beats on these machines:
- Filebeat  
  - [Filebeat Module Status](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/ELK_Configurations/Filbeat/Filebeat_data_success.png "Filebeat Data Successful")
- Metric  
  - [Metricbeat Module Status](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/ELK_Configurations/Filbeat/Filebeat_data_success.png "Metricbeat Data Successful") 

These Beats allow us to collect the following information from each machine:  
- Filebeat collects log files from systems and servers such as Apache, Microsft Azure tools and web servers, MySQL databases.    
  - [Filebeat Module Kibana Dashboard ](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/ELK_Configurations/Filbeat/Filebeat_system_dashboard.png "Filebeat Dashboard")

- Metricbeat monitors VM stats,  CPU core stats, filesystem stats, memory stats and network stats.
    - [Metricbeat Module Kibana Dashboard](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/Diagrams_and_Images/ELK_Configurations/Metricbeat/Metricbeat_Dashboard.png "Kibana Dashboard with Metricbeat")

_: Answer the following questions:_
- _Which file is the playbook?_    
   -For Ansible create **_[My First Playbook](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Docker/pentest.yml "My First Playbook")_**  
   -For Filebeat create **_[Filebeat Playbook](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Filebeat/filebeat-playbook.yml "Filebeat Playbook")_**  
   -For Metricbeat create **_[Metricbeat Playbook](https://github.com/GanemRahman/ELK_Stack_Project/blob/main/ansible/Metricbeat/metricbeat-playbook.yml "Metricbeat")_**  
    _Where do you copy it?_  
        -**_/etc/ansible/_**  
- _Which file do you update to make Ansible run the playbook on a specific machine?_  
   -**_/etc/ansible/hosts file (IP of the Virtual Machines)._**
- _How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
   - **_Designate two separate groups in the etc/ansible/hosts file. One of the groups will be webservers which has the IPs of the 2 VMs that Filebeat will be installed onto._** 
- _Which URL do you navigate to in order to check that the ELK server is running?
   - **_http://52.190.250.118:5601/app/kibana#/home_**

