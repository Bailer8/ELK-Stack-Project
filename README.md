## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

file:///home/kali/Desktop/SCHOOL/elk_stack_project_files/Elkstack%20Project%20Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

file:///home/kali/Desktop/SCHOOL/elk_stack_project_files/dvwa-playbook.yml
file:///home/kali/Desktop/SCHOOL/elk_stack_project_files/elk-playbook.yml
file:///home/kali/Desktop/SCHOOL/elk_stack_project_files/filebeat-playbook.yml
file:///home/kali/Desktop/SCHOOL/elk_stack_project_files/metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting the DDoS to the network.
- The jump box controls access to the other machines by allowing connections from specific IP addresses and forwarding to those machines.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes.
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.


| Name       | Function    | IP Address | Operating System |
|------------|-------------|------------|------------------|
| Jump Box   | Gateway     | 10.1.0.4   | Linux            |
| DVWA-VM2   | VM w/Docker | 10.1.0.17  | Linux            |
| DVWA-VM3   | VM w/Docker | 10.1.0.12  | Linux            |
| elk server | Elk w/Docker| 10.1.0.16  | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address:
104.42.20.96

Machines within the network can only be accessed by Jump Box Provisioner.

Only the Jump Box Provisioner can access the ELK Server VM. The public IP address of the ELK Server is 138.91.225.225. 

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses |
|------------|---------------------|----------------------|
| Jump Box   | Yes                 | 104.42.20.96         |
| DVWA-VM2   | No                  | 10.1.0.17            |
| DVWA-VM3   | No                  | 10.1.0.12            |
| Elk-Server | No                  | 10.1.0.16            |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible delivers all modules to remote systems and executes tasks as needed to enact the desired configuration. 
Ansible is user friendly and secure. The only requirements for Ansible are a password or SSH key in order to start managing systems and do not require installation of any agent software. Ansible relies on OpenSSH, which is available on a wide variety of 
platforms and when security issues in OpenSSH are discovered, they are patched quicly.  

The playbook implements the following tasks:
- Configure Elk VM with Docker and install docker.io
- Install pip then install the Docker python module
- Increase virtual memory
- Download and launch a docker elk container

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

| Name       | IP Address | 
|------------|------------|            
| DVWA-VM2   | 10.1.0.17  | 
| DVWA-VM3   | 10.1.0.12  |


We have installed the following Beats on these machines:

| Name       | Beats                   |
|------------|-------------------------|            
| DVWA-VM2   | Filebeat and Metricbeat | 
| DVWA-VM3   | Filebeat and Metricbeat |


These Beats allow us to collect the following information from each machine:

Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration and playbook files to the target directories.
- Update the hosts and ansible.cfg files to include the target machine's IP.
- Run the playbook, and navigate to elk webpage (Kibana) to check that the installation worked as expected.

The playbook beat files are located in /etc/ansible/roles and the configuration files are located in etc/ansible/files. 

- The playbook files are using the configuration files to download and install beat files in the target machines. The configuration files are updated to be applicable with the source location in the ansible. We update the /etc/ansible/hosts file with the target machine's IP addresses to specify where the beats are installed. After running the beats playbooks, we navigate to 138.91.225.225:5601 to check the collected log data.

Commands needed to run the playbook:

- eval "$(ssh-agent -s)"
- ssh-add
- ansible -m ping all
- sysctl -w vm.max_map_count=262144
- ansible-playbook dvwa-playbook.yml
- ansible-playbook elk-playbook.yml
- ansible-playbook filebeat-playbook.yml
- ansible-playbook metricbeat-playbook.yml

