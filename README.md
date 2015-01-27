# Setup WSO2 using Vagrant

A Vagrant script for setting up WSO2

## With following config
- Sets up Identity Server and Application Server. (plan to add more with different configurations - clustered, with ELB etc...)
- Configure single sign on between them (SAML2 SSO)
- Use ApacheDS that's shipped with Identity Server as the user store
- Use a MySQL server the DB for mounted Registry between the instances

## Pre-requisites

1. Install Vagrant hostmanager plugin - https://github.com/smdahlen/vagrant-hostmanager. <br /><code>$ vagrant plugin install vagrant-hostmanager</code>
2. Download WSO2 Application Server 5.2.1
3. Download WSO2 Identity Server 4.6.0

## Running

Copy Application Server and Identity Server binaries to packs folder. Then do,

<code>$ vagrant up</code>
  
This sets up 3 VMs using VirtualBox. MySQL, Application Server and Identity Server. This also setup single sign on between services.

## Accessing servers

Open a browser and navigate to,<br/>
  Application Server - https://as.test.wso2.com:9443<br/>
  Identity Server - https://is.test.wso2.com:9443

