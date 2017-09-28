
NS = busboom.org
VERSION = latest

REPO = nginx-proxy-volume-container
NAME = nginx-proxy-volume-container
INSTANCE = default
DOCKER ?= docker

VOLUMES= \
--mount source=secureproxy_vol-conf,destination=/etc/nginx/conf.d \
--mount source=secureproxy_vol-vhost,destination=/etc/nginx/vhost.d \
--mount source=secureproxy_vol-html,destination=/usr/share/nginx/html \
--mount source=secureproxy_vol-certs,destination=/etc/nginx/certs \
--mount source=secureproxy_vol-tmpl,destination=/etc/docker-gen/templates/

.PHONY: build push shell run start stop restart reload rm rmf release

build:
	$(DOCKER) build -t $(NS)/$(REPO):$(VERSION) .

push:
	$(DOCKER) push $(NS)/$(REPO):$(VERSION)

shell:
	$(DOCKER) run --rm -i -t $(PORTS) $(VOLUMES) $(ENV) $(NS)/$(REPO):$(VERSION) 

attach:
	$(DOCKER) exec  -ti $(NAME) /bin/bash

run:
	$(DOCKER) run --rm --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(NS)/$(REPO):$(VERSION)

logs:
	$(DOCKER) logs -f $(NAME) 

start:
	$(DOCKER) run -d --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(NS)/$(REPO):$(VERSION)

stop:
	$(DOCKER) stop $(NAME)
	
restart: stop start

reload: build rmf start

rmf:
	$(DOCKER) rm -f $(NAME)

rm:
	$(DOCKER) rm $(NAME)

release: build
	make push -e VERSION=$(VERSION)

default: build