# Ansible Playbook: Provision AWS

Provisions AWS RedHat Automation specialist 3Tier Application environment.

## Requirements

No special requirements are need for playbook and roles, however some pre-configuration is required on the jumpbox host.
  - Downloading of the required scripts and binaries supplied by RedHat.
  - Creation of the credential.rc file which contains your RedHat OpenTLC credentials.

## Role Variables 

Available variables are listed below, along with set values (see `roles/openstack_instance/vars/{application[1,2].yml,database.yml,frontend.yml`):

    catalog_name: OPENTLC Automation

The name of the RedHat OpenTLC catalog to locate the deployment item in. 

    item_name: Three Tier Application

RedHat OpenTLC item to deploy, this deploys the 3Tier instances to AWS.

## Dependencies

  - None

## Example Playbook

The required playbook has been created to call the OpenTLC provisioning process to install the various instances required by the 3 Tier Application in AWS, the creation playbook is included here for reference:

	---
	## RedHat Ansible Boot Camp homework assignment
	## Playbook have been written as a PoC for a 3Tier Application deployment
	## All playbook have been written via Ansible Core and used in Ansible Tower

	## Created a written by Paul Greenbank - Technical Consultant for Open System Specialists

	## Playbook description:
	#
	# Uses supplied RedHat scripts and instructions to deploy 3Tier Application instances into
	# AWS using RedHat's OpenTLC deployment system.
	#

	## Playbook enhancements post PoC:
	#
	# Issues encountered with this were where the order_svc.sh script called another binary which wasn't located
	# in the path of the running ansible user. I over came this by adding the path to the users shell so it was run
	# correctly.
	# This would have been better achieved using the `environment` argument with the shell command.
	#

	# Playbook to create AWS instances, basic playbook to call build script

	- hosts: jumpbox
	  gather_facts: false
	  vars:
		catalog_name: OPENTLC Automation
		item_name: Three Tier Application

	# Create AWS instances
	  tasks:
	  - name: Create 3Tier application AWS instances via supplied scripts
		shell: source ./credential.rc ; ./order_svc.sh -c '{{ catalog_name }}' -i '{{ item_name }}' -t 1 -y
		register: build_log
		ignore_errors: yes

	  - name: Debug AWS Build
		debug:
		  var: build_log.stdout


## License

MIT / BSD

## Author Information

This role was created in 2018 by Paul Greenbank - Technical Consultant (https://www.oss.co.nz/).
