---
title: Spinnaker How To
tags: [spinnaker]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_spinnaker_howto.html
folder: mydoc
---

## How to start a pipeline via API

without ssh proxy:

    curl -X POST --header "Content-Type: application/json" --header "Accept: */*"  http://<spinnakerhost>:9000/gate/pipelines/<application>/<pipeline>
    
with ssh proxy:    
    
    curl -X POST --header "Content-Type: application/json" --header "Accept: */*"  http://localhost:8084/pipelines/<application>/<pipeline>


Pipeline with parameters: (example using  Postman)

     Post : http://localhost:8084/pipelines/ccmts/New Deployment - Infrastructure
     Header:    Content-Type application/json
                Accept */*
     Body: {"type":"manual","parameters":{"cCMTSName":"cmtstest1"},"user":"[anonymous]"}


    POST /pipelines/ccmts/New Deployment - Infrastructure HTTP/1.1
    Host: localhost:8084
    Content-Type: application/json
    Accept: */*
    Cache-Control: no-cache
    {"type":"manual","parameters":{"cCMTSName":"cmtstest1"},"user":"[anonymous]"}

    curl -X POST \
      http://localhost:8084/pipelines/ccmts/New%20Deployment%20-%20Infrastructure \
      -H 'accept: */*' \
      -H 'cache-control: no-cache' \
      -H 'content-type: application/json' \
      -H 'postman-token: fef51358-b96b-ff63-2b9c-4230111585cd' \
      -d '{"type":"manual","parameters":{"cCMTSName":"cmtstest1"},"user":"[anonymous]"}'





## How to setup Notification

in /opt/spinnaker/config/spinnaker-local.yml:

        notifications:
          # The following blocks can enable Spinnaker to send notifications
          # using the corresponding mechanism.
          # See http://www.spinnaker.io/docs/notifications-and-events-guide
          # for more information.
          mail:
            enabled: true
            host: outbound.cisco.com
            fromAddress: jusilvei@cisco.com

in /opt/spinnaker/config/settings.js:
    
        // var emailEnabled = ${services.echo.notifications.mail.enabled};


run the following command that will update /opt/deck/html/settings.js:

       sudo /opt/spinnaker/bin/reconfigure_spinnaker.sh

Restart spinnaker

Notification can then be set on the UI : Application -> Configuration
 
Spinnaker documentation is not updated, but can be used as reference : <http://www.spinnaker.io/docs/notifications-and-events-guide>



## How to create a Kubernetes service via pipeline

Spinnaker does not have stage type to create a service. It needs to be done editing the pipeline json. \\
Example:\\

    {
      "appConfig": {},
      "executionEngine": "v2",
      "keepWaitingPipelines": false,
      "lastModifiedBy": "anonymous",
      "limitConcurrent": true,
      "parallel": true,
      "stages": [
        {
          "account": "admin",
          "application": "app",
          "availabilityZones": {
            "default": [
              "default"
            ]
          },
          "cloudProvider": "kubernetes",
          "clusterIp": "",
          "detail": "test",
          "externalIps": [],
          "loadBalancerIp": "",
          "loadBalancerName": "app-test-pipeline",
          "name": "Upsert Load Balancer",
          "namespace": "default",
          "ports": [
            {
              "name": "http",
              "port": 80,
              "protocol": "TCP"
            }
          ],
          "provider": "kubernetes",
          "serviceAnnotations": {},
          "serviceType": "ClusterIP",
          "sessionAffinity": "None",
          "stack": "pipeline",
          "type": "upsertLoadBalancer",
          "user": "anonymous"
        }
      ],
      "triggers": [],
      "updateTs": "1492612546204"
    }

Caveats:
It was added to Spinnaker on 4/20, so you need a recent build\\
There issues adding stages after the upsertLoadBalancer. It may be better to add a pipeline to create services. \\
To have server group created in the same run as service, you will need to enter the load balancer name in json on the deploy stage.
  
## How to add node selector

Server groups have the option to add node selector. Check labels at kubernetes-> nodes to see what can be used.  \\
Jobs do not have this option in the UI. Need to edit json like in the following examples:


          "nodeSelector": {
            "kubernetes.io/hostname": "ip-10-43-0-20"
          },
          
          
          "name": "Run Job",
          "namespace": "ccmts-prod-1",
          "nodeSelector": {
            "kubeadm.alpha.kubernetes.io/role": "master"
          },
          "refId": "1",
               
## How to set hostNetwork

Spinnaker UI does not support hostNetwork. It needs to be added directly in the pipeline json:

          "hostNetwork": true


## How to export and import pipelines from one Spinnaker environment to another

One of the options is to use the front50 API. Make sure when you ssh proxy to spinnaker you have port 8080 : -L 8080:localhost:8080 \\

Known issue: When pipelines are imported you need to manually create the application in the target system. Application name does not get imported\\

### Export Pipelines:

    curl -X GET --header "Accept: */*" http://localhost:8080/pipelines/mytest | json_pp > pipelines.json

### Import Pipelines: 

    curl -X POST --header "Content-Type: application/json" --header "Accept: */*" -d @pipelines.json http://localhost:8080/pipelines/batchUpdate 

### Useful Spinnaker expressions

Parameter from trigger pipeline (use the parent parameter when using it on pipeline parameter):

    ${ trigger['parentExecution']['trigger']['parameters']['Tenant'] }

Other stages can have:

    ${ execution['trigger']['parameters']['Tenant'] }
    
    
pipeline parameters:
    
    ${ parameters['cCMTSName'] }
    "tag": "${parameters['cmmgmtversion'] }"
    

## How to use Jenkins configuration file in Spinnaker. A use case is the BOM file.

The configuration file needs to be archived in Jenkins. In the Jenkins job add a post build action to archive artifacs and add the path for the file. In the case of the Tricorder BOM file it is tricorder-bom/bomfiles/$DEVELOPMENT_PHASE/$BOM_VER.bom  \\
In Spinnaker, on a Jenkins stage,  add only the file name in the field "Property File", like for example : 1.1.0.bom \\
The key/value pairs in the configuration file will be set as parameters in Spinnaker and can be used on later stages. Here is an example of an image version from the BOM used on the json file:

                "imageId": "dockerhub.cisco.com/tric_vpp_vnf_agent:${ tric_vpp_vnf_agent.substring(tric_vpp_vnf_agent.indexOf(':') + 1, tric_vpp.length()) }",
                "registry": "dockerhub.cisco.com",
                "repository": "vnf-docker-dev/tric_vpp_vnf_agent",
                "tag": "${ tric_vpp_vnf_agent.substring(tric_vpp_vnf_agent.indexOf(':') + 1, tric_vpp_vnf_agent.length()) }"
              },

Here is another example of the properties from a pipeline trigger: \\

               "imageId": "dockerhub.cisco.com/tric_sample_go_service:${ trigger['parentExecution']['context']['tric_sample_go_service'].substring(trigger['parentExecution']['context']['tric_sample_go_service'].indexOf(':') + 1, trigger['parentExecution']['context']['tric_sample_go_service'].length()) }",
                "registry": "dockerhub.cisco.com",
                "repository": "vnf-docker-dev/tric_sample_go_service",
                "tag": "${ trigger['parentExecution']['context']['tric_sample_go_service'].substring(trigger['parentExecution']['context']['tric_sample_go_service'].indexOf(':') + 1, trigger['parentExecution']['context']['tric_sample_go_service'].length()) }"
              },

Spinnaker documentation has a good explanation on use of property files:\\
<http://www.spinnaker.io/docs/pipeline-expressions-guide#section-property-files>


## How to access Spinnaker console directlly without ssh proxy


### Installation without hal (old) :

Edit spinnaker configuration file:

    sudo vi /etc/apache2/sites-enabled/spinnaker.conf  :

        <VirtualHost 0.0.0.0:9000>
          DocumentRoot /opt/deck/html
        
          ProxyPass "/gate" "http://localhost:8084" retry=0
          ProxyPassReverse "/gate" "http://localhost:8084"
        
          <Directory "/opt/deck/html/">
            Require all granted
          </Directory>
        </VirtualHost>

Edit appache ports.conf:

    sudo vi /etc/apache2/ports.conf:

        Listen 9000
        
        <IfModule ssl_module>
                Listen 443
        </IfModule>
        
        <IfModule mod_gnutls.c>
                Listen 443
        </IfModule>


restart apache:

    sudo service apache2 restart



Edit deck :

    sudo vi /opt/spinnaker/config/spinnaker-local.yaml :

      deck:
        # Frontend configuration.
        # If you are proxying Spinnaker behind a single host, you may want to
        # override these values. Remember to run `reconfigure_spinnaker.sh` after.
        baseUrl: http://<IP>:9000
        gateUrl: ${services.deck.baseUrl}/gate
        #bakeryUrl: ${services.deck.baseUrl}/rosco
        auth:
          enabled: false


Execute:

        sudo /opt/spinnaker/bin/reconfigure_spinnaker.sh

Restart Spinnaker :

        restart spinnaker

### Installation with hal

Go to ~/.hal/default/service-settings and create files gate.yml and deck.yml with entry: host: 0.0.0.0

    sudo hal deploy apply

    hal config security ui edit --override-base-url http://<IP>:9000

    hal config security api edit --override-base-url http://<IP>:8084

    sudo hal deploy apply

After Spinnaker starts go to http://localhost:9000



## How to setup a Bitbucket trigger:

Follow Spinnaker documentation : <http://www.spinnaker.io/docs/notifications-and-events-guide>

For our EngIt bitbucket we need to follow the instructions for Stash. In the Bitbucket repo select settings -> Hooks and enable Post-Receive WebHooks. URL : <http://spinnakerIP:9000/gate/webhooks/git/stash> (without proxy) or <http://localhost:8084/gate/webhooks/git/stash>

In Spinnaker add a git trigger, select stash as repo type and TRIC as project (if you are using the Tricorder project)

Note that the trigger will just start the pipeline on pushes, but it will not have that much info available besides commit hash : 
    
    ${ execution['trigger']['hash'] }



## How to get a file from Bitbucket - Webhook Stage

One of the possible uses of the Webhook stage is to get Git/Bitbucket info.

The following will get back the bom file for example:

Webhook URL:
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/rest/api/1.0/projects/TRIC/repos/tricorder-bom/browse/bomfiles/dev/1.1.0.bom?json&at=master>


## How to get back error reason in RunJobs stage

<p align="justify">In Kubernetes every message directed towards stderr stream are redirected to default log file. The default file path is  <b>“/dev/termination-log”</b> which is determined by “terminatingMsgPath” property.  This path is configurable but then while redirecting you have to use this new file path. 
So if you want to get back the error reason from your pods container script to spinnaker UI , you have to redirect the error message  to  /dev/termination-log instead of redirecting to  &2 which is Error File descriptor.</p>


Example: (in Container script)

echo “Error Message from Container” > /dev/termination-log; 
exit 1        

Result:

&nbsp;&nbsp;&nbsp;&nbsp;<img src="images/ErrorMsg.JPG" style="width: 500px;"/><br>


{% include links.html %}
