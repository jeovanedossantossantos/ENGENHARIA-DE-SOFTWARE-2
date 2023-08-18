# Configuração Inicial de servidor com Ubuntu 

Passo 1 — Fazendo login como raiz

```ssh root@your_server_ip```

Passo 2 — Criando um novo usuário

```adduser sammy```

Passo 3 — Concedendo privilégios administrativos

```usermod -aG sudo sammy```

Passo 4 — Configurando um firewall básico

```
ufw app list
ufw allow OpenSSH
ufw enable
ufw status
```
<a href="https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04-pt">Referência</a>

# Instalando banco de dados

Passo 1 - Baixando e Instalndo o mariadb

- Baixando ```sudo apt install mariadb-server```
- Instalando ```sudo mysql_secure_installation```

Passo 2 - Criara um banco 

- Entra no mysql: ```sudo mysql```
- Criar m usuario: ```GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;```
- Acessando o mysql: ```mysql -u [nomedousuario] -p [senha];``` ou ```mysql -u nomedousuario -p ```
- Acessando remotamente ```mysql -u [nomedousuario] -p [senha] -h [dns ou ip 192.168.1.20];``` ou ```mysql -u nomedousuario -p -h 192.168.1.20```
- Gerenciamento de usuários: ```SELECT User FROM mysql.user;```
- Gerenciando databases

  - Criando database: ```CREATE DATABASE nomedatadabase;```
  - Acessando uma database: ```USE nomedadatabase;```
  - Listando tabelas de uma database: ```show tables;```

<a href="https://www.linuxnaweb.com/comandos-uteis-do-mysql-mariadb/">Referência</a>
