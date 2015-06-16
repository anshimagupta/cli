vCloud Air Blueprint Management with vca-cli
--------------------------------------------

This section describes the blueprint service operations available through **vca-cli**

###Manage Blueprints
--------------------------------------------

	vca bp --help

	Usage: vca bp [OPTIONS] [list | info | upload | delete]

  	Operations with Blueprints

	Options:
  	-b, --blueprint <blueprint_id>  Name of the blueprint to create
  	-f, --file <blueprint_file>     Local file name of the blueprint (TOSCA
                                  YAML)
  	-h, --help                      Show this message and exit.

####List

Command for listing all uploaded blueprints:

	$ vca bp list

	no blueprints found

#### Upload

A blueprint can be uploaded by specifying the location of the blueprint. 

    $ vca bp upload --blueprint hellow --file ~/vca/tosca-blueprints/helloworld/blueprint.yaml

    successfully uploaded blueprint '95e04439-7dcc-42e3-90ec-36ef2e4b0757_hellow'

#### Delete

A blueprint can be deleted as follow:

	$ vca bp delete --blueprint hellow

	successfully deleted blueprint 'hellow'

### Manage Deployments
--------------------------------------------

	$ vca dep --help

	Usage: vca dep [OPTIONS] [list | info | create | delete | execute | cancel]

  	Operations with Deployments

	Options:
  	-w, --workflow <workflow_id>    Workflow Id
  	-d, --deployment <deployment_id>
                                  Deployment Id
  	-b, --blueprint <blueprint_id>  Blueprint Id
  	-f, --file <input_file>         Local file with the input values for the
                                  deployment (YAML)
  	-s, --show-events               Show events
  	-h, --help                      Show this message and exit.

#### Create Deployment

To create a deployment for a blueprint, use the command shown in the example:

	$ vca dep create --blueprint hellow --deployment hellowdep --file ~/vca/tosca-blueprints/helloworld/my-input-values.yaml

	successfully created deployment 'hellowdep'

To list all the deployments, use `list` command
	
	$ vca dep list

	deployments:
	| Blueprint Id   | Deployment Id   | Created             |
	|----------------+-----------------+---------------------|
	| hellowdep      | hellowdep       | 2015-06-15 18:21:11 |

The `info` subcommand can be used to display detailed information:

	$ vca dep info --deployment hellowdep

	Deployment information:
	| Blueprint Id   | Deployment Id   | Created             | Workflows                                          |
	|----------------+-----------------+---------------------+----------------------------------------------------|
	| hellowdep      | hellowdep       | 2015-06-15 18:21:11 | execute_operation, scale, heal, install, uninstall |
	Workflow executions for deployment 'hellowdep'
	| Workflow                      | Created             | Status     | Id                                   |
	|-------------------------------+---------------------+------------+--------------------------------------|
	| create_deployment_environment | 2015-06-15 18:21:11 | started | cbf5a25c-0fd8-45ac-aa0a-53e3a1ac0f56 |

Install the workflow, as per the example shown below:

	$ vca dep execute --deployment hellowdep --workflow install --show-events

	Workflow execution 'f4265ee5-2988-4ee7-91d9-42129c84995a' for deployment '95e04439-7dcc-42e3-90ec-36ef2e4b0757_hellowdep'
	| Workflow   | Created             | Status   | Message   |
	|------------+---------------------+----------+-----------|
	| install    | 2015-06-15 18:40:22 | pending  |           |

	$ vca dep info --deployment hellowdep

	Deployment information:
	| Blueprint Id   | Deployment Id   | Created             | Workflows                                          |
	|----------------+-----------------+---------------------+----------------------------------------------------|
	| hellowdep      | hellowdep       | 2015-06-15 18:21:11 | execute_operation, scale, heal, install, uninstall |
	Workflow executions for deployment 'hellowdep'
	| Workflow                      | Created             | Status     | Id                                   |
	|-------------------------------+---------------------+------------+--------------------------------------|
	| create_deployment_environment | 2015-06-15 18:21:11 | terminated | cbf5a25c-0fd8-45ac-aa0a-53e3a1ac0f56 |
	| install                       | 2015-06-15 20:18:23 | started    | ad4a19e4-d517-4dcd-8472-75606e557098 |


	$ vca vm &&  vca nat

	Available VMs in 'Anshima VDC' for 'default' profile:
	| VM     | vApp   | Status     | IPs           | Networks               |   vCPUs |   Memory (GB) | CD/DVD   | OS                    | Owner               |
	|--------+--------+------------+---------------+------------------------+---------+---------------+----------+-----------------------+---------------------|
	| hellow | hellow | Powered on | 192.168.109.2 | default-routed-network |       3 |             2 | []       | Ubuntu Linux (64-bit) | anshimag@vmware.com |
	NAT rules
	|   Rule Id | Enabled   | Type   | Original IP      | Original Port   | Translated IP   | Translated Port   | Protocol   | Applied On             |
	|-----------+-----------+--------+------------------+-----------------+-----------------+-------------------+------------+------------------------|
	|     65537 | True      | SNAT   | 192.168.109.0/24 | any             | 92.246.242.37   | any               | any        | d11p16v3-ext           |
	|     65538 | True      | DNAT   | 92.246.242.37    | 22              | 192.168.109.2   | 22                | tcp        | d11p16v3-ext           |
	|     65539 | True      | SNAT   | 192.168.109.1    | any             | 92.246.242.37   | any               | any        | default-routed-network |

#### Uninstall Workflow

To tear down the application, execute the `uninstall` workflow of the deployment:

	$ vca dep execute --deployment hellowdep --workflow uninstall --show-events

	| Workflow   | Created             | Status   | Message   |
	|------------+---------------------+----------+-----------|
	| uninstall  | 2015-06-15 20:26:35 | pending  |           |

#### Cleaning Up

The deployment can be deleted with the `delete` subcommand:

	$ vca dep delete --deployment hellowdep

	successfully deleted deployment 'hellowdep'
