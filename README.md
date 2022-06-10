# ELK-Stack-Project-By-Briana-Norman
ELK Stack - Briana Norman Project 
## ELK Stack Deployment (with automation)

The files in this repository were used to configure the network depicted below.
![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/diagram/Briana%20Norman%20NetWork.jpg)
### "Azure Network with Elk Server Diagram"

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the [AllPlayBooks aka JumpBox Files](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/tree/main/Jumpbox%20Files) file may be used to install only certain pieces of it, such as Filebeat.

- [ElkPlaybook.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/elkplaybook.yml) - this Playbook installs Elk Ansible container. 
- [filebeat-playbook.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/filebeat-playbook.yml) - This is a Playbook that installs and launches the filebeat service
- [filebeatconfig.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/filebeatconfig.yml) - This is the full FileBeat Configuration documented in a file listing all non-deprecated options and its comments that have its corresponding updated IP Addressess
- [metricbeat-playbook.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/metricbeat-playbook.yml) - This Metricbeat Playbook shows what is being installed to the Docker Metrics/metricbeat
- [metricbeat-config.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/metricbeat-config.yml) - This metricbeat configuration file illustrates its  most commonly used options and highlights them
- [vmplaybook.yml](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/vmplaybook.yml) - This is a configured Web Docker VM that has Ansible along with Launch Web DVWA Docker Container
- [host file](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/host%20file) - This is the default ansible 'hosts' file
- [Config](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/Jumpbox%20Files/Config) - This is the configuration file for ansible



This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

**_What aspect of security do load balancers protect? What is the advantage of a jump box?_**
- Load Balancers play a significant role in both aspects of security and protection. The function of the "off-loading" in a load balancer is critical for defending company's or organizations from DDoS (denail of service) attacks by balancing or shifting the incoming traffic  and additional attack traffic from a company or corporate server to multiple servers which can include a public cloud provider, which then prevent the DDoS from executing because the server has not been overloaded with request traffic on one or smaller servers.

**The advantages of a Jump box are:**
- Setting up Full Control for accessing the jumpbox are completely customizable. Controlling the access by a specific ip address from your work station allows better security and protocols. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the machine and its metrics aka traffic and system logs.

**_What does Filebeat watch for?_**
- Filebeat forwards things like; monitoring log files (full or seleced and specified) along with a list log events to Logstash for indexing (or to Elasticsearch for indexing).

**_What does Metricbeat record?_**
- Metricbeat is a simplified (or lightweight) agent/shipper that is utilized in collecting the systems metrics along with more things like application metrics and then forwards them to Elasticsearch aka Elastic Stack Server. 

**The configuration details of each machine may be found below.**

_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.
| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web1     |DVWA Container|10.0.0.5| Linux            |
| Web2     |DVWA Container|10.0.0.6| Linux            |
| Web3     |DVWA Container|10.0.0.7| Linux            |

### Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed connection through the "LoadBalancer FrontEnd IP" which has the following IP addresses:

**Add whitelisted IP addresses_**
Machines within the network can only be accessed by _____.
- 13.67.200.90

**Which machine did you allow to access your ELK VM? What was its IP address?**
- 10.0.0.4

**A summary of the access policies in place can be found in the table below**.

| Name     | Publicly Accessible |Allowed IP Addresses  |
|----------|---------------------|----------------------|
| Jump Box | No                  |13.67.200.90/22, 13.67.200.90/80|
| Web1     | No                  |10.0.0.4              |
| Web2     | No                  |10.0.0.4              |
| Web3     | No                  |10.0.0.4              |


### Elk Configuration
**Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...**
_it benefits overall costs reductions due to the downtime and costs not occured prior to deployment. Additionally, due to the limited need to monitor automated scripted tasks the consistency and reliability increases tenfold because of these checked, tested, made and deloyed scripts. Better operations management and efficiency by enabling and putting IaC aka Infrastructure as Code. 

---

### The playbook implements the following tasks:
    ---
    - name: Configure Elk VM with Docker
      hosts: elk
      remote_user: RedAdmin
      become: true
      tasks:

#### Using apt install module - _installing docker.io using apt
    - name: installing docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

#### Using apt install module - _installing python3-pip using apt
    - name: installing pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

#### Using python module - _installing the docker python module with pip
    - name: installing python module
      pip:
        name: docker
        state: present

#### Using a sysctl module - _increasing virtual memory to "262144"
    - name: more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

#### Using a DockerContainer module - _download and launch the docker elk container with the image "sebp/elk:761"
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

#### Using a control system daemon module aka a systemd module - _enabling docker service on Boot systemd
    - name: enabling service doc
      systemd:
        name: docker
        enabled: yes



#### The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/1.jpg)

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/2.jpg)


**_![click here to view all screenshots in the listed "screenshot" folder](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/tree/main/screenshots)_**



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

**We have installed the following Beats on these machines:**
- Filebeat
- Metricbeat

**These Beats allow us to collect the following information from each machine:**
- System Metrics: CPU, memory, network usages, disk usage, services (like Redis, Zookeeper, Nginx, MongoDB, and Apache)
- These Beats also allow us to collect System Log Files and furthermore, can collect and log specifics like user and locations and the selected users web traffic 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
- Copy the configuration file to the Jumpbox _/etc/ansible/files_.
- Update the hosts file to include the target machines IP address in the appropiate hosts group and if it does not exist then you create it.

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/7.jpg)


**- Run the playbook, and navigate to the "ELK Server Kibana" as a GUI webpage accessed from the web browser which allows you to check that the installation worked as expected.**

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/1.png)

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/6.png)

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/8.png)

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/g.jpg)




**- _Checking System Logs on Kibana_**

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/f.jpg)




**- _Checking Filebeat System in Dashboards on Kibana_**

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-Bobbrinksz/blob/main/screenshots/d.jpg)


### **_Answer the following questions to fill in the blanks:_**

  **_Which file is the playbook? Where do you copy it?_**

- The files with the "YAML" or ".yml" end extension in the file names are the files that are PLAYBOOKS.
- They are located (like most Ansible playbooks) in a docker container on a Jumpbox and placed in the directory, _/etc/ansible/roles directory. 

**_Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_**

- Update the _/etc/ansible/hosts_ file and make sure the targets machines IP addresses have the appropiate "host header" which is bracketed and remove the # comment out. 
- You specify which machine to install the ELK server on by indicating this in the playbooks themselves where to install (whether it be on the ELK Server and/or Filebeat or Metricbeat) where the the ELKS hosted machine indicates such for the host file. 

**_Which URL do you navigate to in order to check that the ELK server is running?_**

- All 3 URLS below worked for checking if the ELK server is running on the GIU interface website Kibana
  - https://20.125.35.100:5601/app/kibana
  - http://20.125.35.100:5601/app/kibana#/home/tutorial/dockerMetrics 
  - http://20.125.35.100:5601/app/kibana#/home

### **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

**_The following instructions will demonstrate commands used to "run" and then "deploy" one playbook at a time._**

### Generating SSH Key-Gen
  - Run in the command line: _ssh-keygen_ (to create a new SSH key-gen pair).
  - SSH into the Jumpbox after the public key from the generated key pair is uploaded using the following line command, _ssh admin-username@VM-public-IP_ 
    - (substitute "admin-username" for our current example being, "RedAdmin" and "VM-public-IP" is "40.83.244.89" for this examples network set up = Command: **_ssh RedAdmin@40.83.244.89_**.

### Installing Docker.io 
On Jumpbox install Docker.io by running the command:
  - sudo apt update
  - sudo apt install docker,io
  - sudo systemctl status docker

### Pull for Ansible Container
Once Docker.io is installed and up and running, execute the commands:
  - sudo docker pull cyberxsecurity/ansible
  - sudo su
  - docker run -ti cyberxsecurity/ansible:latest bash **_This RUN command is done ONCE - if you RUN multiple times, you will NEED TO REMOVE_** use command line:
    - docker rm {the container number of your duplicates you made running RUN more then once - Just don't do it}. 

### Bootup and Accessing Your Docker Container
  - sudo docker container list -a
  - sudo docker start {name of container you wish to bootup}
  - sudo docker attach {name of container you wish attach to access}
- Generate new SSH key-gen (like performed in previous steps above) provind the authentication needed to access the docker container from the web server. 

### Ansible Containers on Jumpbox's - How to Run Playbooks
  - Using the command: _ansible-playbook [path and name of playbook]_
    - **Example:** _ansibleplaybook /etc/ansible/vmplaybook.yml_

--- 

_The specified and specific tasks listed at the top of this document will be regurgitated in use for application in creating these playbooks, regarding variances of execution abilities._



