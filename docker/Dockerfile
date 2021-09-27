FROM centos:7

# Install Apache
RUN yum -y update
RUN yum -y install httpd httpd-tools

# Install EPEL Repo
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
 && rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm

# Install PHP
RUN yum --enablerepo=remi-php74 -y install php php-bcmath php-cli php-common php-gd php-intl php-ldap php-mbstring \
    php-mysqlnd php-pear php-soap php-xml php-xmlrpc php-zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

# composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Add entrypoint
COPY docker-entrypoint /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint
ENTRYPOINT ["docker-entrypoint"]

WORKDIR /var/www/html

# Add outside user to apache group ( Not Work yet :< )
# ARG HOST_USER_UID=1000
# RUN useradd -G apache,root -u $HOST_USER_UID -d /home/dev dev

# Start Apache
CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
