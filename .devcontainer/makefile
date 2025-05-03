# ================================
# Configuración general
# ================================
IMAGE_NAME := dev_python_r
TAG := latest
DOCKERFILE := Dockerfile
CONTEXT := .

USER := devuser
CONTAINER_NAME := dev_container
PROJECT_DIR := $(shell pwd)
WORKSPACE := /workspace

# Volúmenes adicionales (modifica según tus necesidades)
VOLUMES := \
	-v $(PROJECT_DIR):$(WORKSPACE) \
	# -v ~/.ssh:/root/.ssh:ro \
	# -v /etc/timezone:/etc/timezone:ro \
	# -v /etc/localtime:/etc/localtime:ro

# ================================
# Tareas Docker directas
# ================================

.PHONY: build
build:
	docker build -t $(IMAGE_NAME):$(TAG) -f $(DOCKERFILE) $(CONTEXT)

.PHONY: run
run:
	docker run -dit \
		--name $(CONTAINER_NAME) \
		--entrypoint bash \
		$(VOLUMES) \
		-w $(WORKSPACE) \
		$(IMAGE_NAME):$(TAG)

	docker exec -u root $(CONTAINER_NAME) chown -R $(USER):$(USER) $(WORKSPACE)

.PHONY: attach
attach:
	code --remote-container $(CONTAINER_NAME)

.PHONY: clean
clean:
	-docker rm -f $(CONTAINER_NAME)

.PHONY: rebuild
rebuild: clean build run

# ================================
# docker-compose (si tienes docker-compose.yml)
# ================================

.PHONY: compose-up
compose-up:
	docker-compose up -d

.PHONY: compose-down
compose-down:
	docker-compose down

.PHONY: compose-rebuild
compose-rebuild:
	docker-compose down --remove-orphans
	docker-compose build
	docker-compose up -d
