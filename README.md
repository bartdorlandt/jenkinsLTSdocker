# jenkins

Playing with Jenkins and having a permanent docker, to run other dockers.

## Installation

First we'll spin up the Jenkins server. 

    docker compose up -d ltscustomjenkins
    
Find the password using:

    docker logs ltscustomjenkins

Open the browser to [Jenkins](http://localhost:8080/) and type the password.

* Install the plugins
* Provide the admin user
* Set the url (keep it http://localhost:8080)

### Create a node

[Create a node](http://localhost:8080/computer/new)

* name: dockeragent
  * permanent agent
* Remote root directory
  * `/home/jenkins/agent`
* Save

[Open the agent](http://localhost:8080/computer/dockeragent/)

Copy the .jenkins_token_template file to .jenkins_token and modify it as desired.

    cp .jenkins_token_template .jenkins_token

Copy the secret shown to [.jenkins_token file](.jenkins_token).

## Add the agent

Copy the .env_template file to .env and modify it as desired.

    cp .env_template .env

Start the agent.

    docker compose up -d jenkinsagent

Refresh the [dockeragent](http://localhost:8080/computer/dockeragent/) page. 

# Take it down 

To take it down, use the command:

    docker compose down


## Notes

### docker in docker as user (MacOS)

Note the group used for the Jenkins Server. In order to be able to run this on MacOS a bit extra is needed. This is because of the way docker works on MacOS.

    group_add:
      - 0

The docker.sock doesn't have the docker group assigned to it, as in Linux. Instead it has root:root and any other user is not allowed to read this file. 
