---
layout: post
date: 2020-05-24 
permalink: /blog/pageSetup
title: Webpage Setup using Jekyll
---
This post illustrates how to get a Jekyll static site up and running with some useful add-ons and tips. Rather than be a self contained post, this serves as more of a collection of pointers to other useful resources around the internet that when taken together get the job done.<br>
The tutorial and linked instructions assume a linux system with a *bash shell* available.  

#### **Lift off!**

When starting off making a personal webpage with Jekyll, the first place one might end up is the [official website](https://jekyllrb.com/). The Jekyll documentation is great for understanding its more advanced features but I found, [this post](https://www.taniarascia.com/make-a-static-website-with-jekyll/) to be a much more self contained tutorial to getting a webpage up and running. In particular the official Jekyll documentation misses out on some of the details of creating some so called 'gemfiles' within the folder structure.

After completing the procedure in the linked tutorial, the directory structure we get looks as follows- 
{% highlight bash %}
.
├── 404.html
├── about.md
├── _config.yml
├── feed.xml
├── Gemfile
├── Gemfile.lock
├── index.md
├── _posts
│   └── 2019-07-16-welcome-to-jekyll.markdown
└── _site
    ├── 404.html
    ├── about
    │   └── index.html
    ├── assets
    │   ├── main.css
    │   └── minima-social-icons.svg
    ├── feed.xml
{% endhighlight %}

As you might gather from following the linked tutorial, when we run `bundle exec jekyll serve` (or just `jekyll serve`) in the outermost directory containing our project, a rendering version of our webpage on the local url [http://127.0.0.1:4000](http://127.0.0.1:4000) becomes avaiable. Or when we run `bundle exec jekyll build`. The finished webpage (or rather collection of pages) is placed into the `_site` directory from which it could be deployed onto our preferred web server. <br>
The `_site` directory is the finished product in some sense, so any changes made directly to the contents of this direcotry are pointless since these would be overwritten during the next compilation.

#### **Deploying in the wild**

Although usually when a jekyll based blog/webpage is setup, it is done on our own free github.io domain name using github pages, in which case we don't need to worry about deploying the contents of `_site` ourselves, since github does that for us automatically when we push the entire outer directory and any subsequent changes to it to a specially named remote directory. You can check out the [github pages](https://pages.github.com/) link for details.

#### **Creating and placing markdown files**

The first page a visitor would see when they arrive at your website would be the contents of `index.md` Under the minima template the top right corner would contain links to pages rendered using any and every markdown file present in either the outermost project directory, in any specially named directory for instance, one called `pages`. Every other webpage, (blog posts, or any other format) should appear either inside the `_posts` directory or any of its sub-directories (and so on recursively). Note that all markdown files present inside `_posts` must be named using the format `YYYY-MM-DD-file-name`, else the files won't show up in the `_site` finished product and by extension, will not be accessible from your website. Any `.md` markdown files present anywhere outside of this prescribed template will be treated like a new page and would be added to the upper right corner navigation pane.

Under the default *minima* template, there are two options as far as the layout of a webpage generatred using a markdown file is concerned - **post** and **page**. To me the difference between the two seems very minor with the post layout having an automatic date stamp at the top of the rendered document and the page layout (somewhat fortunately) lacking such a date feature.

#### **Customizing URLs using permalinks**

Another useful jekyll feature is that of *permalinks*. By default, jekyll parses your file name for its date to create a url, for instance, by default, the url of this page would have been <br> `https://ishank-juneja.github.io/2020/05/24/webpage-setup.html`. If you, like me, find such a url aesthetically displeasing, then you might want to look into [permalinks](https://www.youtube.com/watch?v=938jDG_YPdc) in Jekyll.<br>

The title, date, permalink and other useful information go into the [front-matter](https://jekyllrb.com/docs/front-matter/) of the markdown file, for instance, the front matter for this page looks like so,
{% highlight markdown %}
---
layout: post
date: 2020-05-24 
permalink: /blog/pageSetup
title: Webpage Setup using Jekyll
---
{% endhighlight %}
 
#### **The picture-frame surrounding your webpage**

At the bottom of every page on your static website, are little links to the owners email, twitter and potentially other places of online presence. The structure of this bottom bar is dependent entirely on our choice of template and cannot be altered without creating our own fork of the template or using some other pre-existing template. The contents of the bottom bar are controlled by the `_config.yml` file. The file for this web-page is given below. 

{% highlight yaml %}
title: Ishank's Webpage 
full_name: Ishank Juneja
email: ishankjuneja@gmail.com
email2: ishankjuneja@iitb.ac.in
description: >- # this means to ignore newlines until "baseurl:"
  Personal homepage: Ishank Juneja
url: https://ishank-juneja.github.io # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: ishankjuneja
github_username: ishank-juneja
quora_username: Ishank-Juneja
stackoverflow: 4477387/ishank-juneja
jekyll-mentions:
    base_url: https://github.com
# Google Analytics
google_analytics: UA-166118550-1
linkedin_username: ishank-juneja 
include: ['_pages']
footer-links-active:
  stackoverflow: true
# Build settings
markdown: kramdown
theme: minima
plugins:
  - jekyll-feed
{% endhighlight %}

The files that create this frame around each webpage are present in a so-called `Gemfile` and can be accessed and modified in the manner described in the next section. In an earlier version of Jekyll these files placed in specially named directories were These concealed directories are abstracted out by Jekyll inIn addition to creating entries for links that you wish to display, we must also provide Jekyll with relevant graphics (the GH icon, the twitter bird etc.) in the form of  .svg files that need to be included in a special `_includes` folder. Such a folder did not come by default when I created my webpage but some tutorials (see next section) assume the existence of such a directory anyway. To get the auxiliary files needed I referred to the [source code](https://github.com/martiansideofthemoon/martiansideofthemoon.github.io) for the webpage of a former student here at IITB. His well desgined webpage also inspired me to add [stack exchange flair](https://stackoverflow.com/help/flair) to the footer of my webpage.  

#### **Overriding themes**
At this point, it would be pertinent to mention a change to Jekyll themes brought about somewhat recently. Earlier the default minima theme for Jekyll appeared as a raw directory structure visible within the outermost directory of your project.<br>
However, the minima template is now present in the form of a so called `Gemfile` (Ruby language terminology). To be able to modify any of the default behaviour of the minima template, we can navigate to the Gemfile directory for the minima theme by running `xdg-open $(bundle info --path minima)` or `cd $(bundle info --path minima)` in a terminal. On navigating to the minima template source location a user would see the following files-
{% highlight bash %}
.
├── assets
│   ├── main.scss
│   └── minima-social-icons.svg
├── _includes
│   ├── disqus_comments.html
│   ├── footer.html
│   ├── header.html
│   ├── head.html
│   ├── icon-github.html
│   ├── icon-github.svg
│   ├── icon-twitter.html
│   ├── icon-twitter.svg
│   └── social.html
├── _layouts
│   ├── default.html
│   ├── home.html
│   ├── page.html
│   └── post.html
├── LICENSE.txt
├── README.md
└── _sass
    ├── minima
    │   ├── _base.scss
    │   ├── _layout.scss
    │   └── _syntax-highlighting.scss
    └── minima.scss
{% endhighlight %}

These are folders of the minima template that contain all the files, scripts and templates required to put together your webpage. 
[overiding](https://jekyllrb.com/docs/themes/#overriding-theme-defaults) 

#### **Monitoring traffic with Google Analytics**
If you wish to see how many hits your blog is getting, you could choose to create a google analytics (GA) profile and add some code snippets as is explained in this well written blog post on [adding analytics to Jekyll](https://desiredpersona.com/google-analytics-jekyll/). The post assumes that the project has an `_includes` directory in case your project does not already have one by defualt, you could simply create an empty `_includes` directory and populate it with the `analytics.html` file as described in the tutorial. However, simply doing this might be insufficient since the tutorial also tells us to add a few lines of code to a certain `head.html` file. The simplest way to cater to all these requirements is to populate the project directory with the complete folder structure that creates the document. The reader can refer to the [source code](https://github.com/ishank-juneja/ishank-juneja.github.io) for this webpage and could read the section below on overiding Jekyll themes.
 
A caveat here is that the GA identification code for a website is publily accessible. This does open up the possibility of misuse of the code through an deliberate or accidental attack that uses the same analytics code on another webpage. The good thing is that GA allows us to create filters to take care of such possiblities as has been described [here](https://stackoverflow.com/questions/49419250/is-sharing-google-analytics-tracking-id-safe). The instructions are as follows,<br>
1. In the google anayltics control view, in the lower left corner go to admin menu (The gear symbol)
2. Go to the all filters menu and click add filter to create a new *custom-filter*
3. Create an inclusive type filter and choose *Hostname* as the Filter Field
4. In the hostname field enter the domain name of your website, for instance to restrict view to this webpage I used  
4. Save the filer. This filter should exclude any spurious views arising from domain names other than yout

#### **LateX for the Web**
Technical posts inevitably involve mathematical symbols, equations and relationships. The software [LateX](https://www.latex-project.org/about/) is perhaps the most popular and efficient method of producing documents with typesetted mathematics. This post discusses a popular way of adding latex like typesetting to a webpage using a combination of [MathJax](https://www.mathjax.org/#gettingstarted) and [Pandoc](https://pandoc.org/).


[MathJax](http://docs.mathjax.org/en/latest/basic/mathjax.html) is a web based LateX rendering service that allows users to use LateX naturally  
