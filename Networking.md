vCloud Air Network Management with vca-cli
------------------------------------------

This section describes the network operations available through **vca-cli**


Network Commands
----------------

The following is a list of network related commands:

    dhcp      Operations with Edge Gateway DHCP Service
    firewall  Operations with Edge Gateway Firewall Rules
    gateway   Operations with Edge Gateway
    nat       Operations with Edge Gateway NAT Rules
    network   Operations with Networks
    org       Operations with Organizations
    vdc       Operations with Virtual Data Centers (vdc)
    vpn       Operations with Edge Gateway VPN


Network Configuration Overview
------------------------------

The `org` command provides the list of networks defined in the organization:

    $ vca org info
    
    Details for org 'M735816878-4430':
    | Type       | Name                                 |
    |------------+--------------------------------------|
    | Org Id     | 0ea8e6de-8dc0-4b2d-83a0-629544be5465 |
    | Org Name   | M735816878-4430                      |
    | catalog    | blueprints                           |
    | catalog    | Public Catalog                       |
    | orgNetwork | blueprints-network                   |
    | orgNetwork | M735816878-4430-default-routed       |
    | orgNetwork | M735816878-4430-default-isolated     |
    | vdc        | M735816878-4430                      |

The `vdc` command lists the networks available in the virtual data center and the edge gateways (usually one), with a summary of the configuration:

    $ vca vdc info
    
    Virtual Data Center 'M735816878-4430' for 'default' profile; details:
    | Type         | Name                             |
    |--------------+----------------------------------|
    | edge gateway | M735816878-4430                  |
    | network      | blueprints-network               |
    | network      | M735816878-4430-default-routed   |
    | network      | M735816878-4430-default-isolated |
    | vApp         | ubu                              |
    | vApp         | nodejs_host_e538b                |
    | vApp         | mongod_host_af84c                |
    | vAppTemplate | managed_template                 |
    Compute capacity:
    | Resource    |   Allocated |   Limit |   Reserved |   Used |   Overhead |
    |-------------+-------------+---------+------------+--------+------------|
    | CPU (MHz)   |       30000 |   30000 |      15000 |      0 |          0 |
    | Memory (MB) |       61440 |   61440 |      61440 |   3072 |         97 |
    Edge Gateways:
    | Name            | External IPs                 | DHCP   | Firewall   | NAT   | VPN   | Routed Networks                                    | Syslog   | Uplinks   |
    |-----------------+------------------------------+--------+------------+-------+-------+----------------------------------------------------+----------+-----------|
    | M735816878-4430 | 23.92.225.232, 23.92.225.247 | On     | Off        | On    | On    | M735816878-4430-default-routed, blueprints-network |          | d2p3-ext  |

In this example, the name of the edge gateway is `M735816878-4430`


Working with Networks
---------------------

The `network` command lists the networks in the virtual data center:

    $ vca network
    
    Networks available in Virtual Data Center 'M735816878-4430':
    | Name                             | Mode      | Gateway       | Netmask       | POOL IP Range                 |
    |----------------------------------+-----------+---------------+---------------+-------------------------------|
    | M735816878-4430-default-isolated | isolated  | 192.168.99.1  | 255.255.255.0 | 192.168.99.2-192.168.99.100   |
    | M735816878-4430-default-routed   | natRouted | 192.168.108.1 | 255.255.255.0 | 192.168.108.2-192.168.108.100 |
    | blueprints-network               | natRouted | 192.168.110.1 | 255.255.255.0 | 192.168.110.2-192.168.110.100 |

To add a new network, use `vca network create` as shown in the example:

    $ vca network create --network routed-120 --gateway M735816878-4430 --gateway-ip 192.168.120.1 \
          --netmask 255.255.255.0 --dns1 192.168.120.1 --pool 192.168.120.2-192.168.120.100
          
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 12:55:46 | 0 mins 34 secs | success  |

The `--pool` option allows to define the range of IP addresses to be used for the static IP pool.

To delete an existing network, use `vca network delete`. Here is an example:

    $ vca network delete --network routed-120
    
    | Start Time          | Duration      | Status   |
    |---------------------+---------------+----------|
    | 2015-03-11 12:59:21 | 1 mins 1 secs | success  |


Edge Gateway
------------

Each virtual data center has an edge gateway with several services available. The `gateway` command provides an overview of the current configuration of the edge gateway:

    $ vca gateway
    
    Edge Gateways:
    | Name            | External IPs                 | DHCP   | Firewall   | NAT   | VPN   | Routed Networks                                    | Syslog   | Uplinks   |
    |-----------------+------------------------------+--------+------------+-------+-------+----------------------------------------------------+----------+-----------|
    | M735816878-4430 | 23.92.225.232, 23.92.225.247 | On     | Off        | On    | On    | M735816878-4430-default-routed, blueprints-network |          | d2p3-ext  |

The `info` subcommand can be used to display detailed information:

    $ vca gateway info --gateway M735816878-4430
    
    Gateway 'M735816878-4430' details:
    | Property         | Value                        |
    |------------------+------------------------------|
    | Name             | M735816878-4430              |
    | DCHP Service     | On                           |
    | Firewall Service | Off                          |
    | NAT Service      | On                           |
    | VPN Service      | On                           |
    | Syslog           |                              |
    | External IP #    | 2                            |
    | External IPs     | 23.92.225.232, 23.92.225.247 |
    | Uplinks          | d2p3-ext                     |

Public IP Addresses
-------------------

In vCloud Air On Demand, it is possible to allocate and deallocate public IP addresses. As an example, the following gateway doesn't have a public IP address allocated:

    $ vca gateway
    
    Edge Gateways:
    | Name    | External IPs   | DHCP   | Firewall   | NAT   | VPN   | Routed Networks        | Syslog   | Uplinks     |
    |---------+----------------+--------+------------+-------+-------+------------------------+----------+-------------|
    | gateway |                | Off    | On         | Off   | Off   | default-routed-network |          | d2p3v29-ext |

The `add-ip` subcommand allocates a public IP address and makes it available to the gateway:

    $ vca gateway add-ip
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-05-20 17:24:44 | 0 mins 46 secs | success  |

The gateway has now an external IP:

    $ vca gateway
    
    Edge Gateways:
    | Name    | External IPs   | DHCP   | Firewall   | NAT   | VPN   | Routed Networks        | Syslog   | Uplinks     |
    |---------+----------------+--------+------------+-------+-------+------------------------+----------+-------------|
    | gateway | 107.189.93.162 | Off    | On         | Off   | Off   | default-routed-network |          | d2p3v29-ext |

More than one IP address can be allocated to a gateway using the same subcommand.

When the public IP address is no longer needed, it can be deallocated from the gateway:

    $ vca gateway del-ip --ip 107.189.93.162
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-05-20 17:26:01 | 0 mins 19 secs | success  |

DHCP Service
------------

The edge gateway provides a DHCP service that can be operated with the `dhcp` command.

The `dhcp` command shows the status of the DCHP service:

    $ vca dhcp
    
    DHCP Service
    | Network            | IP Range From   | To              | Enabled   |   Default lease |   Max Lease |
    |--------------------+-----------------+-----------------+-----------+-----------------+-------------|
    | blueprints-network | 192.168.110.101 | 192.168.110.200 | Yes       |            3600 |        7200 |
    | routed-120         | 192.168.120.101 | 192.168.120.200 | Yes       |            3600 |        7200 |

Support for adding and removing DHCP pools will be added in a future version.


Firewall Service
----------------

The `firewall` command allows to enable and disable the firewall service on the edge gateway.

The command `vca firewall enable` enables the functionality and `vca firewall disable` disables it.

Support for adding and removing firewall rules will be added in a future version.


NAT Service
-----------

The `nat` command allows to define DNAT and SNAT rules in the NAT service on the edge gateway.

The command `vca nat` lists the rules currently defined.

    $ vca nat
    
    NAT rules
    |   Rule Id | Enabled   | Type   | Original IP      | Original Port   | Translated IP   | Translated Port   | Protocol   | Applied On   |
    |-----------+-----------+--------+------------------+-----------------+-----------------+-------------------+------------+--------------|
    |     65538 | True      | SNAT   | 192.168.110.122  | any             | 23.92.225.247   | any               | any        | d2p3-ext     |
    |     65539 | True      | DNAT   | 23.92.225.247    | any             | 192.168.110.122 | any               | any        | d2p3-ext     |

Individual NAT rules can be added or removed as shown in the examples below. 

The following example shows how to configure a SNAT rule to map all the source internal IP addresses to the external IP address:

    $ vca nat add --type SNAT --original-ip 192.168.110.0/24 --original-port any --translated-ip 23.92.225.247 --translated-port any --protocol any
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 13:49:49 | 0 mins 43 secs | success  |

This example configures a DNAT rule to allow ssh access to an internal VM:

    $ vca nat add --type DNAT --original-ip 23.92.225.247 --original-port 22 --translated-ip 192.168.110.2 --translated-port 22 --protocol tcp
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 13:54:49 | 0 mins 23 secs | success  |

It is possible to delete all NAT rules in one command:

    $ vca nat delete --all
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 13:46:19 | 0 mins 54 secs | success  |
    
    
    $ vca nat
    
    No NAT rules found in this gateway

The `nat` command allows to provide a file with the all the NAT rules defined in YAML syntax. The rules will be configued on the NAT service in one call:

    $ vca nat add --file nat-rules.yaml

Below is a sample YAML file that can be used as a template to specify NAT rules:

    
    - NAT_rules:
    
        - type: SNAT
          original_ip: 192.168.109.0/24
          original_port: any
          translated_ip: 107.189.93.162
          translated_port: any
          protocol: any
          
        - type: DNAT
          original_ip: 107.189.93.162
          original_port: 22
          translated_ip: 192.168.109.2
          translated_port: 22
          protocol: tcp
          
        - type: DNAT
          original_ip: 107.189.93.162
          original_port: 80
          translated_ip: 192.168.109.2
          translated_port: 80
          protocol: tcp
          
        - type: DNAT
          original_ip: 107.189.93.162
          original_port: 322
          translated_ip: 192.168.109.3
          translated_port: 22
          protocol: tcp
          
        - type: DNAT
          original_ip: 107.189.93.162
          original_port: 422
          translated_ip: 192.168.109.4
          translated_port: 22
          protocol: tcp

VPN Service
-----------

The edge gateway also includes a VPN service that allows IPsec VPN tunnels between data centers. The `vpn` command provides access to this service. `vpn` without arguments provides an overview of the current configuration of the VPN service:

    $ vca vpn
    
    VPN Service
    | Gateway         | Enabled   |
    |-----------------+-----------|
    | M735816878-4430 | On        |
    VPN Endpoints
    | EndPoint   | Public IP     |
    |------------+---------------|
    | d2p3-ext   | 23.92.225.247 |
    VPN Tunnels
    | Tunnel   | Local IP      | Local Networks     | Peer IP        | Peer Networks    | Enabled   | Operational   |
    |----------+---------------+--------------------+----------------+------------------+-----------+---------------|
    | vpn1     | 23.92.225.247 | blueprints-network | 192.240.158.81 | 192.168.250.0/24 | Yes       | Yes           |

The first operation to enable the VPN service is to define a local endpoint using the name of the uplink network and one of the public IP addresses. See the `vca gateway` command to obtain this information. 

Here is an example of configuring a VPN endpoint:

    $ vca vpn add-endpoint --network d2p3-ext --public-ip 23.92.225.247
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 14:29:18 | 0 mins 43 secs | success  |

Let's assume we want to setup a VPN tunnel between two virtual data centers on vCloud Air: Site-A and Site-B. Both sites need to setup a VPN endpoint as shown above.

On Site-A we create a VPN tunnel with the following command:

    $ vca vpn add-tunnel --tunnel vpn1 --local-ip 23.92.225.247 --local-network blueprints-network \
          --peer-ip 107.189.77.247 --peer-network 192.168.115.0/24 --secret 123456789012345678901234567890Ab
          
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 15:09:18 | 0 mins 43 secs | success  |    

The tunnel should be enabled but not yet operational:

    $ vca vpn
    
    VPN Service
    | Gateway         | Enabled   |
    |-----------------+-----------|
    | M735816878-4430 | On        |
    VPN Endpoints
    | EndPoint   | Public IP     |
    |------------+---------------|
    | d2p3-ext   | 23.92.225.247 |
    VPN Tunnels
    | Tunnel   | Local IP      | Local Networks     | Peer IP        | Peer Networks    | Enabled   | Operational   |
    |----------+---------------+--------------------+----------------+------------------+-----------+---------------|
    | vpn1     | 23.92.225.247 | blueprints-network | 107.189.77.247 | 192.168.115.0/24 | Yes       | No            |

The shared secret must be a string between 32 and 128 characters long that must include digits, upper and lower case letters. It has to be the same on both ends.

On Site-B we create a VPN tunnel with the parameters shown below:

    $ vca vpn add-tunnel --tunnel vpn1 --local-ip 107.189.77.247 --local-network routed-116 \
          --peer-ip 23.92.225.247 --peer-network 192.168.110.0/24 --secret 123456789012345678901234567890Ab
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 15:16:07 | 0 mins 21 secs | success  |

After a few minutes, the tunnel will be operational and ready to use:

    $ vca vpn
    
    VPN Service
    | Gateway   | Enabled   |
    |-----------+-----------|
    | Score     | On        |
    VPN Endpoints
    | EndPoint   | Public IP      |
    |------------+----------------|
    | d4p14-ext  | 107.189.77.247 |
    VPN Tunnels
    | Tunnel   | Local IP       | Local Networks         | Peer IP        | Peer Networks    | Enabled   | Operational   |
    |----------+----------------+------------------------+----------------+------------------+-----------+---------------|
    | vpn1     | 107.189.77.247 | routed-115             | 23.92.225.247  | 192.168.110.0/24 | Yes       | Yes           |

A convenient feature of **vca-cli** is the ability to add (or delete) a network to an existing tunnel, without having to delete and re-create the entire tunnel. Here is an example:

    $ vca vpn add-network --tunnel vpn1 --local-network routed-116

And on the peer side:

    $ vca vpn add-network --tunnel vpn1 --peer-network 192.168.116.0/24

The result, as shown on Site-A:

    $ vca vpn
    
    VPN Service
    | Gateway   | Enabled   |
    |-----------+-----------|
    | Score     | On        |
    VPN Endpoints
    | EndPoint   | Public IP      |
    |------------+----------------|
    | d4p14-ext  | 107.189.77.247 |
    VPN Tunnels
    | Tunnel   | Local IP       | Local Networks         | Peer IP        | Peer Networks    | Enabled   | Operational   |
    |----------+----------------+------------------------+----------------+------------------+-----------+---------------|
    | vpn1     | 107.189.77.247 | routed-115, routed-116 | 23.92.225.247  | 192.168.110.0/24 | Yes       | Yes           |    

Endpoints, tunnels and networks can be deleted, as shown in these examples:

    $ vca vpn del-network --tunnel vpn1 --local-network routed-116
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 14:20:22 | 0 mins 25 secs | success  |
    
    $ vca vpn del-tunnel --tunnel vpn1
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 14:22:22 | 0 mins 46 secs | success  |
    
    $ vca vpn del-endpoint --network d2p3-ext --public-ip 23.92.225.247
    
    | Start Time          | Duration       | Status   |
    |---------------------+----------------+----------|
    | 2015-03-11 14:26:19 | 0 mins 43 secs | success  |

Monitoring Edge Gateway Logs
----------------------------

The edge gateway can be configured to send log events to a server for analysis, with a simple option of the `gateway` command. Here is an example:

    $ vca gateway set-syslog --gateway gateway --ip 192.168.109.2
    
    | Start Time          | Duration      | Status   |
    |---------------------+---------------+----------|
    | 2015-02-12 21:58:38 | 0 mins 8 secs | success  |
    
    $ vca gateway
    
    Edge Gateways:
    | Name    | External IPs       | DHCP Service   | Firewall Service   | NAT Service   | Internal Networks          | Syslog        |
    |---------+--------------------+----------------+--------------------+---------------+----------------------------+---------------|
    | gateway | ['107.189.93.162'] | On             | On                 | On            | ['default-routed-network'] | 192.168.109.2 |

The following blog entry contains additional information about capturing [edge gateway logs](http://blog.pacogomez.com/monitoring-the-firewall-at-vcloud-air/)