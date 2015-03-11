vCloud Air Network Management with **vca-cli**
----------------------------------------------

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

The `org` and `vdc` commands provide an overview of the network configuration in the virtual data center:

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