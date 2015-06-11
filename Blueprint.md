vCloud Air Blueprint Management with vca-cli
-------------------------------------------

This section describes the blueprint service operations available through **vca-cli**

Blueprint Commands
-------------------

The following is a list of blueprint service related commands:

Operations
-------------------

$ vca bp --help

Usage: vca bp [OPTIONS] [list | info | upload | delete]

  Operations with Blueprints

Options:
  -b, --blueprint <blueprint_id>  Name of the blueprint to create
  -f, --file <blueprint_file>     Local file name of the blueprint (TOSCA YAML)
  -h, --help                      Show this message and exit.

$ vca bp upload \
	--blueprint 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec \
 	--file ~/vca/tosca-blueprints/cloudify-nodecellar-example/blueprint.yaml

successfully uploaded blueprint '37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec'

$ vca dep --help

Usage: vca dep [OPTIONS] [list | info | create | delete | execute | cancel]

  Operations with Deployments

Options:
  -w, --workflow <workflow_id>    Workflow Id
  -d, --deployment <deployment_id>
                                  Deployment Id
  -b, --blueprint <blueprint_id>  Blueprint Id
  -f, --file <input file>         Local file with the input values for the
                                  deployment (YAML)
  -s, --show-events               Show events
  -h, --help                      Show this message and exit.

$ vca dep create --blueprint 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec \
	--deployment 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1 \
	--file ~/vca/tosca-blueprints/input.yaml

successfully created deployment '37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1'

vca dep info --deployment 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1

Deployment information:
| Blueprint Id                               | DeploymentId| Created             | Workflows                                          |
|--------------------------------------------+-------------+---------------------+----------------------------------------------------|
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec | 37d_nodec_1 | 2015-03-25 13:07:53 | execute_operation, scale, heal, install, uninstall |
Workflow executions for deployment '37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1'
| Workflow                      | Created             | Status     | Id                                   |
|-------------------------------+---------------------+------------+--------------------------------------|
| create_deployment_environment | 2015-03-25 13:07:53 | terminated | e3844450-f5e9-4e36-84ad-841ee05aa7cf |
| install                       | 2015-03-25 13:11:44 | terminated | 5e00a8ac-806d-49b2-bd5e-edb2bc00087d |

$ vca dep execute --deployment 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1 \
	--workflow install

Workflow execution '5e00a8ac-806d-49b2-bd5e-edb2bc00087d' for deployment '37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1’

| Workflow   | Created             | Status   | Message   |
|------------+---------------------+----------+-----------|
| install    | 2015-03-25 13:11:44 | pending  |           |

$ vca dep execute --deployment 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1 \
	--workflow uninstall

Workflow execution '5e00a8ac-806d-49b2-bd5e-edb2bc000acf' for deployment '37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1’

| Workflow   | Created             | Status   | Message   |
|------------+---------------------+----------+-----------|
| uninstall  | 2015-03-25 13:41:45 | pending  |           |

$ vca dep delete --deployment 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1

successfully deleted deployment '37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec_1'

$ vca bp delete --blueprint 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec

successfully deleted blueprint '37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec'

$ vca vm

Available VMs in 'VDC1' for 'default' profile:
| VM                | vApp              | Status      | IPs           | Networks   |   vCPUs | Mem | CD/DVD   | OS        |
|-------------------+-------------------+-------------+---------------+------------+---------+-----+----------+-----------|
| nodejs_host_292d3 | nodejs_host_292d3 | Powered on  | 192.168.221.2 | routed-221 |       1 |     | []       | Ubuntu    |
| mongod_host_32dc2 | mongod_host_32dc2 | Powered on  | 192.168.221.3 | routed-221 |       1 |     | []       | Ubuntu    |

$ vca nat

NAT rules
|   Rule Id | Enabled   | Type   | Original IP      | Original Port   | Translated IP   | Translated Port   | Protocol   | Applied On   |
|-----------+-----------+--------+------------------+-----------------+-----------------+-------------------+------------+--------------|
|     65537 | True      | SNAT   | 192.168.221.0/24 | any             | 107.189.92.225  | any               | any        | d2p3v29-ext  |
|     65538 | True      | SNAT   | 192.168.221.2    | any             | 107.189.93.162  | any               | any        | d2p3v29-ext  |
|     65539 | True      | DNAT   | 107.189.93.162   | Any             | 192.168.221.2   | Any               | Any        | d2p3v29-ext  |

$ open http://107.189.93.162:8080

$ vca bp

Blueprints:
| Blueprint Id                               | Created             |
|--------------------------------------------+---------------------|
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_00    | 2015-03-24 00:10:01 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_ex00a | 2015-03-19 20:20:06 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_ex01c | 2015-03-19 22:30:59 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_ex01e | 2015-03-19 22:56:07 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nc3   | 2015-03-20 02:40:26 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nc5   | 2015-03-25 05:14:01 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nc6   | 2015-03-25 06:38:36 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nc7   | 2015-03-25 07:01:44 |
| 37d9e482-a5d1-4811-b466-c4d1e2f67f2c_nodec | 2015-03-25 13:05:05 |


