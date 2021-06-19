# Laboratório de estudo Zabbix com Docker

## Clonando o repositório

`git clone https://github.com/Joejeff/zabbix.git`

`cd zabbix`

**Subindo os containers**

`docker-compose up -d`

**Verificando se estão ativos**

`docker-compose ps`

Obs. Na primeira execução ir para **Criando o banco de dados** e demais passos.

**Parando os containers**

`docker-compose down`

Obs. Os dados de bando de dados são persistentes na pasta `./zabbix/mysql`

## Criando o banco de dados

Acesse o container do banco de dados

`docker container exec -ti mysql bash`

Dentro do container 

```
# mysql -uroot -p
password (Enter se estiver usando a senha em branco)
mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user zabbix@localhost identified by 'zabbix';
mysql> grant all privileges on zabbix.* to 'zabbix'@'%';
mysql> quit;
```

De curiosidade o comando abaixo que substitui o **create user** e o **grant all privileges** em apenas uma linha.

`mysql> grant all privileges on zabbix.* to 'zabbix'@'%' identified by 'zabbix';`


## Importando o schema de banco de dados do Zabbix

Agora vamos precisar importar o schema do banco de dados "criar as tabelas necessárias".
A instalação do zabbix nos fornece isso, o scritp é disponibilizado na pasta `/usr/share/doc` procure pelo arquivo `create.sql.gz` dentro de uma pasta com "zabbix" no nome.

Acesse o container do zabbix-server

`docker container exec -ti zabbix-server bash`

Copie o arquivo *create.sql.gz* para a pasta */var/lib/zabbix/export/* que é mapeada na pasta do projeto *./zbx_env/var/lib/zabbix/export*.

`$ cp /usr/share/doc/zabbix-server-mysql/create.sql.gz /var/lib/zabbix/export/`

Na maquina host, copie o arquivo `./zbx_env/var/lib/zabbix/export/create.sql.gz` para `./zabbix/mysql` para termos acesso ao mesmo de dentro do container mysql, nosso banco de dados.


Acesse novamente o container do banco de dados

`docker container exec -ti mysql bash`

```
cd /var/lib/mysql
zcat create.sql.gz | mysql -uzabbix -p zabbix
```
Esse passso pode demorar um pouco, e ao termino finalizamos, zabbix pronto para uso.

## Acessando o zabbix frontend

URL: http://localhost:8080
ou
URL: http://127.0.0.1:8080
ou
http://ip-do-host-docker:8080

Usuário e senha padrão:

User: `Admin`  # Atenção ao A ele é maiúsculo

Senha: `zabbix`


## Ajustando o monitoamento do zabbix-agent para monitor o zabbix-server

Logado na interface web, vá no menu Configuration / Hosts.

Clique no `Zabbix server`

Em Interfaces, em **DNS name** digite `zabbix-agent` e em **connet to** clique em `DNS`

Clique em update.

O Availability do Zabbix server agora deve ficar verde em no maximo 5min.