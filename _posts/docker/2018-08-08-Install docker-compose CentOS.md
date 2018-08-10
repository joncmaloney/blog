Install docker-compose CentOS
=============================

source:
    https://docs.docker.com/compose/install/#install-compose
    https://github.com/docker/compose/releases

https://docs.docker.com/compose/completion/#bash

Download the compose software
-----------------------------
1. Run this command to download the latest version of Docker Compose:
   

  ```bash
  $ curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o 
  /usr/local/bin/docker-compose
  ```

  

2. Apply executable permissions to the binary

  ```bash
  $ chmod +x /usr/local/bin/docker-compose
  ```

Install command completion
---------------------------
1. Place the completion script in 

  ```bash
  $ cd /etc/bash_completion.d/.
  $ curl -L https://raw.githubusercontent.com/docker/compose/1.22.0/contrib/completion/bash/docker-compose -o 
  $ /etc/bash_completion.d/docker-compose
  ```

  

2. Load the completion into the current terminal session ~/.bash_profile or launch a new terminal to utilize completion..