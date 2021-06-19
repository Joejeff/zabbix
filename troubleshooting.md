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


## Verificando a escuta das portas no host docker

Portas usadas pelo zabbix

10050 (server)
10051 (cliente) 

`ss -lntp`

State       Recv-Q       Send-Q              Local Address:Port                Peer Address:Port
LISTEN      0            4096                      0.0.0.0:443                      0.0.0.0:*          
LISTEN      0            4096                      0.0.0.0:8000                     0.0.0.0:*          
LISTEN      0            4096                      0.0.0.0:10050                    0.0.0.0:*          
LISTEN      0            4096                      0.0.0.0:10051                    0.0.0.0:*          
LISTEN      0            4096                      0.0.0.0:5000                     0.0.0.0:*          
LISTEN      0            4096                      0.0.0.0:3306                     0.0.0.0:*          
LISTEN      0            4096                      0.0.0.0:8080                     0.0.0.0:*          

```
ss --help
-l, --listening
-n, --numeric
-t, --tcp
-p, --processes
```
