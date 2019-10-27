# Drupal 7 + H5P development environment

This Docker image is based on the regular Drupal 7 image. It differs in these respects:

1. It allows you to mount the container directory ```/var/www/html/sites/default/files``` as a volume. This is the location where H5P stores its data.
2. The installation of the default site is done automatically (uses SQLite as database).
3. The PHP upload limit is increased (required by H5P).
4. The modules H5P and H5PEditor are already installed. 

You still need to change the configuration of the H5P module in the settings to enable (library) development mode.

The container is automatically re-built whenever the regular Drupal 7 image is updated (not when H5P is updated).

## Quick start

1. Create a local directory called ```files``` and give everybody write and read permission (as described [below](#prepare-local-directory)).
1. Execute ```sudo docker run --name drupaldev -d -p 8080:80 -v `pwd`/files:/var/www/html/sites/default/files -t sr258/drupal-h5p-docker```
2. Access Drupal at http://localhost:8080 and get any content type from the H5P Hub to initialize the folders in ```files/h5p```.
3. You can now work with the H5P library files in the ```files/h5p``` directory (if you've configured the access rights of the directory properly in step 1).

## Detailed installation instructions

### Installing docker
Go to https://docs.docker.com/install/ and install Docker for your system. It is convenient to configure docker so that you can run ``docker`` as a normal user and don't have to use ``sudo`` every time.

### Preparing the local data directory
To develop H5P libraries in the Docker container you need to be able to modify files inside the running container. The way Docker does this is to mount "volumes" to paths in your file system. You can think of a volume as a symlink between a directory in your local file system and a directory in the container. Drupal stores all H5P data (content and libraries) in ```/var/www/html/sites/default/files```. You have to mount this path to a local directory. **The local directory must be write-accessible** to the user ``www-data`` of the Drupal container and **you must still be able write its contents from your development user account.** To achieve this execute the following from the command line:

```
mkdir files
sudo chmod -R 777 files
```

### Running the container
Navigate to the directory in which you've created the ``files`` directory and execute this command to fetch the container from the Docker Hub and execute it:
```
sudo docker run --name drupaldev -d -p 8080:80 -v `pwd`/files:/var/www/html/sites/default/files -t sr258/drupal-h5p-docker
```

### Accessing the container and configure H5P.
You can access drupal at http://localhost:8080 (username: admin, password: admin)

Your first step should be to enable the "H5P development mode" and "library development directory" in the settings (see http://localhost:8080/node#overlay=admin/config/system/h5p).

## Developing H5P libraries
You can now develop your libraries in the directory ``files/h5p/development`` on your local machine. All changes will be automatically synced to the docker container.

### Stopping the container
To stop the container and remove it from memory, execute the command below. If you restart it later, its execution state will be restored.
```
sudo docker stop drupaldev
```

### Restarting the stopped container
To restart a stopped container execute this command:
``` 
sudo docker restart drupaldev
```

### Completely removing the container
If you want to wipe the whole Drupal container (and all data inside), execute:
```
sudo docker rm drupaldev
```
This will not remove the files in the ``files`` directory, as Docker volumes are designed to be persistent independent of container lifetime.
