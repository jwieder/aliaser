# aliaser
aliaser v0.01 

A service written in bash to resolve ongoing issues with IP aliasing when using Amazon EC2 virtual machines without the benefit of ec2-net-utils.

###Features

- Adds and removes routing table entries on RHEL Amazon EC2 virtual machines to support the mapping of multiple public IP addresses to the same EC2 instance.

- Prints secondary public and private IP addresses mappings

- Routing table is stored in memory, allowing greater flexibility with storage options

- Multiple IP mappings are supported

###To Do

The following features are going to be added in the immediate future (next couple of days):

- Add support for Debian/Ubuntu 

- Both systemd & init script functionality to retain IP mappings across reboots and network service restarts (the included aliaser.service file is the beginning of this functionality)

- RPM installation

- Integration with ec2-api-tools

### Instructions

1. Ensure that you have assigned your preferred secondary private IP and secondary Elastic IP to your instance [as outlined here](http://www.joshwieder.net/2015/08/assigning-multiple-ip-addresses-to.html)

2. Download the `aliaser` shell script

3. Assign the script an executable bit as follows:

       #chmod +x aliaser 

4. Execute aliaser using one of a variety of commands:

        ./aliaser -h         Prints list of commands
        ./aliaser version    Prints version and copyright information
        ./aliaser start      Adds routes for secondary IPs to the routing table
        ./aliaser restart    Adds routes for secondary IPs to the routing table
        ./aliaser stop       Removes secondary IP routers and restarts network service
        ./aliaser print      Prints list of secondary private and Public IP address assigned to this instance
