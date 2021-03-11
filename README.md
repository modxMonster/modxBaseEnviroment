# About this Repo

This repository holds the docker-compose configuration intended to run a 
modX development environment, from the basic infrastructure needed, to 
the git life cycle, all the needed components to get you up and running
developing modX extras.

## Prerequisites
1. Make sure you have the latest [docker](https://www.docker.com/) and 
   [docker-compose](https://docs.docker.com/compose/install/) installed 
2. After installing both components, make sure your docker instance is 
   running by clicking the docker system try icon => dashboard, this 
   should popup the docker dashboard, and you can check if the instance
   is running in the bottom left corner, if everything is correct, it should
   read "Docker running", with a green button ath the left side of the text.
3. For local development you'll need to a set of certs, those files are already
   part of the current repo inside the certs folder.  
   If you NEED to generate the ssl selfsigned 
   certificates, [here is a guide on how to do so](https://medium.com/the-new-control-plane/generating-self-signed-certificates-on-windows-7812a600c2d8)
   and place the `ssl.crt` and `ssl.key` files inside the certs folder.
4. Follow the wsl installation guide from the [microsoft site](https://docs.microsoft.com/en-us/windows/wsl/install-win10#manual-installation-steps)
   1. Make sure to select the plain 'Ubuntu' installation
   2. Follow the manual until you reach the following message  
   `CONGRATULATIONS! You've successfully installed and set up a Linux distribution that is completely integrated with your Windows operating system!`  
5. Go to Docker -> dashboard -> settings -> resources, and check that your new ubuntu image is listed, the click the switch next to it, and we'll be ready to go!


## Using this image
Once you run the image, the current html and mysql folders are going to get 
populated with all the files for modx installation as well as mysql system files.

1. Configure environment variables: Inside the docker-compose.yml 
   file your can find the different values that can be configured for the images.  
   You need to look for the `MODX_SERVER_ROUTE` key, and update the ip address with the one assigned to your ubuntu instance, for this go to ubuntu and type
   `ifconfig`  
   Also make sure the routes assigned for `volumes` are set up correctly, an example of such route could be `~/development/modxBasicExtra/html:/var/www/html`
   
2. Build the containers
```sh
docker-compose build
```

3. Run the containers
```sh
docker-compose up
```

4. The first time you'll run the container it'll take some time while it 
   downloads and installs the different components, at the end you should see
   a line similar to this one
```sh
web_1  | [Thu Jan 14 13:27:32.027113 2021] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
```   

5. Check that everything is working as expected, for this open `https://MODX_SERVER_ROUTE/`
replace the x's with your actual ip address.

## Setting X410
All these steps, you can see it in this link. [Using Linux PHPStorm inside WSL2](https://www.ddev.com/ddev-local/ddev-local-and-phpstorm-debugging-with-wsl2/)
1. From the terminal, we execute this command.
```
vim .bashrc
```

2. We press "i" and at the end, we add this line of code.
```
export DISPLAY=$(awk '/^nameserver/ {print $2; exit;}' </etc/resolv.conf):0.0
```

4.  For save press "ESC" and then ":wq!"

5.  the section must be like this part.
```
# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
export DISPLAY=$(awk '/^nameserver/ {print $2; exit;}' </etc/resolv.conf):0.0
```

6. Now, we execute this command.
 ```
sudo apt-get update && sudo apt-get install libatk1.0 libatk-bridge2.0 libxtst6 libxi6 libpangocairo-1.0 libcups2 libnss3 xdg-utils x11-apps
```

7. To verify that the configuration was successful, run this command and some eyes appear.
 ```
xeyes
```

## PhpStorm Install

1. We create a development folder.
```
mkdir development
cd development
```

2. Inside the development folder paste the .tar of PhpStorm.

3. Now run these commands.
```
mkdir IDE
sudo chmod -R 0777 IDE
sudo tar -xvf PhpStorm-2020.3.2.tar.gz -C IDE/
```

4. Now run PhpStorm.
```
cd IDE/PhpStorm-181.5281.19/bin
./phpstorm.sh
```

5. If PhpStorm opened, we need to set up the launcher running this command.
 ```
sudo ln -s /home/AdminName/development/IDE/PhpStorm-203.7148.74/bin/phpstorm.sh /usr/bin/storm
```

6. From now on you can open your project using storm . &
 ```
:~/development/newProject/storm . &
```

7. Inside to PhpStorm go to help > Edit Custom VM Option and at the end paste this command.
 ```
-Djava.net.preferIPv4Stack=true
```

## Setting Xdebug

1. From the terminal, run "ifconfig" and save the first IP.

2. Run docker ps and copy the container name that ended in web_1.

3. Rename the container to this command and run it.
 ```
docker exec -it containerName_web_1 /bin/bash
```

4. Run these commands
 ```
apt update
apt install vim
```

5. When the installation is complete, run this command
```
vim /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```

6. Now press "i" and change the IP to the one copied in the first step

7. For save press "ESC" and then ":wq!"

8. At this point, the container must be down, and Inside of Phpstorm go to "run" and select "start listening".

9. Now up the container again and the Xdebug must be working. 




