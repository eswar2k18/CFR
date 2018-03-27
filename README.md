# AnsibleInsideDocker
Ansible inside a Docker and configure other containers

Docker image build commands:

docker build -t apache-php --no-cache=true ./apache
docker build -t nginx --no-cache=true ./nginx
docker build -t ansible --no-cache=true ./ansible

Commands to build container:

docker run -d --name testapachephp2 -p 80:80 -v ~/.ssh/id_rsa.pub:/root/.ssh/authorized_keys apache-php /bin/bash
docker run -d --name testnginx -p 8080:80 -v ~/.ssh/id_rsa.pub:/root/.ssh/authorized_keys nginx
docker run --privileged -d \
    --link testnginx \
    --volume= ~/.ssh/id_rsa:/root/.ssh/id_rsa \
    --volume= ~/.ssh/id_rsa.pub:/root/.ssh/id_rsa.pub \
    --volume=$(pwd):/ansible/playbooks \
	--name ansible-test 
    
[ Before Creating the containers, create ssh key in host machine and use that key in ansible. Use the same as authorized key in other containers ]

After launch of containers, get inside ansible container and add the 'testnginx' in /etc/ansible/hosts.
To enter inside container, docker exec -i -t ansible-test bash


