FROM drupal:7

COPY ./uploads.ini /usr/local/etc/php/conf.d/uploads.ini
RUN mkdir -p /var/www/html/sites/default/files && \
    chmod a+w /var/www/html/sites/default -R

RUN apt-get update && \
 apt-get install -y wget sudo sqlite3 curl
RUN curl -s https://api.github.com/repos/drush-ops/drush/releases/latest | grep -o "http.*phar" | xargs -L 1 wget -O drush.phar && \
    chmod +x drush.phar && \
    mv drush.phar /usr/local/bin/drush
WORKDIR /var/www/html
RUN sudo -u www-data drush site-install --db-url=sqlite://sites/h5pdev/files/.ht.sqlite --account-pass=admin -y
RUN sudo -u www-data drush en h5p -y
RUN sudo -u www-data drush en h5peditor -y