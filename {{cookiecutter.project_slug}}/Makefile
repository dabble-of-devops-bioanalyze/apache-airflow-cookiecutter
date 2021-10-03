mkfile_path := $(abspath $(lastword $(MAKEFILE_LIST)))
current_dir := $(notdir $(patsubst %/,%,$(dir $(mkfile_path))))
AIRFLOW_PORT := 8080
DOCKER_REGISTRY := docker.io
DOCKER_USERNAME := docker_username
DOCKER_REPO = := {{cookiecutter.project_slug}}

# Put it first so that "make" without argument is like "make help".

.PHONY : help
help : Makefile
	@sed -n 's/^##//p' $<

# Do not do this in production!
clean-all:
	docker container stop $(shell docker container ls -aq)
	docker system prune -f -a
	docker system prune -f -a --volumes

## dev/build: development docker images
dev/build:
	docker-compose build

## dev/stop: stop the stack
dev/stop:
	docker-compose stop

## dev/dirs: make helpful directories
dev/dirs:
	@echo "Creating env dirs"
	mkdir -p .envs/.local
	mkdir -p .envs/.prod
	@echo "Creating Documentation Directories"
	mkdir -p docs
	@echo "Creating Airflow Directories"
	mkdir -p airflow/dags
	touch airflow/plugins/__init__.py
	mkdir -p airflow/logs

dev/wait:
	docker-compose  exec airflow-postgresql \
		bash -c \
		"while ! pg_isready --host airflow-postgresql --dbname=bitnami_airflow --username=bn_airflow; do sleep 5; done"
	docker-compose exec airflow \
		bash -c \
		"wait-for-it --service 0.0.0.0:$(AIRFLOW_PORT)"

## dev/up: bring up the dev stack
dev/up:
	$(MAKE) dev/dirs
	docker-compose up -d  --remove-orphans
	@echo "Waiting for airflow db"
	$(MAKE) wait
	# $(shell sleep 60)
	$(MAKE) ports
	$(MAKE) logs


## dev/ports: get the ports
dev/ports:
	@echo "Airflow Web Port"
	docker-compose port airflow 8080
	@echo "Adminer Web Port"
	docker-compose port adminer 8080

## dev/example/create-conn: example command to create a connection
dev/example/create-conn:
	$(MAKE) dev/wait
	docker-compose exec airflow \
	bash -c "airflow connections add 'aws_conn_name' \
		--conn-type 'aws' --conn-login 'ABCDEFGHIJK' \
		--conn-password 'myusecretaccesskey' \
		--conn-extra '{"region_name": "us-east-1"}' "

## dev/shell: grab a bash shell in the airflow container
dev/shell:
	docker-compose  exec airflow bash

## dev/clean: clean up volumes. if you get stuck try cleaning and restarting
dev/clean/volumes:
	docker-compose stop
	docker-compose rm -f
	docker volume rm $(shell docker volume ls -q | grep {{cookiecutter.project_slug}}) || echo "No volumes found"

## dev/restart: restart
dev/restart:
	docker-compose stop airflow
	$(MAKE) up

## dev/build: build the dev stack
dev/build:
	docker-compose  build

## dev/restart/clean: clean
dev/restart/clean:
	$(MAKE) dev/stop
	$(MAKE) dev/build
	$(MAKE) dev/up
	$(MAKE) dev/logs

## dev/logs: get the airflow logs.
dev/logs:
	docker-compose logs -f airflow

## dev/logs: get the airflow application routes. this includes any routes declared in plugins.
dev/routes:
	docker-compose exec -T airflow  \
		bash -c "FLASK_APP=airflow.www.app:create_app flask routes"

## dev/jupyter/token: grab the jupyter token
dev/jupyter/token:
	docker-compose logs jupyter |grep token |tail

## dev/jupyter/restart: restart the jupyter notebook
dev/jupyter/restart:
	docker-compose stop jupyter
	docker-compose rm -f -v jupyter
	docker-compose up -d jupyter

## prod/build: build the prod docker-compose stack
prod/build:
	@echo "This is a default build process. If you haven't already please check this over!"
	docker-compose -f docker-compose.prod.yml build

## prod/push: ALERT! You should look at this and modify it for you own use! Tags all the images in the stack as 'latest' and push them to $(DOCKER_REGISTRY)/$(DOCKER_USERNAME)/$(DOCKER_REPO)-{name}:latest
prod/push:
	@echo "Pushing docker images"

	$(MAKE) prod/build

	# Make sure to check the tags and make sure these tags match what you want!
	docker tag $(current_dir)_airflow $(DOCKER_REGISTRY)/$(DOCKER_USERNAME)/$(DOCKER_REPO)-airflow:latest
	docker tag $(current_dir)_airflow-worker $(DOCKER_REGISTRY)/$(DOCKER_USERNAME)/$(DOCKER_REPO)-airflow-worker:latest
	docker tag $(current_dir)_airflow-scheduler $(DOCKER_REGISTRY)/$(DOCKER_USERNAME)/$(DOCKER_REPO)-airflow-scheduler:latest

date:
	DATE=$(shell date +%Y%m%d)
	@echo $(DATE)