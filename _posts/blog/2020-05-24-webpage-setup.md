---
layout: post
date: 2020-05-24 
permalink: /blog/pageSetup
title: Webpage Setup using Jekyll
---
This post illustrates how to get a Jekyll static site up and running with some useful add-ons and tips. Rather than be a self contained post, this serves as more of a collection of pointers to other useful resources around the internet that when taken together get the job done.

#### **Lift off!**

When starting off making a personal webpage with Jekyll, the first place one might end up is the [official website](https://jekyllrb.com/). The Jekyll documentation is great for understanding its more advanced features but I found, [this post](https://www.taniarascia.com/make-a-static-website-with-jekyll/) to be a much more self contained tutorial to getting a webpage running. In particular the official Jekyll documentation misses out on some of the details of creating some so called 'gemfiles' within the folder structure.

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

As you might have already understood from following the linked tutorial, when we run `bundle exec jekyll serve` (or just `jekyll serve` in the outermost directory containing our to get a rendering of our webpage on the local url [http://127.0.0.1:4000](http://127.0.0.1:4000). Or when we run, `bundle exec jekyll build`. The finished webpage (or rather collection of pages) is placed into the `_site` directory from which it could be deployed onto our preferred web server.<br>

#### **Deploying in the wild**

Although usually when a jekyll based blog/webpage is setup, it is done on our own free github.io domain name using github pages, in which case we don't need to worry about deploying the contents of `_site` ourselves, since github does that for us automatically when we push the entire outer directory and any subsequent changes to it to a specially named remote directory. You can check out the [github pages](https://pages.github.com/) link for details.

#### **Creating and placing markdown files**

The first page a visitor would see when they arrive at your website would be the contents of `index.md` Under the minima template the top right corner would contain links to pages rendered using any and every markdown file in the `pages` directory or any of its sub-directories. Every other webpage, (blog posts, or some other format) should appear either inside the `_posts` directory or any of its sub-directories (and so on recursively). Note that all markdown files present inside `_posts` must be named using the format `YYYY-MM-DD-file-name`, else the files won't show up in the `_site` finished product and by extension, will not be accessible from your website.<br>

Under the default *minima* template, it seems that there are two options as far as the layout of your markdown file is concerned - **post** and **page**. To me the difference between the two seems very minor with the post layout having an automatic date stamp at the top of the rendered document and the page layout (somewhat fortunately) lacking such a date feature.<br>

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
title: Personal Webpage | Ishank Juneja
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
google_analytics: UA-XXXXXXXX-X
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

In addition to creating entries for links that you wish to display, we must also provide Jekyll with relevant graphics (the GH icon, the twitter bird etc.) in the form of  .svg files that need to be included in a special `_includes` folder. Such a folder did not come by default when I created my webpage but some tutorials (see next section) assume the existence of such a directory anyway. To get the auxiliary files needed I referred to the [source code](https://github.com/martiansideofthemoon/martiansideofthemoon.github.io) for the webpage of a former student here at IITB. The webpage also inspired me to add [stack exchange flair](https://stackoverflow.com/help/flair) to the footer of my webpage.  

#### **Monitoring traffic with Google Analytics**
If you wish to see how many hits your blog is getting, and which geographical regions these views come from, you could choose to create a google analytics profile and add some code snippets as is explained in this well written blog post on [adding analytics to Jekyll](https://desiredpersona.com/google-analytics-jekyll/). The post assumes some pre-existing structure to your project which in my case was not present by defualt. Once again [the source code](https://github.com/martiansideofthemoon/martiansideofthemoon.github.io) reference came in handy as clarification.

