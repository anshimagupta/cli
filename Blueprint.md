vCloud Air Blueprint Management with vca-cli
--------------------------------------------

This section describes the blueprint service operations available through **vca-cli**

List
-------------

Command for listing all uploaded blueprints

	$ vca bp list
	Blueprints:
	| Blueprint Id   | Created             |
	|----------------+---------------------|
	| hellow         | 2015-06-12 16:57:12 |

Upload Blueprint
-----------------

A blueprint can be uploaded by specifying the location of the blueprint. 

    vca bp upload --blueprint hellow --file ~/vca/tosca-blueprints/helloworld/blueprint.yaml