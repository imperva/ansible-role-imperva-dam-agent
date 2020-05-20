## Overview
This project can provide the Imperva playbook for an existing ansible installation, or it can be used standalone to deploy the Imperva agent even if there is not an existing ansible infrastructure.

For existing ansible installations, provide the ansible team with this project, the gateway IP and registration password, and any other settings that are not default.  They will also need the which_ragent script from the Imperva ftp as well as the agent tar.gz files for the platforms to be installed.  (this playbook can be used by the ansible team to find out what agent files are needed)

If you don't have an existing ansible infrastructure, use the setup instructions below.  This will install a vagrant environment, an ansible environment, and a virtual environment to test everything.  Once you are able to register the virtual machine to a gateway, you can use the ansible command to push the agent out to other systems.  If you want to test on existing machine VMs, you could skip the Vagrant install, but it is recommended because you should end up with a working environment.

## Setup
1. Install python 3.6+, Vagrant, and your vagrant provider of choice (by default vagrant will use the default provider of Virtualbox if it is present)
2. Configure python virtualenv (if you don't have a PATH_TO_VIRTUALENV and/or you don't have strong feelings on what it should be, you can add a pythonlocal directory in your home dir and path to it - it just keeps you from adding all the python modules to the general machine installation).  (if you have problems with Vagrant finding it later after you logged off, you may need to rerun "source $PATH_TO_VIRTUALENV/bin/activate")
```
python3 -m venv $PATH_TO_VIRTUALENV
source $PATH_TO_VIRTUALENV/bin/activate
```
3. Install python modules
```
pip install -r ./extension/setup/python_requirements.txt
```
4. Install ansible external deps
```
ansible-galaxy collection install -r requirements.yml
```
5. Pull up your vagrant environment.  This command will create a virtual machine, and will check which agent it needs, prompt you for the binaries, install the agent, register it, and start it.
```
vagrant up
```

6. If things didn't come up perfectly the first time, you can try 
```
vagrant destroy
```
to tear down the test environment and start over.  If the VM came up, but the ansible job failed because you were missing files, you can just run 
```
vagrant provision
```
and it will rerun the ansible job without rebuilding the VM.

## Deployment
Once you have the Setup steps done, you will have an environment that deploys the agent to the virtual macine brough up by Vagrant. 

To push out to production systems, you can use the following ansible command from the /v2 directory:
```
ansible-playbook --connection=ssh --timeout=30 --limit="db-server-1" --inventory-file=.vagrant/provisioners/ansible/inventory --extra-vars=\{\"gateway_ip\":\"1.2.3.4\",\"gateway_pass\":\"Webco123\"\} -v v2/site.yml
```
You will need to create a list of systems to push to in the inventory-file (see .vagrant/provisioners/ansible/inventory for an example).  You can then use the "limit" feature to restrict the installation to particular systems on the inventory list.

(to see what command Vagrant is actually running, you can add the 
```
ansible.verbose = "v"
```
to the Vagrantfile underneath ansible.playbook.  This is especially useful if we changed something and didn't update this part of the readme.
