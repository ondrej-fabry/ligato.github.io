---
title: Spinnaker Development Getting Started
tags: [spinnaker]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_spinnaker_dev.html
folder: mydoc
---

## Setup on Mac OSX
The Spinnaker project has prerequisites and directions for setting up your Mac OS X environment for development.  In addition to what is there, the following may help.
 
1. You need a GitHub account if you don’t already have one.  Once you sign up, you need to follow the directions for Adding a new SSH key to your GitHub account.

2. You need to a Kubernetes cluster to deploy containers to.  If you want to run Kubernetes locally, you can install Minikube.  The Minikube project has prerequisites, a link to directions for installing Minikube, and a quickstart demo to show how to use it.

3. After installing Redis and Cassandra using homebrew, you can start them and configure them to run automatically when you login with the following commands:

        $ brew services start homebrew/versions/cassandra21
        $ brew services start redis

4.	spinnaker/dev/run_dev.sh without parameters starts all the spinnaker services, except the UI (deck)!  You have to run it separately.  And you may also have to start the API gateway (gate):

            $ ../spinnaker/dev/run_dev.sh deck
            $ ../spinnaker/dev/run_dev.sh gate

## Setup on Ubuntu 14.04
[The Spinnaker project](https://github.com/spinnaker/spinnaker) has prerequisites and directions for setting up your Linux environment for development.  In addition to what is there, the following may help.
 
1. You can setup Ubuntu on Oracle VirtualBox. I used ubuntu-14.04.5-desktop-amd64.iso
2. Create an account named spinnaker (optional but it is the expected default)
3. Install Git and SSH server (sudo apt-get install openssh-server)
4. You need a GitHub account if you don’t already have one.  Once you sign up, you need to follow the directions for [Adding a new SSH key to your GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
5. You need to a Kubernetes cluster to deploy containers to.  You can also point to a k8s environment on AWS (it is what I did) or If you want to run Kubernetes locally, you can install Minikube.  The [Minikube project](https://github.com/kubernetes/minikube) has prerequisites, a link to directions for installing Minikube, and a quickstart demo to show how to use it. 
6. You do not need to install Redis and Cassandra manually. The bootstrap script will do it. 
7. Follow the instructions from https://github.com/spinnaker/spinnaker . Make sure folder and files are created using the account you created and logged in, not root.
8. You will need to configure at least connections to K8s and Docker.
9. To install Eclipse in the same VM, install Neon and Groovy plug ins: Follow session “How to Install” on <https://github.com/groovy/groovy-eclipse/wiki>

## End-to-end Source to Kubernetes Pipeline

Follow this codelab to get an end-to-end deployment working.  You configure a source repo on GitHub with a small microservice, this triggers a container build on DockerHub, which triggers a pipeline on Spinnaker to deploy the container to your kubernetes.

•	<http://www.spinnaker.io/docs/kubernetes-source-to-prod>

Note: You must enable the Igor service in your spinnaker-local.yml – Igor is the service that triggers pipelines based on DockerHub events.  And you must disable Jenkins in spinnaker-local.yml or Igor won’t start - Jenkins enables automatically if Igor is enabled, so you must set it to false to disable it (or if you are properly configured to connect to Jenkins, this will be a non-issue.)  Once it is configured, you may have to start igor manually

        $ ../spinnaker/dev/run_dev.sh igor

## Troubleshooting issues

### Cassandra problems
I had problems getting Cassandra to work with Spinnaker.  There were some errors with keys not found when the services would start.  It may be working now, but if you have trouble too, you can get it running without Cassandra.  Just switch these services to use redis instead. Make sure you disable Cassandra and enable redis on the spinnaker-local.yml that you created or copied to $HOME/spinnaker/.spinnaker

•	<http://www.spinnaker.io/docs/echo-cassandra-to-in-memory> \\
•	<http://www.spinnaker.io/docs/front50-cassandra-to-redis>
 
### Tomcat exceptions in front50
Exception in thread "main" org.springframework.context.ApplicationContextException: Unable to start embedded container; nested exception is org.springframework.boot.context.embedded.EmbeddedServletContainerException: Unable to start embedded Tomcat

Sometimes old spinnaker processes do not die and they interfere with current processes.  You need to manually kill the zombie processes.
 
This should stop all processes \\
           ` $ ../spinnaker/dev/stop_dev.sh`
 
…except the UI that you have to start/stop manually, so stop that as well \\
           ` $ ../spinnaker/dev/stop_dev.sh deck`
 
Now check to see if any zombie spinnaker processes are still running: \\
           ` $ ps –ef | grep spinnaker`
 
If you see any, kill them \\
          ` $ kill <pid>`
 
Now try running spinnaker again with run_dev.sh.

## Development on the clouddriver project

The clouddriver project is organized as a multi-project gradle build.  You can see how the many subprojects are organized by using this gradle command in the clouddriver project directory:\\
              ` $ ./gradlew -q projects`

You can use Eclipse or Intellij IDEA if you like.  The gradle build script can generate project files for both.

If using Intellij IDEA, first run the command to create the idea project files\\
` $ ./gradlew idea `

Then start Intellij IDEA, and in the project window click Open, select the clouddriver top level folder and click open.  Intellij will start the gradle build of the full project (you can watch the progress in the bottom status bar).

You need to make sure gradle is set to use auto-import.  Under Intellij -> Preferences, navigate to Build, Execute, Deployment > Build Tools > Gradle and make sure Use auto-import is checked.

Once the project is built, you can configure the clouddriver to run from the IDE.  Select the Run menu, then Edit Configurations.   In the dialog, click the + in the top left to create a new configuration and select “Application” as the type (or if you have Intellij Ultimate, you can select Spring Boot).  The configuration should look like this:

Name: clouddriver\\
Main class: com.netflix.spinnaker.clouddriver.Main\\
VM options: \\
    -Dspring.config.location=./config/,/path/to/spinnaker/config/,/your/home/dir/.spinnaker/
Use classpath of module: clouddriver-web_main

/path/to/spinnaker/config/ is the path to the spinnaker repository’s config directory.

/your/home/dir/.spinnaker is the path to the location of your spinnaker-local.yml, for example /Users/dsimonto/.spinnaker/.

Save it, Debug and it should start running in the IDE.


## Contributing to Spinnaker Open Source

Create a fork of the repository you are going to make contributions to on GitHub.com.  The original repository is the “upstream” repository.

There is no option on GitHub to keep a fork merged automatically, so over time your fork will become outdated as the upstream repository keeps changing.  Make sure you [sync your fork with the upstream](https://help.github.com/articles/syncing-a-fork/) repository before you commit contributions that you want to submit, otherwise the upstream maintainers may not be able to merge your contributions.

Example: Syncing the dsimonto fork of the Spinnaker repo
Clone your fork (dsimonto is my Github account where I created my fork of Spinnaker)\\
        `$ git clone git@github.com:dsimonto/spinnaker.git
         $ cd spinnaker`

List the configured remotes
`$ git remote -v
origin	git@github.com:dsimonto/spinnaker.git (fetch)
origin	git@github.com:dsimonto/spinnaker.git (push)`


I need to add the upstream remote
`$ git remote add upstream git@github.com:spinnaker/spinnaker.git
$ git remote -v
origin	git@github.com:dsimonto/spinnaker.git (fetch)
origin	git@github.com:dsimonto/spinnaker.git (push)
upstream	git@github.com:spinnaker/spinnaker.git (fetch)
upstream	git@github.com:spinnaker/spinnaker.git (push)`

Fetch the upstream branches and commits.  Commits to the upstream’s master will be stored in a local branch upstream/master.  There may be additional upstream branches.\\
`$ git fetch upstream`
…
 From github.com:spinnaker/spinnaker \\
* [new branch]      master                        -> upstream/master

Check out your fork’s local master branch \\
`$ git checkout master` \\
 Your branch is up-to-date with 'origin/master'.

Merge the changes from upstream/master into your local master branch.   This syncs your local master with the remote master.  You shouldn’t have local changes to your master, so Git will perform a “fast-forward”.\\
`$ git merge upstream/master` \\
Updating aa561b2..66d0952 \\
Fast-forward \\
 dev/build_release.py  | 154 ++++++++++++++++++++++++++++++++++----------------------------------- \\
 dev/refresh_source.py |   8 +++- \\
 2 files changed, 82 insertions(+), 80 deletions(-) \\
 mode change 100644 => 100755 dev/build_release.py

Then you need to push the changes to your remote fork on GitHub\\
`$ git push origin master`\\
To github.com:dsimonto/spinnaker.git\\
   aa561b2..66d0952  master -> master

At this point the master branch on your GitHub fork should be even with the Spinnaker repo:

 {% include image.html file="spinnakerdev1.png" %}


Now create a branch to work on your feature or bugfix.\\
`$ git checkout -b bugfix/run-script-updates`\\
Switched to a new branch 'bugfix/run-script-updates'

Make your changes, test and commit them with a meaningful message.  Here I just fixed a bug in one file, refresh_source.py:\\
`$ git add dev/refresh_source.py\\
$ git commit -m "Make the update_run_scripts default to True.  Added noupdate_run_scripts to disable it."`

Create the branch on your GitHub fork and link this local branch with the new remote one.\\
`$ git push -u origin bugfix/run-script-updates`
…
To github.com:dsimonto/spinnaker.git\\
 * [new branch]      bugfix/run-script-updates -> bugfix/run-script-updates\\
Branch bugfix/run-script-updates set up to track remote branch bugfix/run-script-updates from origin.

Now back on GitHub.com, go to your fork and you should see a “Compare & pull request” button for the branch you just committed.  Click that, and make sure the base fork is the upstream master, and the head fork is your spinnaker fork on your new branch.  Add your comments, then click “Create pull request”:

  {% include image.html file="spinnakerdev2.png" %}
  
Once the pull request is created, you can track it in the upstream repository’s Pull Requests tab.  It’s up to the maintainers to review and merge your change into their master branch.  Once they merge your code, you will see it in the pull request status and the commit history of the upstream repository:

  {% include image.html file="spinnakerdev3.png" %}

After the maintainers of the upstream repo merge your fix to their master, you can merge their master into your fork’s master:
`$ git checkout master			<-- switch local branch to your master
 $ git fetch upstream				<-- get the latest changes to the upstream remote
 $ git merge upstream/master			<-- merge those changes to your master
 $ git push origin master			<-- push the changes to your remote fork`

Now that your fixed is merged, you no longer need the branch you created.  GitHub gives you a Delete Branch button at the end of the merged pull request.  Click that button, and the branch will be deleted on your GitHub fork (note: this screenshot is from a different fix)
 
  {% include image.html file="spinnakerdev4.png" %}

Then you need to remove the obsolete branch from your local repository with the -p, prune, option to git fetch:\\
`$ git fetch origin -p`\\
From github.com:dsimonto/spinnaker\\
 - [deleted]         (none)     -> origin/bugfix/run-script-updates



{% include links.html %}
