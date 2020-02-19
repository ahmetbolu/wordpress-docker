# Wordpress & Docker {Wordpress, MySQL, PHPMyAdmin}


Tek komut ile Wordpress'in en güncel son sürümünü MariaDB ve PHPMyAdmin ile beraber ayağa kaldırabilirsiniz.

Aşağıdaki kodu "docker-compose.yaml" adlı bir dosyaya ekleyin ve ilgili komutu çalıştırın

Bu komut dosya içindeki servisleri ayarları ile beraber  (Wordpress, MariaDB ya da MySQL, PHPMyAdmin) kuracaktır.

Not: Bilgisayarınızda Docker Uygulaması yüklü olması gerekmektedir: https://www.docker.com/get-started


```
# Yapmanız gereken "docker-compose.yml" adlı dosyanın olduğu konumda aşağıdaki komutu çalıştamk.
$ docker-compose up -d
```
# Projeyi durdurmak/sonlandırmak için komut:
$ docker-compose stop 
```
# Projeyi mevcut dosyaları ile tamamen kaldırmak için gerekli komut (kullanırken dikkatli olunuz):
$ docker-compose down --volumes
```



```
version: '3.7' # Docker Compose Versiyonu detay için: https://docs.docker.com/compose/compose-file/compose-versioning/

services:

# Wordpress Ayarları
  wordpress:
    image: wordpress
    restart: always
    ports:
      - 80:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user
      WORDPRESS_DB_PASSWORD: pass
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - ./wp_data:/var/www/html

# Veritabanı Ayarları
  db:
    image: mariadb:latest # mariadb yerine mysql tercih edebibilirsiniz.
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: user
      MYSQL_PASSWORD: pass
      MYSQL_RANDOM_ROOT_PASSWORD: 'root'
    volumes:
      - ./db_data:/var/lib/mysql

# Phpmyadmin Ayarları
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - '8090:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: pass 



volumes:
  wp_data:
  db_data:
```
