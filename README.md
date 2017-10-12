# aliaser

aliaser v0.02

Copyright ©2017 Josh Wieder

http://www.joshwieder.net/

josh.wieder@live.com

PGP Key 0x30D2ABDB2DF72435

A service written in bash to resolve ongoing issues with IP aliasing when using Amazon EC2 virtual machines without the benefit of ec2-net-utils.

### Features

- Adds and removes routing table entries on RHEL Amazon EC2 virtual machines to support the mapping of multiple public IP addresses to the same EC2 instance.

- Prints secondary public and private IP addresses mappings

- Routing table is stored in memory, allowing greater flexibility with storage options

- Multiple IP mappings are supported

### To Do

The following features are planned:

- RPM installation

- Integration with ec2-api-tools

### Instructions for Red Hat Linux using systemd

1. Ensure that you have assigned your preferred secondary private IP and secondary Elastic IP to your instance [as outlined here](http://www.joshwieder.net/2015/08/assigning-multiple-ip-addresses-to.html)

2. Replicate `aliaser` using git or download as a .zip file
 
3. On your server, copy the systemd service file `aliaser.service` to `/etc/systemd/system/aliaser.service`

4. Copy the shell script `aliaser` to `/usr/sbin/aliaser`

5. Assign the script an executable bit to the shell script as follows:

       `#chmod +x /usr/sbin/aliaser`
       
6. `aliaser` will is now installed! Start the `aliaser` service:

	`#systemctl start aliaser.service`

7. Configure your server to load `aliaser` at boot:

	`#systemctl enable aliaser.service`

8. `aliaser` will now load at boot time and can be managed like any other systemd service:

| Command                                | Description                          |
| -------------------------------------- |:------------------------------------:|
| `#systemctl start aliaser.service`     | Start aliaser service                |
| `#systemctl stop aliaser.service`      |  Stop aliaser service                |
| `#systemctl restart aliaser.service`   |  Restart aliaser service             |    
| `#systemctl disable aliaser.service`   | Prevent aliaser from loading at boot |
| `#systemctl -l status aliaser.service` | View the status of aliaser service   |


9. Execute `aliaser` as a shell script using one of a variety of commands:

| Command                  | Description                                                                                  |
| ------------------------ |:--------------------------------------------------------------------------------------------:|
| `./aliaser -h`           | Prints list of commands                                                                      |
| `./aliaser version`      | Prints version and copyright information                                                     |
| `sudo ./aliaser start`   | Adds routes for secondary IPs to the routing table                                           |    
| `sudo ./aliaser restart` | Adds routes for secondary IPs to the routing table (ATM for service execution purposes only) |
| `sudo ./aliaser stop`    | Removes secondary IP routers and restarts network service                                    |
| `/aliaser print`         | Prints list of secondary private and Public IP address assigned to this instance             |
| `/aliaser test`          | Verifies route additions by querying an outside host                                         |
    
### Bugs & Known Issues

The output of `curl http://169.254.169.254/latest/meta-data/public-ipv4` has proven unpredictable. Unfortunately the Primary Public IP filed for
`./aliaser print` initially relied on this EC2 API call. I am working on formatting another option that will produce stable output. This does not impact the actual functionality of the IP address allocation.

Please be sure to install `curl` (through your package management tool) before installing aliaser

### Licensing and Copyright

Copyright ©2017 Josh Wieder

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License along
with this program; if not, write to the Free Software Foundation, Inc.,
51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
