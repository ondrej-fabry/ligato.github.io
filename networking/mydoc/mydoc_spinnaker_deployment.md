---
title: Spinnaker Deployment
tags: [spinnaker]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_spinnaker_deployment.html
folder: mydoc
---

## Installation Getting Started (instructions to deploy on AWS)

<http://www.spinnaker.io/docs/creating-a-spinnaker-instance>


## Installation on vCenter (not documented on Spinnaker)

We have a OVA with Spinnaker installed but not configured :
[Spinnaker OVA](https://cisco.box.com/s/2sn58egqdm0mio47gzlrvum31sabmh2b)
To upgrade spinnaker to the most recent version:

            apt-get update
            apt-get upgrade spinnaker

OVA is set to DHCP, so if installing on vCenter with static IP, follow: \\
<https://www.howtogeek.com/howto/ubuntu/change-ubuntu-server-from-dhcp-to-a-static-ip-address/>

\\
In order to install from scratch, have a ubuntu 14.04 VM or use the following Ubuntu 14.04 template (root/control) to create the VM:
[Ubuntu 14.04 VM template OVA](https://cisco.box.com/s/2sn58egqdm0mio47gzlrvum31sabmh2b)


Install openssh: (if not installed yet)

        apt-get update
        apt-get  install openssh-server
        export TERM=xterm  
        nano /etc/ssh/sshd_config
        
Sshd_config updates:
        
                # Authentication:
                LoginGraceTime 120
                PermitRootLogin yes
                #StrictModes yes
                RSAAuthentication yes
                PubkeyAuthentication yes
                #AuthorizedKeysFile	%h/.ssh/authorized_keys 
                UsePAM no

Restart ssh:

        service ssh restart

Install Spinnaker:

        apt-get install software-properties-common
        apt-get install git
        git clone https://github.com/spinnaker/spinnaker.git
        ./InstallSpinnaker.sh

Note : to reduce disk utilization (/boot), run :

    apt-get autoremove

## Additional Information for Spinnaker installation on an AWS VM:

### Steps to make the deployment successful:

* Create an AWS User account for Spinnaker
   * In AWS Console, open AWS Identity & Access Management -> Users -> Create New Users.
   * Enter a username and hit Create.
   * Create an access key for the user.
   * Click Download Credentials, you will need them later.
   * Click Close.
* Apply permissions to the new user
   * Click on the username you entered for a more detailed screen.
   * On the Summary page, click on the Permissions tab.
   * Attach a managed policy
     * Click Attach Policy.
     * Click the checkbox next to PowerUserAccess, then click Attach Policy.
   * Create an Inline Policy
     * Click on the Inline Policies header, then click the link to create an inline policy.
     * Click Select for Policy Generator.
     * Select AWS Identity and Access Management from the AWS Service pulldown.
     * Select PassRole for Actions.
     * Type * (the asterisk character) in the Amazon Resource Name (ARN) box.
     * Click Add Statement, then Next Step.
     * Click Apply Policy.
* Add AWS credentials to Spinnaker
   * Connect to the Spinnaker instance (I'm assuming you're using Linux)
   * Run the command: sudo mkdir /home/spinnaker/.aws
   * Run the command: sudo vim /home/spinnaker/.aws/credentials
   * Add the following lines to the file, then save and quit:
     * Make sure to replace my-aws-account to the name of your AWS account
     * Make sure to replace the values of the key ID and access key with the credentials you downloaded in Step 1, Part 4
     
            [my-aws-account]
            aws_access_key_id = AKIAIOSFODNN7EXAMPLE
            aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
    
* Update the default Spinnaker configuration files
   * Run the command: sudo vi /etc/default/spinnaker
   * Locate the line SPINNAKER_AWS_ENABLED, and change the value to true
   * Locate the line SPINNAKER_AWS_DEFAULT_REGION, and change the value to the region where your Spinnaker instance runs (e.g. us-east-1, without the quotes)
   * Save and quit
* Update the local Spinnaker configuration files (these override the stock YAML files that come with the installation of Spinnaker)
   * Run the command: sudo vim /opt/spinnaker/config/spinnaker-local.yml
   * Locate providers, then locate aws beneath that
   * Beneath aws, locate enabled and change the value to true
   * Beneath aws, locate defaultRegion and change the value to the region where your Spinnaker instance runs (e.g. us-east-1, without the quotes)
   * Beneath primaryCredentials, locate name and change the value to my-aws-account
     * Make sure to replace my-aws-account to the name of your AWS account
   * Save and quit
* Restart Spinnaker
   * Run the command: sudo /opt/spinnaker/scripts/reconfigure_spinnaker.sh
   * Run the command: sudo restart spinnaker
* Reset the tunnel from your local machine to the Spinnaker instance

### Possible Problems: 

After Spinnaker upgrade (apt-get upgrade spinnaker), you may get errors running pipelines:
Error modify AMI attributes: InvalidAMIAttributeItemValue: Invalid attribute item value " " for userId item type 

To Fix, add: 

    SPINNAKER_AWS_DEFAULT_ACCOUNT=123456789 (replace with your AWS account ID all numerals)
    into file
    /etc/default/spinnaker
    sudo restart spinnaker


## Additional Information for Spinnaker installation on  Kubernetes:

Spinnaker and Kubernetes: <https://www.youtube.com/watch?v=R0cloa6yi5E>

Spinnaker installed on Kubernetes:\\
<https://github.com/spinnaker/spinnaker/tree/master/experimental/kubernetes/simple>\\
Our fork:\\
<https://github.com/juliole/spinnaker>
 

## Cloud Providers and Adapters Considerations:

### Kubernetes:
Best instructions:\\
<http://www.spinnaker.io/v1.0/docs/target-deployment-configuration#section-kubernetes >\\
and\\
<http://www.spinnaker.io/v1.0/docs/kubernetes-source-to-prod#section-configuring-github>

Update ~/.kube/config, means:\\
On Spinnaker: create .kube folder under /home/spinnaker, create a config file and and copy the content from admin.conf that can be found on the kubernetes master /etc/kubernetes


For AWS need to change the ip in the config file to kubernetes and then add kubernetes to /etc/hosts. Not need for vcenter.


### Docker Registry:
Issue accessing private repos on docker. Had to add pwd to cloud yaml.\\
cloud yaml:

    dockerRegistry:
      enabled: ${providers.dockerRegistry.enabled:true}
      accounts:
        - name: ${providers.dockerRegistry.primaryCredentials.name:juliole}
          address: ${providers.dockerRegistry.primaryCredentials.addres}
          username: ${providers.dockerRegistry.primaryCredentials.username:juliole}
          password: pwd      
         repositories: ${providers.dockerRegistry.primaryCredentials.repository:juliole/demolognew,juliole/spin-kub-demo}


spinnaker-local yaml:

      enabled: ${SPINNAKER_KUBERNETES_ENABLED:true}
      primaryCredentials:
      name: juliole
      address: ${SPINNAKER_DOCKER_REGISTRY:https://index.docker.io/}
      repository: ${SPINNAKER_DOCKER_REPOSITORY}
      username: ${SPINNAKER_DOCKER_USERNAME}
      # A path to a plain text file containing the user's password
      #passwordFile: ${SPINNAKER_DOCKER_PASSWORD_FILE:/opt/spinnaker/config/password}

If you want use passwordFile, need to do it on spinnaker-local yaml , and create the file via echo to avoid NL in the file:

    echo -n "AKCp2VokJkibHZTK2rygBd9pHs4Xfys2GRSryaTMe4JPuox4dRWWcHww5reK3RWcJL4Z2T7Kb" > pwd1

spinnaker-local:

    passwordFile: ${SPINNAKER_DOCKER_PASSWORD_FILE:/opt/spinnaker/config/pwd1
    clouddriver:
    passwordFile: ${providers.dockerRegistry.primaryCredentials.passwordFile}


Do not forget:\\
Additionally, in order for Docker Registry changes to be utilized as a trigger in a Spinnaker pipeline, ensure the following are available inside your /opt/spinnaker/config/igor-local.yml file:\\
dockerRegistry:\\
  enabled: true


### Jenkins:

When creating a Jenkins trigger you may get  a 429 error and it requires Igor update:

    sudo apt-get upgrade spinnaker-igor 
    sudo /opt/spinnaker/bin/reconfigure_spinnaker.sh
    sudo restart spinnaker

Another one for Jenkins:\\
For Jenkins , need to disable csrf on Jenkins 

Jenkins example:\\
<http://www.spinnaker.io/docs/hello-spinnaker >\\
<http://www.spinnaker.io/docs/troubleshooting-guide >


How to use Jenkins artifacts (it can be the BOM file):\\
- In the Jenkins job add a post build step to archive the artifacts and enter path/file like : tricorder-bom/bomfiles/dev/1.1.0.bom\\
- The properties can be reference on  other spinnaker stages like : ${cassandra}\\
- The property can be passed to other pipelines : ${ trigger['parentExecution']['context']['cassandra'] }



### Logs:
/var/log/spinnaker

### Configuration
/opt/spinnaker/config


{% include links.html %}
