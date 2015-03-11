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

    vca network
    Networks available in Virtual Data Center 'M735816878-4430':
    | Name                             | Mode      | Gateway       | Netmask       | POOL IP Range                 |
    |----------------------------------+-----------+---------------+---------------+-------------------------------|
    | M735816878-4430-default-isolated | isolated  | 192.168.99.1  | 255.255.255.0 | 192.168.99.2-192.168.99.100   |
    | M735816878-4430-default-routed   | natRouted | 192.168.108.1 | 255.255.255.0 | 192.168.108.2-192.168.108.100 |
    | blueprints-network               | natRouted | 192.168.110.1 | 255.255.255.0 | 192.168.110.2-192.168.110.100 |

To add a new network, use `vca network add` as shown in the example:

    $ vca network add --network routed-120 --gateway M735816878-4430 --gateway_ip 192.168.120.1 \
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

