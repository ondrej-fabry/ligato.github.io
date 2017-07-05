---
title: Instructions on How to Update Tricorder Code
tags: [opensource,code,devops]
keywords:
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_update_code.html
folder: mydoc
---

# Contributing Guide

Contributions are welcome and are greatly appreciated!

## Support
Tricorder uses CiscoSpark for real time support and collaboration. There is a team space on spark here: [hhttps://web.ciscospark.com/rooms/4a4a4930-3c1d-11e7-a30a-89723d139f6b/chat)<br>
E-mail [sp-cloud-native-admins@cisco.com](sp-cloud-native-admins@cisco.com) with your user ID requesting admission to the spark room

## Set up Your Environment
1. Fork the repo to your own bitbucket project and enable fork syncing. For example:
&nbsp;&nbsp;&nbsp;&nbsp;<img src="images/Fork-1.png" style="width: 500px;"/>
<img src="images/Fork-2.png" style="width: 500px;"/>
2. Clone your forked repo into development environment (i.e., local repo). Example:
&nbsp;&nbsp;&nbsp;&nbsp;```
$ git clone https://junmipan@bitbucket-eng-sjc1.cisco.com/bitbucket/scm/~junmipan/y-cache-java.git
```<br>
**_NOTE: Leave the master and release branches alone. It should be owned by the upstream repo_**.
3. In your local repo, create a branch to work on your changes. Example:<br>
&nbsp;&nbsp;&nbsp;&nbsp;```
$ git checkout -b feature/new-cache-store
```<br>
4. If a new upstream release is made available and your team wants to consume it, either create a pull request from that branch to your master or use the cli to **git pull origin release/<version\>**<br>

## Build and Versioning Convention
Project uses build and versioning convention for container image and artifact based upon release tag and commit revision.  Format:

&nbsp;&nbsp;&nbsp;&nbsp;`{major#}.{minor#}.{patch#}.{build#}`

- Major.minor.patch will all be taken from the git tag in the repo. For example:<br>
&nbsp;&nbsp;&nbsp;&nbsp;<img src="images/Tag-1.png" style="width: 700px;"/><br>
&nbsp;&nbsp;&nbsp;&nbsp;**_NOTE: Tags should be put in place purposefully to identify release checkpoint and version of your product_**.<br><br>
To create tag as new release is launched, follow the example below:
&nbsp;&nbsp;&nbsp;&nbsp;<img src="images/Tag-2.png" style="width: 700px;"/><br>
  - Check the property file _"**version.properties**"_ in the root directory of your repo.  If it does not exist yet, create it.  The file should contain a property as follows:<br>
  ```
  ARTIFACT_VERSION=<release version>
  ```<br>
  For example:<br>
  &nbsp;&nbsp;&nbsp;&nbsp;<img src="images/version.properties.png" style="width: 500px;"/><br>
  Build scripts should look up release version defined in this file, and combine it with build number (as described below) to produce the final version of the product.

- Build# will be looked up at build time using git commands, and will be **_the number of commits away from the git tag_**.  For more details on the logic and use of git commands, refer to sample build scripts as follows:
  - Container build: [build_container.sh](<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-sample-service-java/browse/build_container.sh>)
  - Java artifacts:<br>
&nbsp;&nbsp;&nbsp;&nbsp;- Gradle build: [build.gradle](<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-base-java/browse/build.gradle>)<br>
&nbsp;&nbsp;&nbsp;&nbsp;- Maven build: [tricorder-build.sh](<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-database-java/browse/tricorder-build.sh>)<br><br>
These build scripts generate version string which shall be used by (Jenkins) build job to publish the container image or artifact.

For more details, refer to [Standards for Container Image/Artifact Names and Versions](<https://cisco.jiveon.com/docs/DOC-1660928>)

## Pull Request Guidelines

Before you submit a pull request from your forked repo, check that it meets these guidelines:

1. Branches to be merged will have a name of either "feature/description" or "bugfix/description" where description is somewhat meaningful to the change. Ex: feature/vspherespinnakeradapter bugfix/awsfailedlogin
2. If the pull request adds functionality, the docs should be updated as part of the same PR.
3. If the pull request adds functionality, code in the example app that demonstrates the new functionality should be updated as part of the same PR.
4. If the pull request adds functionality, the PR description should include motivation and use cases for the feature.
5. If the pull request fixes a bug, an explanation including what the bug was, and how to reproduce it should be included in the PR description.
6. Please rebase and resolve all conflicts before submitting.

## Pull Request Steps
1. Commit changes from the root directory of your local repo to your forked repo.  For example:<br>
&nbsp;&nbsp;&nbsp;&nbsp;```$ git add .```<br>
&nbsp;&nbsp;&nbsp;&nbsp;```$ git commit -m "add a new cache store"```<br>
&nbsp;&nbsp;&nbsp;&nbsp;```$ git push```<br><br>
&nbsp;&nbsp;&nbsp;&nbsp;Some git basics for your reference:<br>
&nbsp;&nbsp;&nbsp;&nbsp;[Git Pull](https://git-scm.com/docs/git-pull)<br>
&nbsp;&nbsp;&nbsp;&nbsp;[Git Add](https://git-scm.com/docs/git-add)<br>
&nbsp;&nbsp;&nbsp;&nbsp;[Git Commit](https://git-scm.com/docs/git-commit)<br>
&nbsp;&nbsp;&nbsp;&nbsp;[Git Push](https://git-scm.com/docs/git-push)
2. Create a pull request to the upstream repo<br>
Create a pull request with all the requirements from your forked repo to the original upstream community repo's master/development branch. For example:<br>
&nbsp;&nbsp;&nbsp;&nbsp;<img src="images/PullRequest-1.png" style="width: 500px;"/><br>
<img src="images/PullRequest-2.png" style="width: 700px;"/><br>
&nbsp;&nbsp;&nbsp;&nbsp;Click **Continue** to create the pull request.<br><br>

3. Upstream reviews and approves the pull request, and merges your contributions.<br><br>
**_If you are the code creators, we will make you pull-request reviewers of the upstream repo to govern future contributions_**.
<br>
<br>
<br>

## CI / CD
To integrate your repo with CI/CD processes, you must create **Jenkins job** at<br>
<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/>

- Select the **New Item** link as shown below:<br>
<img src="images/Jenkins-NewItem.png" style="width: 700px;"/><br>
- In the new job page:<br>
  - Enter name of the new job:<br>
  <img src="images/Jenkins-newJob-1.png" style="width: 700px;"/><br>
  - The easiest way to populate initial configuration is to copy from a similar job. Scroll all the way to the bottom.  Enter the name of the job to copy from, then click **OK** to save: <br>
  <img src="images/Jenkins-newJob-2.png" style="width: 700px;"/><br>
  _**Here are some candidates to copy from, depending on the type of your product:**_
    - _For Container:_<br>
      &nbsp;&nbsp;_[Tricorder_Sample_Go_Service_Build](<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Sample_Go_Service_Build/>)_<br>
      &nbsp;&nbsp;_[Tricorder_Sample_Java_Service_Build](<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Sample_Java_Service_Build/>)_
    - _For Java artifact:_
      - _Gradle build:_ _[Tricorder_Base_Java](<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Base_Java/>)_
      - _Maven build:_ _[Tricorder_Database_Java_Maven](<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Database_Java_Maven/>)_
  - Open the configuration page of your job to modify the entries according to your product specifics.  Save the changes.
<br>
<br>
<br>

{% include links.html %}
