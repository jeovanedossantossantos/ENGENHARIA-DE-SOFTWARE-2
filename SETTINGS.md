<div align="center">


   <code><img height="30" src="https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/fbaab1c4-b737-4cd1-ae92-7f6db05411ac"></img></code>
   <code><img height="30" src="https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/6756fd16-549f-48e5-aa2e-2e46b3afb681"></img></code>
   <code><img height="30" src="https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/34488836-2904-4336-8787-80fc82c5e7e3"></img></code>
   <code><img height="30" src="https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/2ee2eaef-240d-48b2-852b-c697cbe2d97f"></img></code>
 <code><img height="30" src="https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/ba2a7180-a1cf-42d9-8a82-7599598242ab"></img></code>
 <code><img height="30" src="https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/e4ec28fd-0a49-4ec9-a528-d847af606587"></img></code>
 <code><img height="30" src="https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/7c68856c-eae8-4734-9fc5-fb7110e57682"></img></code>
 
</div>

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

# Instalando o pm2
O pm2 é um gerenciamento de processos.

- Instalação: ```npm install pm2@latest -g``` ou ```yarn global add pm2```
- Roda uma aplicação: ```pm2 start app.js```
- Encerrar uma aplicação: ```pm2 stop app```
- Gerenciando processos:
  
  ```
  pm2 restart app_name
  pm2 reload app_name
  pm2 stop app_name
  pm2 delete app_name
  
  ```

  <a href="https://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/">Referência</a>

# Colocando um projeto no servidor

- Deve ser colocado dentro do diretorio padrão do servidor ```cd /var/www```
- Clone o projeto com o ```git clone```, será necessaio fazer o login com o usuario e com o token do github, depois de um cd para entra no projeto.
- Crindo o .env ```cat > .env``` ou se tiver um env de exemplo ```cp .envExemplo .env```
- Instale as dependências
- Rode as migrações, se ouver.

# Configurando o nginx e certbot

- Instalando: ```sudo apt-get install nginx```
- Crinado o arauivo de configuração: ```sudo mkdir /etc/nginx/sites-available```
- Abrindo o arquivo de configuração: ```sudo nano /etc/nginx/sites-available/nome.que.for.melhor```
- Restarte o nginx: ```sudo systemctl restart -nginx```
- Instale o certbot: ```sudo snap install --classic certbot```
- Configuração do certbot: ```sudo ln -s /snap/bin/certbot /usr/bin/certbot```
- Automatizar a instalação e configuração de certificados SSL/TLS para um servidor web Nginx: ```sudo certbot --nginx```

### Explicação dos comando da configuração


1 - ```upstream nodejs { ... }```: Isso define um bloco de configuração chamado nodejs dentro do contexto upstream. O bloco upstream é usado para definir grupos de servidores que podem ser usados como proxies reversos. Neste caso, estamos criando um grupo chamado nodejs para encaminhar solicitações para um servidor Node.js.

2 - ```server 127.000.1:4000;```: Esta linha especifica um servidor dentro do grupo nodejs para o qual as solicitações serão encaminhadas. O endereço IP 127.0.0.1 é o endereço de loopback, o que significa que se refere ao próprio computador. O número 4000 é a porta em que o servidor Node.js está ouvindo.

Portanto, a configuração indica que o Nginx será configurado para atuar como um proxy reverso para encaminhar as solicitações que ele recebe para um servidor Node.js que está sendo executado localmente no mesmo computador na porta 4000. Isso é comum em muitas aplicações da web, onde o Nginx é usado como intermediário para melhorar o desempenho, a segurança ou a escalabilidade, enquanto o servidor de aplicativos Node.js lida com a lógica da aplicação.

3 - server { ... }: Isso define o início da configuração para um bloco de servidor Nginx.
4 - server_name api.esportes.ness.dev.br;: Especifica o nome do servidor, ou seja, o domínio no qual essa configuração do servidor Nginx será aplicada.

5 - location / { ... }: Esta é uma diretiva de localização que define como o Nginx lida com as solicitações para o caminho raiz ("/") da URL. Ele configura o proxy reverso para encaminhar solicitações para um servidor backend (no exemplo, chamado de nodejs, que é definido no upstream).

6 - proxy_pass http://nodejs/;: Encaminha as solicitações HTTP recebidas para o servidor backend chamado nodejs (definido em uma seção upstream separada, não incluída neste trecho).

7 - As linhas subsequentes (proxy_set_header) configuram cabeçalhos de proxy para repassar informações relevantes para o servidor backend, como o endereço IP real do cliente, o esquema (HTTP/HTTPS), entre outros.

8 - As linhas com add_header adicionam cabeçalhos de segurança e políticas de conteúdo, como "Strict-Transport-Security", "X-XSS-Protection", "Content-Security-Policy" e assim por diante. Esses cabeçalhos ajudam a proteger a aplicação contra várias vulnerabilidades e ataques.

9 - listen 443 ssl;: Define o servidor para ouvir na porta 443 (a porta padrão para HTTPS) e habilita o suporte a SSL/TLS.

10 - ssl_certificate e ssl_certificate_key: Especificam os caminhos para os certificados SSL/TLS (chave pública e privada) fornecidos pelo Certbot.

11 - include /etc/letsencrypt/options-ssl-nginx.conf;: Inclui um arquivo de opções de configuração do Certbot para otimizar as configurações SSL/TLS.

12 ssl_dhparam: Especifica o parâmetro de troca de chaves Diffie-Hellman (DH) usado para melhorar a segurança do handshake SSL/TLS.

13 - server { ... }: Este bloco de servidor é usado para redirecionar o tráfego HTTP para HTTPS. Ele verifica se o host é o domínio desejado e redireciona para o mesmo host usando HTTPS.

14 - listen 80;: Configura o servidor para ouvir na porta 80 (HTTP).

15 - return 404;: Retorna uma resposta 404 (página não encontrada) para qualquer solicitação que chegue neste bloco. Isso é usado para garantir que o tráfego HTTP que não foi redirecionado para HTTPS receba uma resposta 404.

```
  upstream nodejs {
      server 127.000.1:4000;
  }

    server { server_name api.node.test.dev.br; # Substitua pelo seu domínio ou endereço IP

    location / {
        proxy_pass http://nodejs/;  # Substitua pelo endereço do servidor da sua API
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
       add_header X-Content-Type-Options nosniff;

    add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains; preload';

    add_header X-XSS-Protection "1; mode=block";

    add_header Referrer-Policy "strict-origin";

    add_header Permissions-Policy "geolocation=(),midi=(),sync-xhr=(),microphone=(),camera=(),magnetometer=(),gyrosc>

    add_header Content-Security-Policy "frame-ancestors 'self'";

    add_header X-Frame-Options "SAMEORIGIN";
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/api.node.test.dev.br/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/api.node.test.dev.br/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

    }
    server {
        if ($host = api.node.test.dev.br) {
            return 301 https://$host$request_uri;
        } # managed by Certbot
    
    
    
            listen 80; server_name api.node.test.dev.br;
        return 404; # managed by Certbot
    
    
    }
```

ou

```

upstream siimesporte_backend {

   server 127.0.0.1:4444; ############### MUDAR AQUI

}

server {

  root /home/jeovane/sys_esporte_front/build; ############### MUDAR AQUI ou /var/www

  index index.html;

  server_name siimesportes.ness.dev.br; ############### MUDAR AQUI



  location / {

    try_files $uri $uri/ /index.html;

  }



  location /backend/ {

    proxy_pass http://siimesporte_backend/;

    proxy_set_header X-Real-IP $remote_addr;

    proxy_set_header Host $http_host;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

  }



    listen 80;



    add_header X-Content-Type-Options nosniff;

    add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains; preload';

    add_header X-XSS-Protection "1; mode=block";

    add_header Referrer-Policy "strict-origin";

    add_header Permissions-Policy "geolocation=(),midi=(),sync-xhr=(),microphone=(),camera=(),magnetometer=(),gyroscope=(),fullscreen=(self),payment=()";

    add_header Content-Security-Policy "frame-ancestors 'self'";

    add_header X-Frame-Options "SAMEORIGIN";



}


```
<a href="https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/">NGINX referencia</a>

<a href="https://certbot.eff.org/">Certbot referencia</a>


![image](https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/b3b59af6-ac24-4f0d-ad0f-d2c9e3f78387)

![image](https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/5e3ad2fb-b6ef-462b-82de-3805ae1de3cc)

![image](https://github.com/jeovanedossantossantos/ENGENHARIA-DE-SOFTWARE-2/assets/60934938/7484870e-9351-4885-86df-8e928615d8c5)



