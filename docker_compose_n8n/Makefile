DIRECTORY=n8n_data

up_n8n:
	@make check_directory --no-print-directory
	@docker compose pull

check_directory:
	@printf "Verificando dependencias\n"
	@sleep 1
	@if [ ! -d $(DIRECTORY) ]; then\
		printf "Um novo o diretorio nome $(DIRECTORY) será criado para persistencia de dados\n"; \
		mkdir -p $(DIRECTORY);\
		sleep 1;\
		printf "Concedendo as permissoes de escrita para o container.\n"; \
		sudo chown -R 1000:1000 ./$(DIRECTORY) ;\
		sleep 1;\
		printf "Novo volume \"$(shell pwd)/$(DIRECTORY)\" criado com sucesso.\n";\
	else \
		printf "Volume criado em $(shell pwd)/$(DIRECTORY) \n";\
	fi

stop_container:
	@docker compose stop

remove_container:
	@docker compose down

up_container:
	@docker compose up -d

pull_image:
	docker compose pull