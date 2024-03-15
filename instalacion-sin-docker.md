# Instalación de Redmine

Este tutorial describe la instalación de Redmine de forma directa en el sistema. Esto es, sin usar ningún contenedor. Instalaremos los servicios directamente sobre la máquina y los configuraremos de forma individual para que interactúen entre ellos. Necesitamos instalar:

* El intérprete de ruby y sus librerías
* El gestor de base de datos MySQL
* El servidor web Apache
* Varias dependencias para complementar a los anteriores


## Paso 1: Actualizar el sistema

Antes de instalar cualquier nuevo software, siempre es la mejor práctica actualizar la lista de paquetes de su sistema y actualizar los paquetes existentes.

    sudo apt update && sudo apt upgrade -y


## Paso 2: Instalar Dependencias

Redmine requiere que Ruby, una base de datos (como MySQL o PostgreSQL), y otras bibliotecas funcionen correctamente.

    sudo apt install ruby-full build-essential zlib1g-dev libxml2-dev libmysqlclient-dev libpq-dev libmagickwand-dev


## Paso 3: Instalar el servidor de bases de datos

Elija un servidor de base de datos para usar con Redmine. Aquí usamos MySQL como ejemplo:

    sudo apt install default-mysql-server default-mysql-client libmysqlclient-dev


Para probar su instalación de MySQL y cree una base de datos para Redmine:

    sudo mysql_secure_installation
    mysql -u root -p
    CREATE DATABASE redmine CHARACTER SET utf8mb4;
    GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost' IDENTIFIED BY 'password';
    FLUSH PRIVILEGES;
    EXIT;


## Paso 4: Instalar Redmine

Descargue la última versión estable de Redmine del sitio web oficial:

    wget https://www.redmine.org/releases/redmine-x.y.z.tar.gz
    tar xzf redmine-x.y.z.tar.gz
    sudo mv redmine-x.y.z /opt/redmine

Copia el archivo de configuración de ejemplo y edítelo para que coinádalos con la configuración de su base de datos:

    sudo cp /opt/redmine/config/database.yml.example /opt/redmine/config/database.yml
    sudo nano /opt/redmine/config/database.yml

Instale las dependencias de Ruby:

    cd /opt/redmine
    sudo gem install bundler
    bundle install --without development test


## Paso 5: Configurar Redmine

Genera un símbolo secreto y prepara la base de datos:

    rake generate_secret_token
    RAILS_ENV=production rake db:migrate
    RAILS_ENV=production rake redmine:load_default_data


A continuación, establezca los permisos de archivo correctos:

    sudo chown -R www-data:www-data /opt/redmine


## Paso 6: Instalar y configurar el servidor web

Instale Phusion Passenger como servidor de aplicaciones para Redmine:

    sudo apt install libapache2-mod-passenger
    sudo a2enmod passenger
    sudo service apache2 restart


Añadir un nuevo anfitrión virtual para Redmine:

    sudo nano /etc/apache2/sites-available/redmine.conf


Añadir la siguiente configuración, luego guardar y salir:

    <VirtualHost *:80>
        ServerName redmine.example.com
        DocumentRoot /opt/redmine/public
        <Directory /opt/redmine/public>
            AllowOverride all
            Options -MultiViews
        </Directory>
    </VirtualHost>


Habilitar al host virtual y reiniciar Apache:

    sudo a2ensite redmine
    sudo service apache2 restart


## Paso 7: Primer acceso a Redmine

Abra su navegador web y navegue al dominio que configuró para Redmine. Deberías ver la página de la casa de Redmine. La cuenta de administrador por defecto es:

    Nombre de usuario: admin
    Contraseña: admin

Se recomienda encarecidamente cambiar la contraseña predeterminada después del primer inicio de sesión.
