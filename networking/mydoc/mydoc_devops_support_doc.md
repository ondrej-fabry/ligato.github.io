---
title: Devops Support Documentation
tags: [devops]
keywords: devops
last_updated: March 26, 2017
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_devops_support_doc.html
folder: mydoc
---

# Git Branching Strategy and basic use:

Reference
[Git - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging) \\
GitTag format: v{major#}.{minor#}.{patch#} \\
    Example "v1.5.4"\\
JiraID format: SPOPT-### \\
DevOps architecture slides - Pipeline & Promotion Process section: \\
<https://cisco.box.com/s/kwpuu2p4gd59rmta5jjwvx5q911co57o>\\\\
WebexTraining: \\
Branching strategy:\\
[Play recording](https://cisco.webex.com/ciscosales/lsr.php?RCID=27e6eb326a1f45e78e4fefda3fa130b0) (27 min 39 sec) \\
Recording password: aD6E22JC \\
General Git Use: \\
[Play recording](https://cisco.webex.com/ciscosales/lsr.php?RCID=e66465666eb34140b43ea1792e711e1a)(30 min 8 sec) \\
Recording password: 8JhBDnHv\\

## Overview

Our goal is to reference everything in the environment in a Bill Of Materials (BOM).\\

### BOM content:

    * environment_ver - This is a version tag to represent the environment as a whole. It is used to tag images
    * Tricorder-bootstrap branch
      * BOOTSTRAP_CHECKOUT_REF - The git branch, commit guid or tag to pin automation to
      * COMPONENT_DEPLOYMENT_CHECKOUT_REF - The git branch, commit guid or tag to pin automation to
      * SERVICE_DEPLOYMENT_CHECKOUT_REF - The git branch, commit guid or tag to pin automation to
        For the Integration PipelinePhase it is a commit head guid substring that pins the BOM to a specific revision of the bootstrap automation in the git repo
        For the devops and possibly dev Pipeline phases it is a feature branch
    * Tricorder Component Images - full path to images with their tags
    * Container Images versioned by tag - full path to images with their tags

## Assumptions

The git repo has a master branch that is locked to where only pull requests can allow merges. See: Applying a branch policy

## Dev Instructions:

### Create new feature/bugfix branch

    git checkout -b feature/SPOPT-###{someshortdesc}
    or
    git checkout -b bugfix/SPOPT-###{someshortdesc}
    
### Make changes, Use "git add" and "git commit" - be sure to add Jira ID to the commit

    git commit -m "SPOPT-### this or that change"
    git push
### More Git Help here:

[Git Common Scenarios](https://cisco.jiveon.com/docs/DOC-1572967)\\

### Execute Devtest deploy tests etc in your own pipeline and build from this new branch

### Create a pull request in the bitbucket web ui from your feature/bugfix branch to master
{% include image.html file="devops_support_bitbucket1.png"  %}

### Select the source (your branch) and destination (master)
{% include image.html file="devops_support_bitbucket2.png"  %}

### Have a peer review and approve the request and merge

### OPTIONAL - Only do this step if a new version tag is needed for the repo

    New versions(git tags) are only needed when we want to represent significant changes in code. In most cases this will not be necessary and if subsequent merges/pull requests come in and the team does not wish to increment the major/minor/patch tag, the build will append a .{build#} to the end based on the # of commits past the tag.
     * example: 5 merges to master after the latest tag of v1.0.2 will build a container with a tag of v1.0.2.5
    New tags will always increase in count. We should never go down in version numbers
    
    Tagging instructions:
    Add the appropriate version tag using the format mentioned above to the commit/merge in the bitbucket web ui.
    * Select the commit/merge. See image bitbucket1 below
    * Add  the tag on the right hand side of the UI with the "plus". See image bitbucket2 below
       
    The BOM in the dev environment is automatically updated with the new image and tag as part of the build and validation steps.
    The deploy jobs will use the BOM to get the image and tag information
  {% include image.html file="devops_support_bitbucket3.png" caption="bitbucket1"  %} 
  {% include image.html file="devops_support_bitbucket4.png" caption="bitbucket2"  %}
    


## DevOps Instructions
When building services, the  BOM repo must be a sub module of the service repo. Simply copy wither the contents or the full .gitmodules file from the helloworld service\\
Contents: <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/SPOP/repos/helloworldservice/browse/.gitmodules>\\
Work with dev to add or review the appropriate GitTag format (v{major#}.{minor#}.{patch#}) if needed to\\
Ensure the git repo master branches are locked - [Applying branch policies and policy exceptions in Git](https://cisco.jiveon.com/docs/DOC-1514374)\\
Ensure the git repo requires JiraIDs with commit messages - [Enforce JIRA ID in Git Commits](https://cisco.jiveon.com/docs/DOC-1547530)\\
Use the helloworld service build job as a point of reference for job steps. \\                    <https://engci-private-sjc.cisco.com/jenkins/spo/job/Team-DevOps/job/Hello-World_BuildImage_script/>\\
Services should build from the master branch and have appropriate submodule behaviors selected
       {% include image.html file="devops_support_bitbucket5.png"  %}
\\

## Applying a branch policy

* Open the repository in bitbucket (<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/project/-repos-/-repository->) and click the settings gear.

 {% include image.html file="devops_support_bitbucket6.png"  %}

* Click on the Branch permissions menu
* Click on Add permissions
 {% include image.html file="devops_support_bitbucket7.png"  %}
* Select how you want to select the branch to restrict from the Branches drop down menu. You can specify the branch in three ways:
  * Option 1 - Restricting an explicitly named branch\\
 {% include image.html file="devops_support_bitbucket8.png"  %} 
  * Option 2 - Restricting a branch using a pattern (wildcards)\\
 {% include image.html file="devops_support_bitbucket9.png"  %}
  * Option 3 -  Restricting a branch based on branch types\\
 {% include image.html file="devops_support_bitbucket10.png"  %}
* In the Limit write access to field, you can optionally define the people who are allowed to write to that branch. This is typically used when a branch has a maintainer that has to approve and merge code (such as production)
* In the Prevent check box list, select the appropriate restrictions:
     * Rewrite history: Do not permit somebody to use git to change code that has previously been pushed into the repository. (yes, git allows such a thing) [Recommended]
     * Changes without a pull request: Code cannot be pushed directly to the branch. It must be developed on a side branch and merged in using a Stash pull request (code review) [Recommended, except on development branches obviously]
     * Branch deletion: Do not permit somebody to delete this branch. [Recommended]
* Click Create
 
 
## Applying a branch policy exception
 
 When a policy is applied to a branch, it is sometimes necessary to define exceptions to accommodate the need for the build or other automation to push updates to the branch automatically (for example, adding build tags, updating a Terraform state file, saving a build log, etc)
  
 For example, prevent changes without a pull request for everybody except the automation user. The policy them becomes:
  
 branch: master - policy: Prevent changes without a pull request - Applies to: Everyone EXCEPT automation user
 
 *****

# Setup Code Review Policies in Bitbucket

## Code Review Basics Video
[Play recording](https://ciscosupport.webex.com/ciscosupport/lsr.php?RCID=ed4bbe4aa2114e53b5c1f733688bfc65) (18 min 31 sec)

## Applying a branch policy
* Open the repository in bitbucket (<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/<project>/repos/-repository->) and click the settings gear.
 {% include image.html file="devops_support_bitbucket_cr1.png"  %}
* Click on the Branch permissions menu
* Click on Add permissions
 {% include image.html file="devops_support_bitbucket_cr2.png"  %}
* Select how you want to select the branch to restrict from the Branches drop down menu. You can specify the branch in three ways:
  * Option 1 - Restricting an explicitly named branch
 {% include image.html file="devops_support_bitbucket_cr3.png"  %}
  * Option 2 - Restricting a branch using a pattern (wildcards)
   {% include image.html file="devops_support_bitbucket_cr4.png"  %}
  * Option 3 - Restricting a branch based on branch types
 {% include image.html file="devops_support_bitbucket_cr5.png"  %}
 
* In the Limit write access to field, you can optionally define the people who are allowed to write to that branch. This is typically used when a branch has a maintainer that has to approve and merge code (such as production)
* In the Prevent check box list, select the appropriate restrictions:
    * Rewrite history: Do not permit somebody to use git to change code that has previously been pushed into the repository. (yes, git allows such a thing) [Recommended]
    * Changes without a pull request: Code cannot be pushed directly to the branch. It must be developed on a side branch and merged in using a Stash pull request (code review) [Recommended, except on development branches obviously]
    * Branch deletion: Do not permit somebody to delete this branch. [Recommended]
* Click Create
 
## Setting Number of Approvers
 In the repository settings, select Pull Requests underneath the Workflow section. Check the "Requires N approvers and set a minimum per code review standards.
  {% include image.html file="devops_support_bitbucket_cr6.png"  %}
 
## Applying a branch policy exception
 When a policy is applied to a branch, it is sometimes necessary to define exceptions to accommodate the need for the build or other automation to push updates to the branch automatically (for example, adding build tags, updating a Terraform state file, saving a build log, etc)\\
  
 For example, prevent changes without a pull request for everybody except the automation user. The policy them becomes:\\
  
 branch: master - policy: Prevent changes without a pull request - Applies to: Everyone EXCEPT automation user
 
{% include links.html %}
