# Resolução de problemas, cache, logs e comandos básicos

## DEBUGLEVEL e TIMEOUT

Conforme a documentação oficial os valor os valores padrões são:

#- ZBX_DEBUGLEVEL=3  
#- ZBX_TIMEOUT=4

Caso precisa alterar é só descomentar e mudar o valor, sendo o ZBX_DEBUGLEVEL mínimo 0 e máximo 5.

## Limpando o cache 

Para as alterações entrarem em vigor mais rapidamente, caso esteja com preça após uma alteração o comandondo abaixo vai ajudar.

`docker exec -it zabbix-server zabbix_server -R config_cache_reload`


## Consultando logs

O `-f` é opcional é para seguir o log em tempo real.

Sintaxe é `docker logs container-name`

Exemplos:

`docker logs -f zabbix-agent`

`docker logs -f zabbix-server`

`docker logs -f zabbix-frontend`

`docker logs -f mysql`


## Definindo uma senha para o root do banco de dados

Comente com `#` a variável `MYSQL_ALLOW_EMPTY_PASSWORD=yes` e descomente a  MYSQL_ROOT_PASSWORD=minha-senha

#- MYSQL_ALLOW_EMPTY_PASSWORD=yes
- MYSQL_ROOT_PASSWORD=pass


## Habilitando phpmyadmin

Descomente todas as linhas.

Dica: No VSCODE selecione todas as linhas e pressione `CTRL`+`/`

http://localhost:8000

Servidor: mysql

Usuário: root

Senha:         # "vazio em branco" ou que você definiu

Com ele pe possível gerenciar o banco de dados por um interface web.
A parte de banco de dados, criar a base, usuário, permissão e importar o schema podem ser feitos pelo phpmyadmin.


## Habilitando Grafana

Descomente todas as linhas.

Dica: No VSCODE selecione todas as linhas e pressione `CTRL`+`/`

http://localhost:3000

Usuário: admin

Senha: admin
