Role Name
=========

The Imperva role can be used to install and manage Imperva DAM agents, usually on database servers.  The role will execute the Imperva provided which_ragent script, determine which agent to install and look for that tar.gz file in the local files directory.  It will unpack that tar.gz on the destination, run the installers, register them to the provided gateway, and start the services.

Requirements
------------

You will need to have an active Imperva support contract to to download the which_ragent script and the required agents.

Role Variables
--------------
gateway_ip and gateway_pass will need to be provided to register the agents.  'install' => true is necessary to kick off the install process.

Other variables are available in roles/imperva/defaults/main.yml, but defaults there should work for most people.

The major version number to feed to the which_ragent script is set in v2/roles/imperva/vars/main.yml


Dependencies
------------

Example Playbook
----------------

License
-------

Imperva Community License

Author Information
------------------

Imperva Global Solutions Architecture team
