## Overview

This document describe how to implement inference-preprocessing project to EC2 instance using deploy agent.

### Create EC2 Instance to deploy

+ when create EC2 instance we have to add tag which is used in code deploy group setting to match

+ And we should IAM user role to EC2 instance to get source from S3.

  + create new IAM user role with full S3 access permission

+ reference:  

### Install code deploy agent 

+ There are two way to install codedeploy agent

+ Install it from S3 resources

+ Install deploy agent with s3
  + ```
    sudo apt-get update
    sudo apt-get install ruby
    wget https://aws-codedeploy-us-west-1.s3.us-west-1.amazonaws.com/latest/install
    chmod +x ./install
    sudo ./install auto > /tmp/logfile
    sudo service codedeploy-agent status
    sudo service codedeploy-agent start
    ```
When create code deploy group, there is setting to install code deploy agent to ec2. so we can use it

### Create code deploy application and application group

create application group

we have to add tag which is set in ec2 to match

### Create codepipeline

+ Source provider - github (Version2)

+ Code Deploy

  + Here is appspec.yaml to deploy
    + ```
      version: 0.0
      os: linux
      files:
        - source: /
          destination: /home/ubuntu/www/project
      permissions:
        - object: /home/ubuntu/www
          pattern: "**"
          owner: ubuntu
          group: ubuntu
      hooks:
        ApplicationStop:
          - location: scripts/stop_application.sh
            timeout: 6000
            runas: ubuntu
        BeforeInstall:
          - location: scripts/clean_instance.sh
            timeout: 6000
            runas: root
        AfterInstall:
          - location: scripts/install_python_dependencies.sh
            timeout: 6000
            runas: ubuntu
          - location: scripts/migrate.sh
            timeout: 6000
            runas: ubuntu
        ApplicationStart:
          - location: scripts/start_application.sh
            timeout: 6000
            runas: ubuntu
        ```

