## Introduction

This is a project designed to establish a virtual cloud network and monitor various metrics therein. 
The instructions contained within this readme file should allow users to replicate our steps and reap similar results. It is not intended to serve as the model for an ideal secure network, but rather allow users to explore and sample real data inside a self-contained virtual network.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

!(Images/network_diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  -install-elk.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unauthorized inbound access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs on the network for changes to their file systems as well as system metrics.

The configuration details of each machine may be found below.

| Name       | Function          | IP Address | Operating System     |
|------------|-------------------|------------|----------------------|
| Jump Box   | Gateway           | 10.1.0.7   | Linux - ubuntu 18.04 |
| Web-1 VM   | Web Server        | 10.1.0.5   | Linux - ubuntu 18.04 |
| Web-2 VM   | Web Server        | 10.1.0.6   | Linux - ubuntu 18.04 |
| Web-3 VM   | Web Server        | 10.1.0.8   | Linux - ubuntu 18.04 |
| ELK Server | Monitoring Server | 10.0.0.4   | Linux - ubuntu 18.04 |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 155.186.99.144

Only the machines within the network can access each other. No outside access is permitted.

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses |
|------------|---------------------|----------------------|
| Jump Box   | Yes                 | 155.186.99.144       |
| Web-1 VM   | No                  | 10.1.0.7/32          |
| Web-2 VM   | No                  | 10.1.0.7/32          |
| Web-3 VM   | No                  | 10.1.0.7/32          |
| ELK Server | No                  | 10.1.0.7/32          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because doing so ensures consistency and replicability among subsequent created VMs.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Installing and configuring docker
- Installing python3-pip
- Installing Docker python module
- Increasing system virtual memory for necessary tasks
- Launching the docker container 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/dockerpsoutput.png)
(Images/dockerpsoutput2.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines: 10.1.0.5, 10.1.0.6, 10.1.0.7


We have installed the following Beats on these machines:
- Filebeat
- Metricbeat
- Packetbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors changes to the filesystem. These logs are forwarded to Elasticsearch and Logstash for indexing
- Metricbeat collects system metrics. Specifically we use this to monitor usage of system resources as well as SSH login attempts and subsequent user activity.
- Packetbeat collects network traffic data which we can then use to monitor and analyze activity.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install_elk.yml file to the /etc/ansible/ directory.
- Update the /etc/ansible/hosts file to include the IP of your target ELK server machine.
- Run the playbook, and navigate to http://[Your ELK server's public IP]:5601/app/kibana/ to check that the installation worked as expected.

Additionally, the following files will need to be staged in your /etc/ansible directory so that they may be properly copied to their appropriate directories. Again, make sure to modify directory paths and file names to accurately reflect your system:

filebeat_config.yml
metricbeat_config.yml

Use the following commands to run the playbook. Be sure to modify file and directory names within these files to accurately reflect your system setup:

cd /etc/ansible
ansible-playbook install_elk.yml
ansible-playbook install_filebeat.yml
ansible-playbook install_metricbeat.yml


### Findings and Observations
Following successful deployment, we executed a combination of SSH penetration testing, CPU maxing and generating denied wget requests in order to monitor the stress of each test on our virtual system resources.

For the SSH testing, running an infinite loop of failed login attempts flooded the system logs with an expectedly interminable amount of connection failures.

The CPU stress tests yielded intriguing results as we were able to quickly tax system resources to their maximum threshold. An unsecured, unhardened system would be easily brought to its knees with this sort of attack.

Lastly, in running a barrage of failed wget requests, we were able to monitor sharp spikes in network traffic and memory usage. This was in addition to running a file request loop that rapidly consumed available hard disk space, necessitating a thorough removal of all excess data.

### Conclusion

This project has been an enlightening exercise in network architecture through deploying, configuring and monitoring a virtual cloud network to study the ways that security controls are employed within such an environment. While addmittedly rudimentary, we aim to build off of the knowledge gained in this scenario for future projects and pursuits. Any constructive comments are appreciated.
