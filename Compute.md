vCloud Air Network Management with vca-cli
------------------------------------------

This section describes the compute service operations available through **vca-cli**


Compute Commands
----------------

The following is a list of compute service related commands:

    vapp      Operations with vApps
    vm        Operations with VMs


Operating with vApps
--------------------

A vApp can be created by specifying an existing template. Here is an example:

    vca vapp create --vapp myvapp --vm myvm \
        --catalog 'Public Catalog' --template 'Ubuntu Server 12.04 LTS (amd64 20150127)' \
        --network default-routed-network --mode POOL

It is possible to create more than one instances from a template by using the 'count' parameter. The following example creates 10 virtual machines from a template:

    vca vapp create --vapp myvapp --vm myvm \
        --catalog 'Public Catalog' --template 'Ubuntu Server 12.04 LTS (amd64 20150127)' \
        --network default-routed-network --mode POOL \
        --count 10
