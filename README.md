# DocumentationForDevOps

UUD-Lab1 Commands
| Step | Command | Description |
|------|---------|-------------|
| 1    | `sudo yum update -y` | Update the system packages. |
| 2    | `sudo yum install httpd -y` | Install the Apache HTTP Server (httpd). |
| 3    | `sudo systemctl start httpd` | Start the httpd service. |
| 4    | `sudo systemctl enable httpd` | Enable httpd to start at boot. |
| 5    | `sudo yum install -y yum-utils device-mapper-persistent-data lvm2` | Install the necessary packages for managing Docker. |
| 6    | `sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo` | Add the Docker repository to the system's repository list. |
| 7    | `sudo yum install -y docker-ce docker-ce-cli containerd.io` | Install Docker Engine, CLI, and containerd. |
| 8    | `sudo systemctl start docker` | Start Docker. |
| 9    | `sudo systemctl enable docker` | Set Docker to automatically start on system boot. |
| 10   | `mkdir httpd-container && cd httpd-container` | Create a directory for the httpd container and navigate into it. |
| 11   | `echo "FROM httpd:2.4\nEXPOSE 80" > Dockerfile` | Create a Dockerfile for the httpd container specifying Apache version 2.4 and expose port 80. |
| 12   | `mkdir -p httpd-container` | Ensure the httpd-container directory is created only if it doesn't already exist. |
| 13   | `cd httpd-container` | Navigate into the httpd-container directory. |
| 14   | `echo "FROM httpd:2.4" > Dockerfile` | Create and write the base image to the Dockerfile. |
| 15   | `echo "EXPOSE 80" >> Dockerfile` | Append the EXPOSE instruction to the Dockerfile to make port 80 available outside the container. |
| 16   | `sudo docker build -t my-httpd .` | Build the Docker image named my-httpd from the Dockerfile in the current directory. |
| 17   | `sudo docker run -d -p 8080:80 --name my-httpd-instance my-httpd` | Run the container from the my-httpd image in detached mode, mapping port 8080 on the host to port 80 in the container, and name the container instance my-httpd-instance. |


Dodatne komande, mozda zatrebaju: 
1. Stop the Existing Container:
   <b>sudo docker stop my-httpd-instance </b>
2. To Delete a Stopped Container:
<b> sudo docker rm [container_name_or_ID] </b>



UUD-Lab2 Commands
| Step                                                      | Command                                                                                     | Description                                                                                        |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| 2. Pull latest Nginx image                                | `sudo docker pull nginx`                                                                    | Pulled the latest Nginx container image from Docker Hub                                            |
| 3. Find latest Nginx image with Podman                    | `podman search nginx --limit 1`                                                             | Find the latest Nginx container image from Docker Hub using Podman                                 |
| 4. List running containers with Podman                    | `podman ps`                                                                                 | List all currently running containers on the system using Podman                                   |
| 5. Pull the latest image found with Podman search         | `podman pull [last_image_found_in_search_command]`                                          | Pull the latest Nginx container image found in the previous Podman search command                  |
| 6. Inspect the image found with Podman search             | `podman image inspect [last_image_found_in_search_command]`                                  | Find more information about the Nginx container image found with the Podman search                 |
| 7. Create a container instance and map to host port 80    | `podman run -d -p 80:80 --name my-nginx nginx:latest`                                       | If an error occurs due to port 80 being in use, resolve by selecting a different port or ensuring no other services are using port 80. Use `sudo ss -tuln` to find a free port, stop and remove the existing container with `podman stop my-nginx` and `podman rm my-nginx`, then run the command again with the new port. |
| 8. Create an instance and map to host port 8080           | `podman run -d -p 8080:80 --name web8080 nginx:latest`                                      | If an error occurs (e.g., name already in use), choose a different name or remove the existing container. |
| 9. List running containers                                | `podman ps`                                                                                 | List all currently running containers on the system using Podman                                   |
| 10. Stop the first container                              | `podman stop my-nginx`                                                                      | Stop the first container you created by using `podman stop`.                                       |
| 11. Attempt to create container named web8080             | `podman run -d -p 8081:80 --name web8080 nginx:latest`                                      | Attempt to create another container instance named `web8080` will result in an error due to the name being already in use. To resolve the issue, choose a different name with `podman run -d -p 8082:80 --name web8080-2 nginx:latest` or remove the existing container with `podman stop web8080` followed by `podman rm web8080`, and then rerun the command with `podman run -d -p 8081:80 --name web8080 nginx:latest`. |
| 12. Inspect the details of web8080                        | `podman inspect web8080`                                                                    | Inspect the details of the `web8080` container.                                                    |
| 13. List all downloaded images                            | `podman images`                                                                             | List all the images you have downloaded using the `podman images` command.                         |
| 14. Export the nginx image                                | `podman image save -o nginx.tar nginx:latest`                                                | Export the nginx image to a tar archive with `podman image save`.                                  |
| 14a. Export the nginx container                           | `podman export -o my-nginx.tar my-nginx`                                                     | Export the container's filesystem with `podman export`.                                            |
| 14b. Differences between save and export                  | N/A                                                                                         | `podman image save` saves the image with all its layers and metadata, suitable for image distribution or backup. `podman export` exports the current filesystem of a container.               |
| 15. Save changes to a new image                           | `podman commit modify-nginx custom-nginx-image`                                              | Save changes made to a container to a new image with `podman commit`.                              |
| 16. Modify container's web page without stopping          | `docker cp index.html web8080:/usr/share/nginx/html/`                              | Change the contents of the default web page of the `web8080` container by copying a new `index.html` file into it without stopping the container. Verify changes with `podman exec web8080 cat /usr/share/nginx/html/index.html`. |
                                

Dodatne komande:
1. Provjera jel Docker runna: <b> sudo docker run hello-world </b>
2. kak mi se kontejner zove: <b> podman ps </b>
3. To check if the Docker daemon is running on your system: <b> sudo systemctl status docker </b>




UUD-Lab3 Commands
| Step | Command | Description |
|------|---------|-------------|
| 1. Containerize the Application Using Podman | `git clone https://github.com/docker/getting-started-app`<br>`cd getting-started-app`<br> `touch Dockerfile `<br> `podman build -t my-nodejs-app .`<br>`podman run -d --name my-running-app -p 3000:3000 my-nodejs-app` | Clone the repository and navigate to the application directory. Build the container image and run the container, mapping port 3000 to the host. |
| 2. Build Image with Initial Tag | `podman build -t getting-started-app .` | Build the initial Docker image from the Containerfile. |
| 3. Rebuild and Tag Image with 0.0.1 | `podman build -t getting-started-app:0.0.1 .` | Build and tag the image with version 0.0.1. |
| 4. Inspect the Image | `podman image inspect getting-started-app:0.0.1` | Inspect the detailed information of the image tagged as 0.0.1. |
| 5. Modify Application and Rebuild | Modify the application code.<br>`podman build -t getting-started-app:0.0.2 .` | Change the message displayed when there are no to-do items. Rebuild the image with the new modifications and tag it as 0.0.2. |
| 6. Compare Image Versions | `podman diff getting-started-app:0.0.1 getting-started-app:0.0.2` | Show the differences between the two image versions, 0.0.1 and 0.0.2. |
| 7. Push Images to Docker Hub | `podman login --username yourusername docker.io`<br>`podman push getting-started-app:0.0.1 docker.io/yourusername/getting-started-app:0.0.1`<br>`podman push getting-started-app:0.0.2 docker.io/yourusername/getting-started-app:0.0.2` | Log in to Docker Hub and push the images tagged 0.0.1 and 0.0.2 to Docker Hub. |
| 8. Create a UBI8 Based Containerfile and Build | Create the Containerfile.<br>`podman build -t myweb:0.0.1 .` | Define a new Containerfile using UBI8 as the base image, including httpd setup. Build the image and tag it as 0.0.1. |
| 9. Configure Quadlet for System Integration | Follow the quadlet guide to create configuration file. | Configure the system so that the container automatically starts with the system using a quadlet configuration file. |
| 10. Clone and Build Go Application Using Multi-Stage Builds | `git clone https://github.com/jstanesic/example-go-app`<br>`cd example-go-app`<br>`podman build -f Dockerfile1 -t example:Dockerfile1 .`<br>`podman build -f Dockerfile2 -t example:Dockerfile2 .` | Clone the Go application repository and navigate to the project directory. Build the image using Dockerfile1 and Dockerfile2 to see the benefits of multi-stage builds. |
| 11. Compare Built Images and Check Benefits of Multi-Stage | `podman image ls` | List and compare the sizes of the built images to discover the benefits of multi-stage builds, especially in reducing the final image size and isolating build dependencies. |


UUD-Lab4 Commands
| Step | Command | Description |
|------|---------|-------------|
| 2. List All Podman Networks | `podman network ls` | List all Podman networks; should only show the default network called "podman" with the bridge driver. |
| 3. Inspect the Default Network | `podman network inspect podman` | Inspect the network configuration of the default Podman network. |
| 4. Create Custom Network with DNS Enabled | `podman network create --subnet 10.88.0.0/16 --gateway 10.88.0.1 --opt "com.docker.network.bridge.name=podman1" --dns-enabled=true labnet` | Create a custom network named "labnet" with DNS enabled. |
| 5. Create MySQL Container | `podman run -d --name mysql --network labnet -e MYSQL_ROOT_PASSWORD=$(openssl rand -base64 32) -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql:latest` | Create a MySQL database container using the official MySQL image. Configure it to randomize the root password, create a "wordpress" database, and a user "student" with a specified password. |
| 6. Create WordPress Container | `podman run -d --name wordpress --network labnet -p 8080:80 -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_USER=student -e WORDPRESS_DB_PASSWORD=DB15secure! -e WORDPRESS_DB_NAME=wordpress wordpress:latest` | Create a WordPress container using the official WordPress image. It should be started in the background, connected to the "labnet" network, and publish port 80 from the container to port 8080 on the host. |
| 8. Create Default Web Page in WordPress | `Step 1: Access Your WordPress Site: http://<VM's-IP-address>:8080`<br>`Step 2: Complete WordPress Installation`<br>`Step 3: Access the WordPress Admin Dashboard, Navigate to Pages, Add content using the WordPress editor, Preview your page, Publish your page.`<br>`Step 5: Go to Settings → Reading, select "A static page", choose the page you created, and click "Save Changes".` | Create a default web page in the WordPress application by accessing the admin dashboard and configuring the homepage. |
| 9. Manage MySQL Container with Bind Mount | `podman stop mysql`<br>`podman rm mysql`<br>`podman run -d --name mysql --network labnet -v /home/student/mysql:/var/lib/mysql:Z -e MYSQL_ROOT_PASSWORD=$(openssl rand -base64 32) -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql:latest` | Delete the old MySQL container, and create a new one which will bind mount the directory `/var/lib/mysql` to `/home/student/mysql`. |
| 10. Recreate MySQL Container with Volume | `podman stop mysql`<br>`podman rm mysql`<br>`podman volume create mysql_data`<br>`podman run -d --name mysql --network labnet -v mysql_data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=$(openssl rand -base64 32) -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql:latest` | Recreate the MySQL container using a volume instead of a bind mount to ensure data persistence. |
| 11. Install Podman Compose | `sudo dnf install -y podman-compose` | Install the podman-compose package to manage multi-container applications with a single command. |
| 13. Create Docker Compose File | `Create a file named docker-compose.yml` | Create a compose file to deploy WordPress and MySQL using podman-compose. |
| 14. Deploy Using Compose File | `podman-compose up -d` | Deploy WordPress and MySQL by using the compose file you created. |

# UUD- Additional Commands

| Step | Command | Description |
|------|---------|-------------|
| 1  | `dnf` | Runs the DNF package manager which is used to manage packages in Fedora-based systems. |
| 2  | `sudo dnf -y install podman` | Installs Podman, a container management tool, on the system. |
| 3  | `podman` | Starts the basic podman command to check its availability. |
| 4  | `podman pull wordpress` | Downloads the latest WordPress container image using Podman. |
| 5  | `podman images` | Lists all container images currently stored locally by Podman. |
| 6  | `podman run -it cc84570a8e5d /bin/bash` | Runs a container from the image with ID `cc84570a8e5d` interactively with a Bash shell. |
| 7  | `podman run cc84570a8e5d` | Runs a container from the image with ID `cc84570a8e5d` in detached mode. |
| 8  | `podman run -t cc84570a8e5d` | Runs a container with a pseudo-TTY from the image ID `cc84570a8e5d`. |
| 9  | `podman run -d cc84570a8e5d` | Runs a container in detached mode from the image ID `cc84570a8e5d`. |
| 10 | `podman exec -it f5a081c4b0f6 /bin/bash` | Opens an interactive Bash shell in the running container with ID `f5a081c4b0f6`. |
| 11 | `podman ps` | Lists all running containers managed by Podman. |
| 12 | `podman exec -it f5a081c4b0f6 "cat /etc/os-release"` | Executes the `cat /etc/os-release` command inside the container `f5a081c4b0f6` to display OS information. |
| 13 | `podman exec -it f5a081c4b0f6 "cat /etc/passwd"` | Executes the `cat /etc/passwd` command inside the container `f5a081c4b0f6` to list user accounts. |
| 14 | `podman exec -it f5a081c4b0f6 "/bin/cat /etc/passwd"` | Executes the `cat` command via the full path inside the container to list user accounts. |
| 15 | `podman exec -it f5a081c4b0f6 /usr/bin/cat /etc/passwd` | Executes the `cat` command using its full path inside the container to show the password file. |
| 16 | `history` | Displays the command line history. |
| 17 | `ls -al` | Lists all files and directories with detailed information in the current directory. |
| 18 | `cat .bash_history` | Displays the content of the Bash history file, showing past commands executed in the shell. |
| 19 | `exit` | Exits the current shell or terminal session. |
| 20 | `podman kill 2b90b7d29678` | Forcefully stops the running container with ID `2b90b7d29678`. |
| 21 | `podman logs f5a081c4b0f6` | Retrieves logs from the container with ID `f5a081c4b0f6`. |
| 22 | `podman exec -it 867947924aca /bin/bash` | Opens an interactive Bash shell inside the container with ID `867947924aca`. |
| 23 | `echo 123` | Echoes the string "123" to the terminal. Useful for testing and script outputs. |
| 24 | `echo 1234` | Echoes the string "1234". Similar usage as above for slightly different test cases. |
| 25 | `echo 12345` | Echoes "12345". Useful for sequential echo testing in scripts. |
| 26 | `sudo dnf install vim` | Installs the Vim text editor via the DNF package manager. |
| 27 | `vim test.txt` | Opens or creates the file `test.txt` in Vim for editing. |
| 28 | `podman pull nginx:latest` | Pulls the latest Nginx container image using Podman. |
| 29 | `podman pull docker.io/library/nginx` | Pulls the Nginx image from Docker Hub using Podman. |
| 30 | `podman rmi docker.io/library/wordpress` | Removes the WordPress image from the local storage. |
| 31 | `podman rm f5a081c4b0f6` | Removes the stopped container with ID `f5a081c4b0f6`. |
| 32 | `podman rm --all` | Removes all stopped containers. |
| 33 | `podman image inspect 2ac752d7aeb1` | Provides detailed information about the image with ID `2ac752d7aeb1`. |
| 34 | `podman run -d 2ac752d7aeb1 nginx` | Runs a detached container using the image `2ac752d7aeb1` with Nginx service. |
| 35 | `podman logs 8d94a02e90ac` | Retrieves logs from the container with ID `8d94a02e90ac`. |
| 36 | `podman run -it --rm -d -p 8080:80 --name web nginx` | Runs a detached Nginx container named `web`, mapping port 8080 externally to 80 internally, and removes itself on exit. |
| 37 | `curl localhost:8081` | Uses Curl to send a request to `localhost` on port `8081`. Useful for testing web services locally. |
| 38 | `podman save 2ac752d7aeb1` | Saves the container image `2ac752d7aeb1` to the local disk. |
| 39 | `podman image save -o image.tar 2ac752d7aeb1` | Saves the image `2ac752d7aeb1` as a tar file named `image.tar`. |
| 40 | `tar xvif image_export.tar` | Extracts the tar file `image_export.tar` with verbose output and shows file information. |
| 41 | `mkdir container` | Creates a directory named `container`. |
| 42 | `cd container` | Changes the current directory to the newly created `container` directory. |
| 43 | `cp ../image_export.tar .` | Copies the `image_export.tar` file into the current directory. |
| 44 | `rm -rf *` | Removes all files and directories in the current directory recursively and forcefully. |
| 45 | `podman export -o image_export.tar 3db8907dc37b` | Exports the filesystem of the container `3db8907dc37b` to a tar file `image_export.tar`. |
| 46 | `mkdir image` | Creates a directory named `image`. |
| 47 | `cd image` | Changes the current directory to `image`. |
| 48 | `tree` | Displays a tree of all files and directories starting from the current directory. |
| 49 | `podman exec -it 3db8907dc37b echo "dobar dan3" > /usr/share/nginx/html/index.html` | Executes a command inside the container `3db8907dc37b` that changes the content of the `index.html` file. |
| 50 | `sudo dnf install git` | Installs Git using DNF. |
| 51 | `podman build -t getting-started .` | Builds a Podman image from a Dockerfile in the current directory with the tag `getting-started`. |
| 52 | `podman run -d localhost/getting-started` | Runs a container in detached mode from a local image named `getting-started`. |
| 53 | `curl localhost:3000` | Sends a request to `localhost` on port `3000` to test connectivity to a web application. |
| 54 | `podman build -t getting-started:0.0.1 .` | Builds a container image with a specific tag `0.0.1` from a Dockerfile in the current directory. |
| 55 | `podman push docker.io/mpleslic/test/myweb:01` | Pushes the image `test/myweb:01` to a Docker Hub repository. |
| 56 | `podman login docker.io` | Logs into the Docker Hub registry. |
| 57 | `systemctl cat cups` | Displays the content of the CUPS service unit file using systemd's `cat` command. |
| 58 | `podman build -f Dockerfile1 -t example1:0.0.1` | Builds an image from a specified Dockerfile `Dockerfile1` with the tag `example1:0.0.1`. |
| 59 | `podman build -f Dockerfile2 -t example2:0.0.1` | Builds an image from `Dockerfile2` with the tag `example2:0.0.1`. |
| 60 | `podman diff 1f01460f454b 4be465c938a2` | Displays differences between the container states identified by the IDs `1f01460f454b` and `4be465c938a2`. |
| 61 | `mkdir mycontainer` | Creates a directory named `mycontainer`. |
| 62 | `cd mycontainer` | Changes the directory to `mycontainer`. |
| 63 | `touch Containerfile` | Creates an empty file named `Containerfile` in the current directory. |
| 64 | `podman pull redhat/ubi8` | Pulls the Red Hat Universal Base Image (UBI) version 8 from the registry. |
| 65 | `podman build -t mojcontainer:0.0.1 .` | Builds a container image with the tag `mojcontainer:0.0.1` from the current directory's Dockerfile. |
| 66 | `podman run -it c70d72aaebb4 /bin/bash` | Runs a container from the image `c70d72aaebb4` interactively using Bash. |
| 67 | `podman network ls` | Lists all networks managed by Podman. |
| 68 | `podman network create --dns=8.8.8.8 labnet` | Creates a new network named `labnet` with a DNS server set to `8.8.8.8`. |
| 69 | `podman run -d --name mysql --network labnet -e MYSQL_RANDOM_ROOT_PASSWORD=yes -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql` | Starts a MySQL container with environment variables set for database initialization, running detached on the `labnet` network. |
| 70 | `podman run -d --name wordpress --network labnet -p 8080:80 -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_USER=student -e WORDPRESS_DB_PASSWORD=DB15secure! -e WORDPRESS_DB_NAME=wordpress wordpress` | Runs a WordPress container connected to the MySQL database on `labnet`, mapping port 8080 on the host to port 80 on the container. |
| 71 | `ifconfig` | Displays network interface configuration. Useful for troubleshooting and configuring network settings. |
| 72 | `sudo dnf install net-tools` | Installs the `net-tools` package which includes the `ifconfig` utility. |
| 73 | `podman network create --subnet 10.88.0.0/16 --gateway 10.88.0.1 labnet2` | Creates another network named `labnet2` with a specified subnet and gateway. |
| 74 | `podman run -d --name wordpress --network labnet2 -p 8085:80 -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_USER=student -e WORDPRESS_DB_PASSWORD=DB15secure! -e WORDPRESS_DB_NAME=wordpress wordpress3` | Runs a second instance of WordPress on `labnet2`, exposed on port 8085. |
| 75 | `podman-compose up` | Starts all services defined in a `docker-compose.yml` file with Podman. |
| 76 | `sudo dnf install podman-compose` | Installs the Podman Compose tool, allowing the use of Docker Compose files with Podman. |
| 77 | `pip3 install podman-compose` | Installs the Podman Compose Python package. |
| 78 | `sudo dnf install python39 -y` | Installs Python version 3.9 using DNF. |
| 79 | `python3.9 -m pip install` | Uses Python 3.9 to install packages via pip. |
| 80 | `podman-compose --help` | Displays help information for Podman Compose commands. |
| 81 | `which python3.9` | Locates the path of the Python 3.9 executable. |

