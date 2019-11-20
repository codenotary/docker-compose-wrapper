# docker-compose-wrapper

Overview
--------
Wrapper for script for docker-compose adding some very useful convenience commands:
   ```
   Convenience commands (parameters in brackets are optional):
   cleanup            Removes all docker containers and images
   edit               Edit the current compose file in vi
   exec <name> <cmd>  Executes a command in a container
   gc     [<name>]    Removes unused docker containers and images
   inspect <name>     Show all container information in JSON format
   shell   <name>     Opens a bash terminal in a container
   show               Shows the current compose file
   stats  [<name>]    Print system usage statistics for containers
   status [<name>]    See 'ps' command
   renew  [<name>]    Renews containers by stopping, removing and creating them
   update [<name>]    Updates containers by pulling, stopping, removing and creating them
   auth   [<name>]    Authenticate container images by pulling, authenticate using CodeNotary
   ```

Prerequisites
-------------
* [Docker](https://docs.docker.com/engine/installation/) must be installed
* [Docker-Compose](https://docs.docker.com/compose/install/) must be installed
* [CodeNotary vcn](https://github.com/vchain-us/vcn) must be installed under /usr/local/bin/vcn
* jq should be installed / apt install jq

Installation
------------
* Download docker-compose-wrapper script to `/usr/local/bin` folder:

   ```
   sudo wget https://raw.githubusercontent.com/chrisipa/docker-compose-wrapper/master/docker-compose-wrapper -O /usr/local/bin/docker-compose-wrapper
   ```
   
* Make docker-compose-wrapper script executable:   

   ```
   sudo chmod +x /usr/local/bin/docker-compose-wrapper
   ```
   
* Create sample init script `/etc/init.d/my-app-stack` for your application stack:

   ```
   #!/bin/bash
   ### BEGIN INIT INFO
   # Provides:          my-app-stack
   # Required-Start:    docker
   # Required-Stop:
   # Default-Start:     3 5
   # Default-Stop:      0 1 2 6
   # Description:       My App Stack Init Script
   ### END INIT INFO

   # set environment variables
   export TZ="Europe/Berlin"

   # set compose file location
   composeFile="/opt/my-app-stack/docker-compose.yml"

   # include docker-compose-wrapper script
   source /usr/local/bin/docker-compose-wrapper
   ```
   
* Make init script executable:   

   ```
   sudo chmod +x /etc/init.d/my-app-stack
   ```   
   
* Register init script for autostart:

   ```
   sudo chkconfig my-app-stack on
   ```
   
* Now you can use your init script to control docker-compose in a convenient way:

   ```
   sudo service my-app-stack
   ```
