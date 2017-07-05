---
title: Instructions on How to Update Tricorder Documentation
tags: [documentation]
keywords:
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_update_doc.html
folder: mydoc
---

## Introduction
This site uses a Jekyll template. Jekyll is a popular static site generator that is embedded on Git. The website is rebuilt on every check in. 
You do not need Jekyll installed to create and update content, but you can set up a local version of Jekyll to preview the changes and help troubleshoot failed Jekyll builds.

## Support
Tricorder uses CiscoSpark for real time support and collaboration. There is a team space on spark here: [https://web.ciscospark.com/teams/f82e8c00-003b-11e7-9dd2-fffe351f5a21](https://web.ciscospark.com/teams/f82e8c00-003b-11e7-9dd2-fffe351f5a21)<br>
E-mail [tricorder-admins@cisco.com](tricorder-admins@cisco.com) with your user ID requesting admission to the spark room

## Content Editors
Use a text editor such as Sublime Text, WebStorm, IntelliJ, or Atom to create pages. Atom is recommended because it’s created by Github, which is driving some of the Jekyll development through Github Pages


## Get Started - Clone the code
In order to make documentation update you will need to clone <https://wwwin-github.cisco.com/tricorder/tricorder.wwwin-github.cisco.com>


## The main config file
File _config.yml in the root folder contains generic information about the project. We should avoid changing it but it is nice to know its content.


## Side Navigation
File _data/sidebars/mydoc_sidebar.yml uses a specific YAML syntax that must be followed to present the side bar options.

Each `folder` or `subfolder` must contain a `title` and `output` property. Each `folderitem` or `subfolderitem` must contain a `title`, `url`, and `output` property.

The two outputs available are `web` and `pdf`. (Even if you aren't publishing PDF, you still need to specify `output: web`).

The YAML syntax depends on exact spacing, so make sure you follow the pattern shown in the sample sidebars.

The html file in the URL property must have the same name as a .md file at /pages/mydoc

## Top Navigation
File _data/topnav.yam uses a specific YAML syntax that must be followed to present the top navigation options


## Pages
Most of the new content and updates will be done on .md files on the /pages/mydoc folder.
Every URL in the side navigation _data/sidebars/mydoc_sidebar.yml must match a page file.

### Frontmatter
Each page must have a frontmatter at the top like this:


```yaml
title: Container Networking - Calico
tags: [network]
keywords:
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_container_net_calico.html
folder: mydoc
```

Frontmatter is always formatted with three hyphens at the top and bottom. Your frontmatter must have a `title` and `permalink` value. All the other values are optional.

Note that you cannot use variables in frontmatter.

The following table describes each of the frontmatter that you can use with this theme:

| Frontmatter | Required? | Description |
|-------------|-------------|-------------|
| **title** | Required | The title for the page |
| **tags** | Optional | Tags for the page. Make all tags single words, with underscores if needed (rather than spaces). Separate them with commas. Enclose the whole list within brackets. Also, note that tags must be added to \_data/tags_doc.yml to be allowed entrance into the page. This prevents tags from becoming somewhat random and unstructured. You must create a tag page for each one of your tags following the pattern shown in the tags folder. (Tag pages aren't automatically created.)  |
| **keywords** | Optional | Synonyms and other keywords for the page. This information gets stuffed into the page's metadata to increase SEO. The user won't see the keywords, but if you search for one of the keywords, it will be picked up by the search engine.  |
| **last_updated**  | Optional | The date the page was last updated. This information could helpful for readers trying to evaluate how current and authoritative information is. If included, the last_updated date appears in the footer of the page in rather small font.|
| **summary** | Optional | A 1-2 word sentence summarizing the content on the page. This gets formatted into the summary section in the page layout. Adding summaries is a key way to make your content more scannable by users. The only drawback with summaries is that you can't use variables in them. |
| **permalink**| Required | The permalink *must* match the filename in order for automated links to work. Additionally, you must include the ".html" in the filename. Do not put forward slashes around the permalink (this makes Jekyll put the file inside a folder in the output). When Jekyll builds the site, it will put the page into the root directory rather than leaving it in a subdirectory or putting it inside a folder and naming the file index.html. Having all files flattened in the root directory is essential for relative linking to work and for all paths to JS and CSS files to be valid. |
| **datatable** | Optional | 'true'. If you add `datatable: true` in the frontmatter, scripts for the [jQuery Datatables plugin](https://www.datatables.net/) get included on the page. You can see the scripts that conditionally appear by looking in the \_layouts/default.html page. |
| **toc** | Optional | If you specify `toc: false` in the frontmatter, the page won't have the table of contents that appears below the title. The toc refers to the list of jump links below the page title, not the sidebar navigation. You probably want to hide the TOC on the homepage and product landing pages.|

### Markdown or HTML format

Pages can be either Markdown or HTML format (specified through either an .md or .html file extension).

If you use Markdown, you can also include HTML formatting where needed. But if you're format is HTML, you must add a `markdown="1"` attribute to the element in order to use Markdown inside that HTML element:

```
<div markdown="1">This is a [link](http://exmaple.com).</div>
```

For your Markdown files, note that a space or two indent will set text off as code or blocks, so avoid spacing indents unless intentional.

If you have a lot of HTML, as long as the top and bottom tags of the HTML are flush left in a Markdown file, all the tags inside those bookend HTML tags will render as HTML, regardless of their indentation. (This can be especially useful for tables.)

### Kramdown Markdown

Kramdown is the Markdown flavor used in the theme. This mostly aligns with Github-flavored Markdown, but with some differences in the indentation allowed within lists. Basically, Kramdown requires you to line up the indent between list items with the first starting character after the space in your list item numbering. See this [blog post on Kramdown and Rouge](http://idratherbewriting.com/2016/02/21/bug-with-kramdown-and-rouge-with-github-pages/) for more details.

### Automatic mini-TOCs

By default, a TOC appears at the top of your pages and posts. If you don't want the TOC to appear for a specific page, such as for a landing page or other homepage, add `toc: false` in the frontmatter of the page.

The mini-TOC requires you to use the `##` Markdown syntax for headings. If you use `<h2>` elements, you must add an ID attribute for the heading element in order for it to appear in the mini-TOC (for example, `<h2 id="mysampleid">Heading</h2>`.


## News
The top navigation includes the option to show news. New are Jekyll posts.

### Posts

Posts are typically used for blogs or other news information because they contain a date and are sorted in reverse chronological order.

You create a post by adding a file in the \_posts folder that is named yyyy-mm-dddd-permalink.md, which might be 2016-02-25-my-latest-updates.md. You can use any number of subfolders here that you want.

Posts use the post.html layout in the \_layouts folder when you are viewing the post.

The news.html file in the root directory shows a reverse chronological listing of the 10 latest posts

### Allowed posts frontmatter

The frontmatter you can use with posts is as follows:

```yaml
title:  "Welcome to the Cloud Native Open Source!"
categories: tricorder
permalink: welcome.html
tags: [news]
```


| Frontmatter | Required? | Description |
|-------------|-------------|-------------|
| **title** | Required | The title for the page |
| **tags** | Optional | Tags for the page. Make all tags single words, with underscores if needed. Separate them with commas. Enclose the whole list within brackets. Also, note that tags must be added to \_data/tags_doc.yml to be allowed entrance into the page. This prevents tags from becoming somewhat random and unstructured. You must create a tag page for each one of your tags following the sample pattern in the tabs folder. (Tag pages aren't automatically created.)  |
| **keywords** | Optional | Synonyms and other keywords for the page. This information gets stuffed into the page's metadata to increase SEO. The user won't see the keywords, but if you search for one of the keywords, it will be picked up by the search engine.  |
| **summary** | Optional | A 1-2 word sentence summarizing the content on the page. This gets formatted into the summary section in the page layout. Adding summaries is a key way to make your content more scannable by users. The only drawback with summaries is that you can't use variables in them. |
| **permalink**| Required | This theme uses permalinks to facilitate the linking. You specify the permalink want for the page, and the \_site output will put the page into the root directory when you publish. Follow the same convention here as you do with page permalinks -- list the file name followed by the .html extension. |

## Tags
### Add a tag to a page
You can add tags to pages by adding `tags` in the frontmatter with values inside brackets, like this:

```
title: C Base Classes
tags: [tricorder, baseclass, code]
keywords:

```

### Tags overview
To prevent tags from getting out of control and inconsistent, first make sure the tag appears in the _data/tags.yml file. If it's not there, the tag you add to a page won't be read. I added this check just to make sure I'm using the same tags consistently and not adding new tags that don't have tag archive pages.

Additionally, you must create a tag archive page similar to the other pages named tag_{tagname}.html folder. 


### Setting up tags

Tags have a few components.

Create a tag archive file for each tag in your tags_doc.yml list. Name the file following the same pattern in the tags folder. 



## How to setup a local Jekyll to test changes

### Install Docker in you laptop or a Linux VM:

Docker site has the instructiosn on how to install Docker \
For installation on MAC for example : <https://docs.docker.com/docker-for-mac \>

### Clone the Ligato doc

    git clone  https://github.com/ligato/ligato.github.io.git

### Run Jekyll on a docker container :

On the folder that has the ligato doc code:

    docker run --rm --label=jekyll --volume=$(pwd):/srv/jekyll  -it -p 127.0.0.1:4000:4000 jekyll/jekyll jekyll serve

### Update and test

Go to localhost:4000 and you will have the ligato site.
Update the code and Jekyll will pick it up


{% include links.html %}
