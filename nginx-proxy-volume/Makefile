
NS = busboom.org
VERSION = latest

REPO = nginx-proxy-volume-container
NAME = nginx-proxy-volume-container
INSTANCE = default
DOCKER ?= docker

# NOTE! Docker-compose gets the project name from the directory the compose file is in, 
# or the -p option, or the COMPOSE_PROJECT_NAME environmental variable. 
PROJECT_NAME=secureproxy

VOLUMES= \
-v $(PROJECT_NAME)_vol-conf:/etc/nginx/conf.d \
-v $(PROJECT_NAME)_vol-vhost:/etc/nginx/vhost.d \
-v $(PROJECT_NAME)_vol-html:/usr/share/nginx/html \
-v $(PROJECT_NAME)_vol-certs:/etc/nginx/certs \
-v $(PROJECT_NAME)_vol-tmpl:/etc/docker-gen/templates/

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