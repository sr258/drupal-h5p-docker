# docker-compose: Drupal 7 + H5P with development mount

1. ```mkdir files``` (this is where Drupal will store data related to H5P)
2. ```sudo chmod 777 files``` to allow the www-data user inside the container to access to local folder
3. ```docker-compose build``` to fetch latest Drupal 7 and Drush
4. ```docker-compose up``` to start container.
5. Access Drupal at http://localhost:8001 (Username: admin, Password: admin)
6. Use the files in the ```files/h5p``` folder to develop for H5P.