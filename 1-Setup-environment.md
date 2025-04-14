# Guia de Configuração de Servidor para Desenvolvimento

## Actualização do Sistema

```bash
sudo apt update && sudo apt upgrade && sudo apt autoremove && sudo apt autoclean
```

## Instalar o PHP 8.3

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.3 php8.3-common php8.3-cli php8.3-fpm php8.3-mysql php8.3-pgsql php8.3-mbstring php8.3-xml php8.3-curl php8.3-zip php8.3-redis php8.3-bcmath php8.3-gd libapache2-mod-php8.3 php8.3-soap php8.3-sqlite3 php8.3-intl php8.3-memcached php8.3-imagick php8.3-xdebug php8.3-opcache
sudo apt install php-gnupg
```

## Instalar Composer

Link: [https://getcomposer.org/download/](https://getcomposer.org/download/)

## Instalar o Apache

```bash
sudo apt update && sudo apt upgrade
sudo apt install apache2
sudo systemctl status apache2
sudo systemctl start apache2
sudo systemctl enable apache2
```

## Configurar o Apache para usar o PHP 8.3

```bash
sudo a2enmod php8.3
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php8.3-fpm
sudo systemctl restart apache2
```

# Configuração de Bancos de Dados

## Instalar o PostgreSQL

```bash
sudo apt update && sudo apt upgrade
sudo apt install postgresql postgresql-contrib -y
sudo systemctl status postgresql
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

## Configurar o PostgreSQL

```bash
sudo -i -u postgres
psql
ALTER USER postgres PASSWORD 'admin';
CREATE USER root WITH PASSWORD 'admin';
ALTER USER root WITH SUPERUSER;
\l
CREATE DATABASE myapp_db;
ALTER DATABASE myapp_db OWNER TO root;
\q
```

## Instalar pgAdmin

Link: [https://www.pgadmin.org/download/pgadmin-4-apt/](https://www.pgadmin.org/download/pgadmin-4-apt/)

## Instalar MySQL

```bash
sudo apt update && sudo apt upgrade
sudo apt install mysql-server
sudo systemctl status mysql
sudo systemctl start mysql
sudo systemctl enable mysql
```

## Configurar a instalação do MySQL

```bash
sudo mysql_secure_installation
```
*Seguir as instruções na tela para configurar a senha do root e outras opções de segurança.*

## Configurar o MySQL

```bash
mysql -u root -p
CREATE USER 'user_here'@'localhost' IDENTIFIED BY 'password_here';
ALTER USER 'user_here'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password_here';
GRANT ALL PRIVILEGES ON *.* TO 'user_here'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
SELECT User, Host FROM mysql.user;
EXIT
```

## Instalar o phpMyAdmin

```bash
sudo apt install phpmyadmin
```
*Durante a instalação, você será solicitado a escolher o servidor web a ser configurado. Selecione o **Apache2** e pressione **OK**.*

## Configurar o phpMyAdmin

```bash
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
sudo systemctl restart apache2
```
Acessar: http://localhost/phpmyadmin

## Desabilitar o php8.4 voltando para 8.3

```bash
update-alternatives --display php
sudo update-alternatives --config php
sudo a2dismod php8.4
sudo a2enmod php8.3
sudo systemctl restart apache2
```

## Configurar o acesso remoto (opcional)

```bash
sudo nano /etc/apache2/apache2.conf
```

*Adicione ou altere as permissões para permitir o acesso remoto:*
```apache
<Directory /var/www/>
   Options Indexes FollowSymLinks
   AllowOverride None
   Require all granted  # Alterar de 'Require local' para 'Require all granted'
</Directory>
```

```bash
sudo systemctl restart apache2
```

# Configuração do Redis

## Instalar Redis
```bash
sudo apt update && sudo apt upgrade
sudo apt install redis-server
sudo systemctl status redis-server
sudo systemctl start redis-server
sudo systemctl enable redis-server
```

## Configurar o Redis (opcional)
*Por padrão, o Redis está configurado para rodar apenas no modo local (não acessível externamente). Se você quiser permitir o acesso remoto ou fazer outras configurações personalizadas, siga os passos abaixo:*

*Edite o arquivo de configuração do Redis:*
```bash
sudo nano /etc/redis/redis.conf
```

## Permitir acesso remoto (opcional):
*Se você quiser acessar o Redis de uma máquina remota, localize a linha `bind 127.0.0.1 ::1` e altere para permitir o acesso de todos os IPs ou de um IP específico, como no exemplo abaixo:*

```
bind 0.0.0.0
```

*Cuidado: Se você fizer isso, certifique-se de proteger o Redis com uma senha e considere o uso de um firewall para limitar o acesso à instância.*

## Configurar senha (opcional):
*Caso queira adicionar uma senha para o Redis, localize a linha `# requirepass foobared` e descomente (removendo o `#`), substituindo por algo como:*

```
requirepass sua_senha_forte
```

## Reinicie o Redis após as mudanças:
```bash
sudo systemctl restart redis-server
```

## Testar a instalação do Redis
```bash
redis-cli
```

*Dentro do prompt do Redis, digite:*
```
ping
```
*Se tudo estiver certo, o Redis deve responder com:*
```
PONG
```

*Se você configurou uma senha, use o comando abaixo para autenticar:*
```bash
redis-cli -a sua_senha_forte
```
