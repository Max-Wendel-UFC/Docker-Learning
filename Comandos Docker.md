# Comandos Básicos
---
- `docker images`
	- Lista todas as imagens baixadas
- `docker run [IMAGE] [COMAND]`
	- Executa a imagem, se não tiver baixada ele executa um `pull`
	- Exemplos de comandos:
		- `-i` Modo Interativo, faz com que o container possua interação
		- `-t` Modo Terminal, executa o container com um console (terminal do Ubuntu, por exemplo) 
		- `-d` Modo Background, executa o container em um processo em background
- `docker pull [REPO/IMAGE]`
	- Baixa a imagem de um repositório do Docker Hub.
- `docker push [REPO/IMAGE]`
	- Sobe a imagem para o seu repositório do Docker Hub.
- `docker search [USER/IMAGE]`
	- Pesquisa imagens no Docker Hub a partir de um usuário ou uma imagem.
- `docker ps`  
	- Lista o container que está em execução
- `docker ps -a`
	- Lista todos os containers da maquina (parados ou não)
- `docker start [CONTAINER_ID]`
	- Inicia o conteiner 
- `docker stop [CONTAINER_ID]` 
	- Pausa o container
- `docker unstop [CONTAINER_ID]`
	- Retoma o container	
- `CTRL + P + Q` 
	- Sai do container sem matar o processo
- `docker attach [CONTAINER_ID]`
	- Retorna para o container
- `docker create [IMAGEM]`
	- Cria uma imagem (mas não executa)
- `docker stats [CONTAINER_ID]`
	- Mosra os recursos (hardware) que o container está usando 
- `docker top [CONTAINER_ID]`
	- Mostra os processor que estão sendo executados no container 
- `docker logs [CONTAINER_ID]`
	- Mostra os logs do container executados em primeiro plano
- `docker rm [CONTAINER_ID]`
	- Remove um container que está parado
- `docker rm -f [CONTAINER_ID]`
	- Remove um container, mesmo que esteja rodando
- `docker rm $(docker ps -qa)`
	- Remove todas as imagens.
- `docker rmi [IMAGE]`
	- Remove uma imagem.
- `docker rmi -f [IMAGE]`
	- Remove a imagem, mesmo que um container esteja usando.
- `docker inspect [CONTAINER_ID]`
	- Mostra todas as informações do container

## Comandos intermediários

- `docker run [COMANDS] [IMAGE] [PARAMETERS]`
	- Exemplo de parâmetros
		- `--memory [X]m`
			- Define um limite de tamanho `X` a memória de um container
		- `--cpu-shares [MEMORY_VALUE]`
			- Define um limite de processamento de acordo com o tamanho da memória.
		- `--name [NAME]`
			- Define `NAME` como o nome do container
		- `-e`
			- Adiciona uma variável de ambiente
		- `[IMAGE]:[VERSION]`
			- Especifica a versão da imagem
		- `-p [LOCAL_PORT]:[CONTAINER_PORT]`
			- Define uma porta visivel do container de acordo com uma porta da sua maquina
		- `-v [LOCAL_DIR]:[MIRROR_CONTAINER_DIR]`
			- Mapeia o volume do container para um diretório local
		- `-volume-from [CONTAINER_ID]`
			- Importa o volume de um determinado container
		- `docker update [RESOURCE] [VALUE] [CONTAINER_ID]/[CONTAINER_NAME]`
			- Altera a quantidade de um recurso de um container. Exemplo de recursos:
				- `--memory`
					- Memoria
				- `--cpu-shares`
					- CPU
		- `docker build [_Dockerfile_] [PARAMERS]`
			- Constroi e executa o _Dockerfile_. Exemplos de parâmetros:
				- `-t [NAME]:[VERSION].`
					- "Builda", corretamente, o _Dockerfile_ com um nome e uma versão. 

## Comandos de Rede

- `docker run [COMAND] --dns 8.8.8.8 [IMAGE]`
	-Passa um servidor dns para o container.
- `docker run [COMAND] --hostname [NAME] [IMAGE]`
	- Define um nome ao container interno. Diferente do `--name` que define um nome ao container externo (visivel para o gerênciamento dos dockers, mas não pro container em si). 
- `docker run [COMAND] --link [CONTAINER_A] [PARAMETERS] [CONTAINER_B] `
	- Deixa o conteiner `A` visível ao container `B`.
- `docker run [COMAND] --expose [PORT] [IMAGE]`
	- Expõe a porta do container para o meio externo. Como o passo `EXPOSE` do _Dockerfile_.
- `docker run [COMAND] --publish/-p [PORT_REF]:[CONTAINER_PORT]`
	- Redireciona a porta de referência da maquina para a porta do container.
- `docker run -ti --mac-address [MAC_ADDRESS] [IMAGE]`
	- Define um endereço _MAC_ a imagem.
- `docker run [COMAND] --net=host [IMAGE]`
	- Faz com que o _Stack_ de _networking_ seja utilizado no _host Docker_ e não no _container_. Ou seja, o _IP_ e outras informações de redes no _container_ será do _host_ e não do _container_.

## Comandos Personalizados

- `docker inspect -f {{.Mounts}} [CONTAINER_ID]`
	- Retorna o volume do host que o container está usando.
- `docker inspect [IMAGE]:[VERSION]`
	- Retorna os detalhes de uma imagem contida em um container.
- `docker history [IMAGE]:[VERSION]`
	- Retorna as informações sobre as camadas da imagem. Bom para ver a como foi a evolução do container.

## Comandos auxiliares

- `ss -s`
	- Verifica as portas que estão sendo usadas.
- `ss -a`
	- Lista as portas usadas e os os processos.
- `ip addr`
	- Apresenta o ip da maquina.
- `iptables -t nat -L -n`
	- Mostra a tabela de ips e seus respectivos redirecionamento.
	
# _Dockerfile_

- `FROM imagem:versão(opcional)`
	- Define uma imagem base para montar o container em cima dela
- `RUN comando`
	- Executa o comando quando rodar o container. Geralmente usado para instalar programas ao container. 
- `ADD arquivo /diretorio/`
	- Importar um arquivo do diretório local para o container
- `CMD ["command1","command2",...]`
	- Executa os comandos logo ao executar a imagem
- `LABEL` Description="Algum metadado"
	- Adiciona metadados ao seu container, por exemplo, versão, fabricante, descrição e etc.
- `COPY arquivo /diretorio/`
	- Somente copia um arquivo ou diretório para o seu container. Diferente do `ADD` que pega arquivos empacotados, por exemplo, `*.zip`, `*.rar`, `*.targz` e etc
- `ENTRYPOINT ["PROCESSO","EVENTO","AÇÃO"]`
	- É o ponto de entrada quando você iniciar o container, ou seja, geralmente definimos no ENTRYPOINT o comando ou script que chama o processo responsável pela execução do container e que manterá o container vivo.
- `ENV name="value"`
	- Adiciona uma variável de ambiente a seu container
- `EXPLOSE port`
	- Define uma porta do container para ser exposta
- `USER nome_usuario`
	- Define o usuário padrão do container.
- `WORKDIR diretorio`
	- Define o diretório raiz do container
- `VOLUME diretorio`
	- Define o volume do container
- `MAINTAINER maintainer maintainer@email.com`
	- Informa quem é o manutenedor do container
