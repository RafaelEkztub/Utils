up_n8n_api_service:
	@make .check_directory --no-print-directory
	@docker compose up -d

stop_container:
	@docker compose stop

remove_container:
	@docker compose down

up_container:
	@make .check_directory  --no-print-directory
	@docker compose up -d

pull_image:
	docker compose pull

status_service:
	docker ps -a

.DIRECTORY=n8n_data

.check_directory:
	@printf "Verificando diretorios e dependencias\n"
	@sleep 1
	@if [ ! -d $(.DIRECTORY) ]; then\
		printf "Criando diretorio $(.DIRECTORY) para persistencia de dados\n"; \
		mkdir -p $(.DIRECTORY);\
		sleep 1;\
		printf "Concedendo as permissoes de escrita para o container.\n"; \
		sudo chown -R 1000:1000 ./$(.DIRECTORY) ;\
		sleep 1;\
		printf "Novo volume \"$(shell pwd)/$(.DIRECTORY)\" criado com sucesso.\n";\
	else \
		printf "Volume criado em $(shell pwd)/$(.DIRECTORY) \n";\
	fi
