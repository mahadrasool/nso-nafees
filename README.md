# Deploying HAproxy and Flask app using Ansible

This project demonstrates how to deploy a Flask application using HAproxy load balancer using Ansible. 

## Overview

The basic service is a simple (flask) app, that only answers requests, by replying with the time and hostname of the host that replied. The service is found in the following git repository: https://github.com/patrikarlos/NSO_A2. Use the 'application2.py' for this assignment.

As we are expecting our service to be a massive hit with our intended customers, so we plan to balance the load across three (3) servers. The load-balancer that you need to use is HAproxy. 

The logical network/service map would be as follows: 

(Internet)--->HAproxy 
             (internal network using private address range; 10.0.1.0/27) 
              +--->devA 
              +--->devB 
              +--->devC 
(Internet)--->Bastion 

The site consists of 5 hosts: HAproxy, devA, devB, devC, Bastion, and an internal site-local network (A network connecting the above 5). The HAproxy host acts as the entry point to the service and load balance between devA, devB, and devC.  

The dev servers use private addresses from the site-local network and do not have direct public access.  

The last host, Bastion, is acting as an SSH (Secure Shell) entry point to the internal network. i.e., if you connect to Bastion, you can ssh to all the other hosts within the site-local network. Bastion host acts as a secure access point for development.  

## Requirements

- Ansible
- Python 3

## Configuration

- Update the hosts file with the appropriate IP addresses.
- Update the `site.yaml` file with the correct IP addresses for the hosts and the `haproxy.cfg` file with the correct IP addresses for the backend servers.
- Create an SSH config file for the host that allows the host to use the Bastion host as a jump host.
- Ensure that the Bastion host also has SSH access to all the site-local hosts, using SSH keys.

## Usage

1. Update the `hosts` file with the appropriate IP addresses.
2. Update the `site.yaml` file with the correct IP addresses for the hosts and the `haproxy.cfg` file with the correct IP addresses for the backend servers.
3. Create an SSH config file for the host that allows the host to use the Bastion host as a jump host.
4. Ensure that the Bastion host also has SSH access to all the site-local hosts, using SSH keys.
5. Run the Ansible playbook `site.yaml` using the following command: 

ansible-playbook --ssh-common-args "-F /path/to/ssh/config/file" -i hosts site.yaml

## Tests

- `deploy_environment(NSO_$tag)` 
    - `ansible-playbook --ssh-common-args "-F /home/joedoe/.ssh/include/NSO_$tag" -i hosts site.yaml  | tee $logFile` 
- `test_responses(3, $floatIP)`
