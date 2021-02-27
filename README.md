## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Elk Diagram (Diagrams/HW13Github_Elk ResourceGroup_Diagram.png)

https://github.com/jarlamin/zona/blob/main/Diagrams/HW13Github_Elk%20ResourceGroup_Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: (Ansible/Elk_Playbook.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable (secured and available), in addition to restricting access/traffic to the network.
- _TODO: What aspect of security do load balancers protect? = 

A load Balancer adds additonal layers of security to a website/ server without any changes to your application. The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider. is the process of distributing workloads across multiple servers, collectively known as a server cluster. The main purpose of load balancing is to prevent any single server from getting overloaded and possibly breaking down. The Web Application Firewall (WAF) in the load balancer protects your website from hackers and includes daily rule updates just like a virus scanner. The load balancer can Authenticate User Access by requesting a username and password before granting access to a website to protect against unauthorized access. 

 What is the advantage of a jump box?_

 A jump box is a secure computer that all admins first connect to before launching any administrative task or use as an organization point to connect to other servers or untrusted environments. It is an intermediary host or an SSH gateway to a remote network, through which a connection can be made to another host in a dissimilar security zone, for example a demilitarized zone (DMZ). It bridges two dissimilar security zones and offers controlled access between them.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the __logs___ and system _traffic____.

- _What does Filebeat watch for?_

    Filebeat monitors the log files or locations that are specified, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
    
- What does Metricbeat record?_

   Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.7   | Linux            |
| Web-1    | DVWA     | 10.0.0.10  | Linux            |
| web-2    | DVWA     | 10.0.0.9   | Linux            |
| Web-3    | ELK      | 10.0.0.12  | Linux            |
| Jar_ELK  | ELK      | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __Jump Box___ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_ 45.178.73.238

Machines within the network can only be accessed by __JumbBox ___.
- _TODO: Which machine did you allow to access your ELK VM? __JumpBox _ What was its IP address? _10.0.0.7__

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 45.178.73.238        |
| Web-1    | No                  | 10.0.0.7/ loadbalancer ip |
| Web-2    | No                  | 10.0.0.10/ LBalancer ip   |
| Web-3    | N0                  | 10.0.0.12/LB IP           |
| Jar_ELK  | N0                  | 10.1.0.4/ LB IP           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- What is the main advantage of automating configuration with Ansible?______The primary benefit of Ansible is it allows IT administrators to automate away the drudgery from their daily tasks. That frees them to focus on efforts that help deliver more value to the business by spending time on more important tasks_ 

The playbook implements the following tasks:

- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...

    ...Install Python3-pip
    ...Install Docker using pip
    ...Install ELK
    ...Enable docker service on restart

- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Docker ps output](Ansible/docker_ps.png) 

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring__10.0.0.10, 10.0.0.9, 10.0.0.12

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed__filebeat and metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

__-Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

-Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash__ 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ___playbook.yml__ file to __ansible_container_in_the_Jump_Box ___.
- Update the __hosts ___ file to include...the IPs of the machines (Web-1 10.0.0.10, Web_2 10.0.0.9, Web_3 10.0.0.12 and elk_project 10.1.0.4 )
- Run the playbook, and navigate to _23.100.127.27:5601/app/kibana___ to check that the installation worked as expected.

 Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?

 __We_have filebeat and metricbeat playbooks which are copied in the abisble container_ 

- _Which file do you update to make Ansible run the playbook on a specific machine?_ filebeat.yml and filebeat config. How do I specify which machine to install the ELK server on versus which to install Filebeat on?_By adding the ELK's IP to the hosts file
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._