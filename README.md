# Landing Page Jekyll theme

Jekyll theme based on [landing-page bootstrap theme ](http://startbootstrap.com/templates/landing-page/)

## How to use
 - Place a image in `/img/services/`
 - Create posts to display your services. Use the follow as an example:

```txt
---
layout: default
img: ipad.png
category: Services
title: The service title
---
The description of this service
```

## Demo
View this jekyll theme in action [here](https://swcool.github.io/landing-page-theme)

## Screenshot
![screenshot](https://raw.githubusercontent.com/swcool/landing-page-theme/master/img/screenshot.png)

===

For more Jekyll details, read [documentation](http://jekyllrb.com/).
This Jekyll theme used [Freelancer Jekyll theme](https://github.com/jeromelachaud/freelancer-theme/) as reference.

## License
The contents of this repository are licensed under the [Apache
2.0](http://www.apache.org/licenses/LICENSE-2.0.html).

## Version
1.0.1


# How to edit header titles

Header title are configurable, and you can edit/add title in `_config.yml.`


![screenshot]({{home}}/img/read1.png)


## How to edit/add new title? 

GIT hub link:
https://github.com/ligato/ligato.github.io/blob/master/_config.yml

You can add/edit in section   headertitles:
Example:

```bash

headertitles:

```txt
 - title: New title
```
   url:  Path to this title   example :   networking/
   type: link
```


Note:  Keep the value for type as link only. It can have values like link or scroll. For new header you have to create a new folder with same name as title and follow the markdown files template and structure.

How to add/edit the left navigation Tree in Docs page

To add to the left navigation tree you simply have to add new values in “docpages_sidebar.yml” file. 

GIT hub link:
https://github.com/ligato/ligato.github.io/blob/master/_data/sidebars/docpages_sidebar.yml
Left tree:


![screenshot]({{home}}/img/read2.png)


## Adding new Branch:

```bash
```txt
  - title: Your Branch One
    output: web, pdf
    folderitems:

    - title: Sub Branch 1
      url: /mydoc_standardization.html
      output: web, pdf
      type: homepage
```
```


To above content add in this way:

```bash
```txt
   -title:  New Branch You Added
    output:     web
    folderitems:

    - title: Sub Branch You added
      url: /FilePath.html
      output: web
```
```

You can add sub branch like this:


```bash
```txt
   -title:  New Branch You Added
    output:     web
    folderitems:

    - title: Sub Branch You added
      url: /FilePath.html
      output: web
    - title:  New Sub Branch you can add
      url: /NewFilePath.html
      output: web
```
```


For URL, you require html file for which you need to create a file in .md format and place in pages/mydoc folder. 

You can download and use the following template file to create a .md file.

GitHub Link:
https://github.com/ligato/ligato.github.io/blob/master/pages/mydoc/mydoc_index.template


# How to edit the networking page

We have to follow the same procedure as if we did for editing the docs pages. But here is folder name is   network/
And left tree data is stored in the file name as  _data/sidebars/mydoc_sidebar.yml

## Git Hub link:
https://github.com/ligato/ligato.github.io/blob/master/_data/sidebars/mydoc_sidebar.yml


# For title on image

For  title like Networking:


![screenshot]({{home}}/img/read3.png)
 
For image title, you have to add additional property in the header i.e. foldertitle

```bash
```txt
---
overview: Information on the various ways to participate and interact with the Ligato community.
layout: pages
type: markdown
sidebar: docpages_sidebar
foldertitle: Ligato Docs
---
```
```

Note: All the content in .md files should be written as same we did in our tricorder docs page.

Link to tricorder pages: 
https://wwwin-github.cisco.com/pages/tricorder/index.html

