# ELK-Stack-Project
Configuring an ELK stack server

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/88013180/146472848-0da79cdc-4209-4665-be41-1b2e80bd4a8c.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

**my-playbook.yml**

![image](https://user-images.githubusercontent.com/88013180/146481852-0151ebdb-9750-4cb5-81e1-4a454ea5a858.png)
      
**install-elk.yml**

![image](https://user-images.githubusercontent.com/88013180/146481891-5d9ed1ac-1b22-43be-b569-df4c136d5566.png)
        
**filebeat-playbook.yml**

![image](https://user-images.githubusercontent.com/88013180/146481917-bc4529a4-8a1d-4df3-8d74-38c79741d629.png)
      
**metricbeat-playbook.yml**

![image](https://user-images.githubusercontent.com/88013180/146481938-54263c2d-a3fe-497b-9426-94ef2ed9ea28.png)

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
Load balancers help protect against DDoS attacks by rerouting traffic and eliminating single points of failure. If one server were to be attacked, the rerouting of traffic can make it harder to deplete the resources. The advantage of using a jump box is to allow access to your machines that are not exposed to the public. Having this one point of entry allows for better monitoring, opposed to having to monitor connections between all machines. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- What does Filebeat watch for? Filebeat logs information about the file system, including which files have changed and when.
- What does Metricbeat record? Metricbeat collects metrics from the operating system and from services running on the server then takes the metrics and statistics that it collects and ships them to the output that you specify.

The configuration details of each machine may be found below.

| Name     | Function   | IP Address | Operating System |
|----------|------------|------------|------------------|
| Jump Box | Gateway    | 10.0.0.6   | Linux            |
| Web-1    | Web-server | 10.0.0.9   | Linux            |
| Web-2    | Web-server | 10.0.0.10  | Linux            |
| Web-3    | Web-server | 10.0.0.8   | Linux            |
| ELK      | ELK-Stack  | 10.1.0.4   | Linux            |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 20.124.205.111

Machines within the network can only be accessed by using ssh from the jump box.
- Which machine did you allow to access your ELK VM? JumpBoxProvisioner
- What was its IP address? 20.124.205.111

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 74.124.173.127       |
| Web-1    | No                  | 10.0.0.6             |
| Web-2    | No                  | 10.0.0.6             |
| Web-3    | No                  | 10.0.0.6             |
| ELK      | No                  | 10.0.0.6             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because using Ansible speeds up the process by allowing you to install on many machines at once instead of individually. Using Ansible also reduces the risk of error in the code.

The playbook implements the following tasks:
- Specifies Elk as target group of machines
- Installs docker.io (used for running containers) and python3-pip (used to install Python software) apt packages
- Installs docker (Python client for Docker) pip package
- Configures the container to start with the port mappings of 5601:5601, 9200:9200, 5044:5044
- Start the container and enables the docker service to start up automatically after restart.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://user-images.githubusercontent.com/88013180/146486729-cc06ce97-9475-427e-903e-97d2a08d17a3.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.9
- Web-2: 10.0.0.10
- Web-3: 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects system logs and then sends them to a SIEM, in this case Kibana, which allows us to analyze the data in human readable form.
- Metricbeat collects system metrics which allows us to see data such as the CPU usage on our Web machines.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to /etc/ansible/roles/filebeat-playbook.yml
   - Command: cp filebeat-playbook.yml /etc/ansible/roles/filebeat-playbook.yml 
- Update the hosts file to include two host groups. One group will be under webservers which includes the Web-1, Web-2 & Web-3 private IPs and the other host group will be under elk and will have the ELK VM private IP. The two host groups allow you to specify which group of machines to run each playbook on.
   - Command: cd /etc/ansible
   - Command: nano hosts
- Run the playbook, and navigate to http://[ELK VM Public IP]:5601/app/kibana to check that the installation worked as expected.
   - Command: ansible-playbook filebeat-playbook.yml
