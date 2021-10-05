# ELK-Project                                                                                                                                                                                                     
## Automated ELK Stack Deployment
![diagram_workstation] ![image](https://user-images.githubusercontent.com/91638328/135723773-52ba85ec-d32e-441f-889e-694485eb2275.png)


The files in this repository were used to configure the network depicted below.

:Images/diagram_workstation.png 

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  # install_elk.yml
- name: Configure Elk VM with Docker
 hosts: elkserver
 remote_user: sysadmin
 become: true
 tasks:
   # Use apt module
   - name: Install docker.io
     apt:
       update_cache: yes
       name: docker.io
       state: present

     # Use apt module
   - name: Install pip3
     apt:
       force_apt_get: yes
       name: python3-pip
       state: present

     # Use pip module
   - name: Install Docker python module
     pip:
       name: docker
       state: present

     # Use command module
   - name: Increase virtual memory
     command: sysctl -w vm.max_map_count=262144

     # Use sysctl module
   - name: Use more memory
     sysctl:
       name: vm.max_map_count
       value: "262144"
       state: present
       reload: yes

     # Use docker_container module
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

The Load balancers protect the system from DDoS attacks. It protects the system by shifting the attack traffic. The advantage of a jumpbox is to give a more secure and monitored access. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
Filebeat watches out and monitors log files or any areas you specify.
Metricbeat collects and ships metrics or statistics to different outputs. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web 1     | Web Server  | 10.0.0.5   | Linux            |
| Web 2     | Web Server  | 10.0.0.6   | Linux            |
| ELK      | Monitoring  | 10.2.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 13.82.209.181


Machines within the network can only be accessed by a jump box provisioner.
 Which machine did you allow to access your ELK VM? Sysadmin    
 What was its IP address? 13.78.198.45

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2     |
| Web 1         |      no              |  10.0.0.4                  |
|  Web 2        |          no          |     10.0.0.4               |
|Elk-Server   | no                   | 10.0.0.4                   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible?
The main advantage is Ansible can be run from the command line without the use of configuration files.
Better workflow using Ansible
No extra software needed 


The playbook implements the following tasks:

Install docker.io
Install pip3
Install Docker python module
Increase the virtual memory
Download/Launch docker 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

Name
IP Addresses
Web-1
10.0.0.5
Web-2
10.0.0.6



We have installed the following Beats on these machines:
Specify which Beats you successfully installed? Microbeats

These Beats allow us to collect the following information from each machine:
In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
Filebeat is used to collect log files from specific files on remote machines. Metricbeat collects machine metrics, also it is simply a measurement to tell analysts how it's doing. Examples of Filebeats can be files that are generated by Apache, Microsoft Azure tools. An example of Metricbeat can be CPU usage.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to Ansible Control Node.
- Update the host file to include the webserver and elk
- Run the playbook, and navigate to Kibana 13.78.198.45  to check that the installation worked as expected.

Answer the following questions to fill in the blanks:_
- _Which file is the playbook? filebeat-playbook.yml
 Where do you copy it? /etc/ansible/roles
- _Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/host
 How do I specify which machine to install the ELK server on versus which to install Filebeat on? The ElkServer group has the IP of the vm i installed elk to,and the web server which has all the IPs i need to install filebeat on my vms.
- _Which URL do you navigate to in order to check that the ELK server is running? http://13.78.198.45:5601/

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
$ cd /etc/ansible
$ mkdir files
$ git clone https://github.com/smoo2021/ELK-Project.git
$ cp /Project-1-ELK-Stack-Project/ReadMe/Playbooks/*


git add
git commit -m "This is Lisa"
git config --global user.email "shannonmoorejr@gmail.com"
git config --global user.name "Shannon"
git commit -m "This is Lisa"
git push

cd /etc/ansible
 $ ansible-playbook install_elk.yml elk
 $ ansible-playbook install_filebeat.yml webservers
 $ ansible-playbook install_metricbeat.yml webservers
