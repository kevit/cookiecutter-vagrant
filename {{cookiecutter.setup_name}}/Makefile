.DEFAULT_GOAL := help


current-dir = $(shell pwd)
ansible = $(shell command -v ansible)
ansible-playbook = $(shell command -v ansible-playbook)
ansible-flags = -e ansible_python_interpreter=/usr/bin/python3
playbook = provisioning/playbook.yml

#export ANSIBLE_INVENTORY
export ANSIBLE_CONFIG=$(current-dir)/machine_inventory.yml


requirements: ## Install requirements from scratch
	ansible-galaxy install -r requirements.yml -p roles --ignore-errors --force


ping: ## ping hosts
	$(ansible) all $(ansible-flags) -m ping

run-playbook: ## run playbook
	$(ansible-playbook) $(playbook) $(ansible-flags)

list-host: ## list hosts
	$(ansible-playbook) $(playbook) $(ansible-flags) --list-hosts

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'
