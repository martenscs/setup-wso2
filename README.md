# Setup WSO2 using Vagrant

A Vagrant script for setting up WSO2

## With following config
- Sets up Identity Server and Application Server. (plan to add more with different configurations - clustered, with ELB etc...)
- Configure single sign on between them (SAML2 SSO)
- Use ApacheDS that's shipped with Identity Server as the user store
- Use a MySQL server the DB for mounted Registry between the instances

## Pre-requisites

1. Install Vagrant hostmanager plugin - https://github.com/smdahlen/vagrant-hostmanager. <br /><code>$ vagrant plugin install vagrant-hostmanager</code>


## Running

Copy Application Server and Identity Server binaries to packs folder. Then do,

<code>$ vagrant up</code>
  
This sets up 3 VMs using VirtualBox. MySQL,Data Services Server,WSO2 Enterprise Service Bus, Application Server,API Manager,Data Analytics Server and Identity Server. This also setup single sign on between services.

## Accessing servers

Open a browser and navigate to,<br/>
 
  WSO2 Identity Server        - https://is-master.tmwsystems.com:9443<br/>
  WSO2 Data Services Server   - https://is-master.tmwsystems.com:9444<br/>
  WSO2 Enterprise Service Bus - https://is-master.tmwsystems.com:9445<br/>
  WSO2 Application Server     - https://as-master.tmwsystems.com:9443<br/>
  WSO2 API Manager            - https://as-master.tmwsystems.com:9444<br/>
  WSO2 Data Analytics Server  - https://as-master.tmwsystems.com:9445<br/>