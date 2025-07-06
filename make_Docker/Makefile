install_docker:
	@echo "\n\nA instalação do Docker requer permissões sudo. Sua autenticação podera ser solicitada algumas vezes."
	@sleep 1
	@make .apt_update_upgrade
	@make .check_install_docker --no-print-directory
	@echo "\n\n\e[31m           Reinicie o terminal para aplicar as atualizações.\e[0m\n\n\n"

.apt_update_upgrade:	
	@sudo apt update && sudo apt upgrade -y

.check_install_docker:
	@if ! command -v docker >/dev/null 2>&1; then \
		make .check_dependencies; \
		make .create_key; \
		make .add_rep; \
		make .install; \
		make .active_service; \
		make .add_user_to_docker_group;\
	else \
		echo "\nDocker já está instalado."; \
	fi

.check_dependencies:
	@echo "\n\nInstalando dependências\n\n"
	@sleep 1
	@sudo apt install -y apt-transport-https ca-certificates curl software-properties-common lsb-release gnupg

.create_key:
	@echo "\nCriando estrutura de diretórios e importando chave GPG\n"
	@sleep 1
	@sudo mkdir -p /etc/apt/keyrings
	@curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

.add_rep:
	@echo "\nAdicionando repositório Docker\n"
	@echo "deb [arch=$$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $$(lsb_release -cs) stable" | \
		sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	@sudo apt update

.install:
	@echo "\nInstalando Docker\n"
	@sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

.active_service:
	@echo "\nAtivando serviço Docker (via systemd)\n"
	@sudo systemctl enable docker
	@sudo systemctl start docker

.add_user_to_docker_group:
	@echo "\nAdicionando usuário '$$(whoami)' ao grupo docker...\n"
	@sudo usermod -aG docker $$(whoami)
	@echo "\nInstalação Docker concluida.\n"

remove_docker:
	@echo "\nRemovendo Docker e pacotes relacionados\n"
	@sleep 1
	@sudo systemctl stop docker.socket 2> /dev/null
	@sudo systemctl disable docker.socket 2> /dev/null
	@sudo apt remove --purge -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
	@sudo apt autoremove -y --purge
	@echo "\nRemovendo arquivos residuais\n"
	@sudo rm -rf /var/lib/docker /etc/docker /var/run/docker.sock ~/.docker
	@echo "\nRemovendo repositório Docker\n"
	@sudo rm -f /etc/apt/sources.list.d/docker.list
	@sudo rm -f /etc/apt/keyrings/docker.gpg
	@sudo getent group docker > /dev/null && sudo groupdel docker
	@sudo apt update
	@echo "\nDocker removido com sucesso!"
	@echo "\n\n\n\e[31m              Reinicie o terminal para aplicar as alterações.\e[0m\n\n\n"


.PHONY: docker_install remove_docker