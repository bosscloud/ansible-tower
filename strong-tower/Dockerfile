# Ansible Tower Dockerfie
FROM ansible-tower:79.0.1

WORKDIR /opt

#RUN pip install ansible

RUN cd /opt/ansible-tower-setup-${ANSIBLE_TOWER_VER} \
#	&& ./setup.sh \
	&& chmod +x /docker-entrypoint.sh

# volumes and ports
VOLUME ["${PG_DATA}", "${AWX_PROJECTS}", "/certs",]
EXPOSE 443

CMD ["/docker-entrypoint.sh", "ansible-tower"]


## docker build --no-cache --squash -t strong-tower:0.1 .
## docker run -d -P --name strong01 strong-tower:0.1
## docker exec -it strong01  /bin/bash
