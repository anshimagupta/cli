vCloud Air Compute Management with vca-cli
------------------------------------------

This section describes the compute service operations available through **vca-cli**


Compute Commands
----------------

The following is a list of compute service related commands:

    vapp      Operations with vApps
    vm        Operations with VMs


vApp Operations
---------------

A vApp can be created by specifying an existing template. Here is an example:

    vca vapp create --vapp myvapp --vm myvm \
        --catalog 'Public Catalog' --template 'Ubuntu Server 12.04 LTS (amd64 20150127)' \
        --network default-routed-network --mode pool

It is possible to create more than one instances from a template by using the `count` parameter. The following example creates 10 virtual machines from a template:

    vca vapp create --vapp myvapp --vm myvm \
        --catalog 'Public Catalog' --template 'Ubuntu Server 12.04 LTS (amd64 20150127)' \
        --network default-routed-network --mode pool \
        --count 10

The new VM instance can be customize as part of the `create` command by specifying the number of virtual CPUs and size of the memory (in MB). Here is an example of creating a virtual machine with 4 virtual CPUs and 8 Gigs of RAM:

    vca vapp create --vapp myvapp --vm myvm \
        --catalog 'Public Catalog' --template 'Ubuntu Server 12.04 LTS (amd64 20150127)' \
        --network default-routed-network --mode pool \
        --cpu 4 --ram 8192
