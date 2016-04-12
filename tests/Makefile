################################################################################
#
# Directory path for storing variable data
#
role_test_dotfile_path = .role_test

#
# Configuration of Docker Machine
#
docker_machine = default
docker_machine_ip = 192.168.99.100
docker_ssh_port = 2223
docker_ssh_pubkey_path = $(role_test_dotfile_path)/sshkey.pub
docker_ssh_privkey_path = $(shell echo $(docker_ssh_pubkey_path) | sed 's/.pub//g')

#
# Pick a distro_version
#
#docker_image_distro = ubuntu_1404
#docker_image_distro = ubuntu_1510
#docker_image_distro = ubuntu_1604
#docker_image_distro = debian_jessie
#docker_image_distro = debian_wheezy
#docker_image_distro = fedora_22
#docker_image_distro = fedora_23
#docker_image_distro = centos_7
docker_image_distro = centos_6
#docker_image_distro = opensuse_13
################################################################################
docker_ssh_pubkey = $(shell cat $(docker_ssh_pubkey_path))
docker_config = $(shell docker-machine config $(docker_machine))
docker_image = ansible_test_$(docker_image_distro)
docker_container = $(docker_image)_container

all: provision

init: makedir ssh-keygen image

provision: container
	@echo \> Testing the role...
	@ansible-playbook -i inventory test.yml

ssh:
	@ssh -p $(docker_ssh_port) -i $(docker_ssh_privkey_path) -o StrictHostKeyChecking=no root@$(docker_machine_ip)

reprovision: clean-ps provision

clean: clean-all

clean-all: clean-ps
	@echo \> Removing image \'$(docker_image)\'...
	@docker $(docker_config) rmi $(docker_image)
	rm inventory

clean-ps:
	@echo \> Stopping container \'$(docker_container)\'...
	@docker $(docker_config) stop $(docker_container) > /dev/null
	@echo \> Removing container \'$(docker_container)\'...
	@docker $(docker_config) rm $(docker_container) > /dev/null

inventory:
	@echo docker-container ansible_port=$(docker_ssh_port)\
	 ansible_user=root ansible_host=$(docker_machine_ip)\
	 ansible_ssh_private_key_file=$(docker_ssh_privkey_path) > inventory

image: inventory
	@echo \> Creating image \'$(docker_image)\' if it does not exist...
	@docker $(docker_config) images |grep $(docker_image) >/dev/null ||\
	 docker $(docker_config) build -t $(docker_image) -f Dockerfile.d/$(docker_image_distro) .

container:
	@echo \> Creating container \'$(docker_image)\' if it does not exist...
	@(docker $(docker_config) ps | grep $(docker_container) > /dev/null ||\
	 docker $(docker_config) run -p $(docker_ssh_port):22 --name=$(docker_container)\
	 -e AUTHORIZED_KEYS="$(docker_ssh_pubkey)" -dt $(docker_image)) > /dev/null

makedir:
	@echo \> Ensuring "$(role_test_dotfile_path)" directory exists...
	@test -d $(role_test_dotfile_path) >/dev/null || mkdir $(role_test_dotfile_path)

ssh-keygen:
	@echo \> Ensuring ssh-key pair $(docker_ssh_privkey_path) exists...
	@test -f $(docker_ssh_privkey_path) >/dev/null || ssh-keygen -q -N "" -t rsa -f $(docker_ssh_privkey_path)
