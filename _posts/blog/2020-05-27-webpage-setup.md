---
layout: post
date: 2020-05-24
permalink: /blog/pageSetupJekyll
comments: true
title: Webpage Setup using Jekyll
mathjax: true
---
This post illustrates how to get a Jekyll static site up and running with some useful add-ons and tips. Rather than be a self contained post, this serves as more of a collection of pointers to other useful resources around the internet that when taken together get the job done.<br>
The tutorial and linked instructions assume a linux system with a *bash shell* available. Further familiarity with creating content for the Web with [Markdown](https://www.markdownguide.org/getting-started/) is assumed.   

#### **Lift Off!**

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

As you might gather from following the linked tutorial, when we run `bundle exec jekyll serve` (or just `jekyll serve` on some systems) in the outermost directory containing our project, a rendering of our webpage on the local URL [http://127.0.0.1:4000](http://127.0.0.1:4000) becomes avaiable. Or when we run `bundle exec jekyll build`, the finished webpage (or rather collection of pages) is placed into the `_site` directory from which it could be deployed onto our preferred web server. <br>
The `_site` directory is the finished product in some sense, so any changes made directly to the contents of this direcotry are pointless since these would be overwritten during the next compilation.

#### **Deploying in the Wild**

Usually when a jekyll based blog/webpage is setup, it is done on our own free `Git-Hub-user-name.github.io` domain name using [github pages](https://pages.github.com/), in which case we don't need to worry about deploying the contents of `_site` ourselves, since github does that for us automatically when we push the entire outer directory and any subsequent changes to it to a specially named remote.

Other than [github pages](https://pages.github.com/), I also have experience with hosting a website on my university [academic homepage](http://home.iitb.ac.in/~ishankjuneja/). In our universities case, students are allocated 1 GB of space on the [BigHome](http://home.iitb.ac.in/) cloud service. Also each student gets a domain name of the form `http://home.iitb.ac.in/~LDAPuserName`. The [procedure](https://www.cc.iitb.ac.in/page/personalwebpage) to make a student webpage work there is as follows
1. Login on `http://home.iitb.ac.in/` using your LDAP ID
2. Create a repository named `public_html` onto your drive space
3. Place the contents of the `_site` directory compiled by Jekyll into the `public_html` repository and you're done
 
In the case of my university homepage I had some trouble making sense of the `url` and `base_url` field appearing in the `_config.yml` file. The StackOverflow question I [asked here](https://stackoverflow.com/questions/52680191/forced-to-use-absolute-urls-in-jekyll) would clear up how the fields have to be set. 

#### **Creating and Placing Markdown Files**

The first page a visitor would see when they arrive at your website would be the contents of `index.md` Under the minima template the top right corner would contain links to pages rendered using any and every markdown file present in either the outermost project directory, in any specially named directory for instance, one called `pages`. Every other webpage, (blog posts, or any other format) should appear either inside the `_posts` directory or any of its sub-directories (and so on recursively). Note that all markdown files present inside `_posts` must be named using the format `YYYY-MM-DD-file-name`, else the files won't show up in the `_site` finished product and by extension, will not be accessible from your website. Any `.md` markdown files present anywhere outside of this prescribed template will be treated like a new page and would be added to the upper right corner navigation pane.

Under the default *minima* template, there are two options as far as the layout of a webpage generatred using a markdown file is concerned - **post** and **page**. To me the difference between the two seems very minor with the post layout having an automatic date stamp at the top of the rendered document and the page layout (somewhat fortunately) lacking such a date feature. Note that the default behaviour of these templates can be changed or new templates to replace them can be created as explained in the section on overriding themes explains.

#### **Customizing URLs using Permalinks**

Another useful jekyll feature is that of *permalinks*. By default, jekyll parses your file name for its date to create a URL, for instance, by default, the URL of this page would have been <br> `https://ishank-juneja.github.io/2020/05/24/webpage-setup.html`. If you, like me, find such a URL aesthetically displeasing, then you might want to look into [permalinks](https://www.youtube.com/watch?v=938jDG_YPdc) in Jekyll.<br>

The title, date, permalink and other useful information go into the [front-matter](https://jekyllrb.com/docs/front-matter/) of the markdown file, for instance, the front matter for this page looks like so,
{% highlight markdown %}
---
layout: post
date: 2020-05-24 
permalink: /blog/pageSetup
title: Webpage Setup using Jekyll
---
{% endhighlight %}
 
#### **The Picture-Frame Surrounding Your Webpage**

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

The files that create this frame around each webpage are present in a so-called `Gemfile` and can be accessed and modified in the manner described in the next section. In an earlier version of Jekyll these files placed in specially named directories were accessible from the project directory by default. These concealed files consist of things like SVG files (for the GH icon, the twitter bird etc.)<br>
The quick way to access these auxiliary files would be to refer to the source code of a pre-existing website. For instance, while making this webpage I refereed to [this website](https://github.com/martiansideofthemoon/martiansideofthemoon.github.io) which belongs to a former student here at IITB. His well desgined webpage also inspired me to add [stack exchange flair](https://stackoverflow.com/help/flair) to the footer of my webpage. Perhaps the better way to access this directory structure would be to navigate to the `Gemfile` supporting the content. This is explained in the next section.

#### **Overriding Themes**
The files that help make the minima template are present in a `Gemfile` (Ruby language terminology). To be able to modify any of the default behaviour of the minima template, we can navigate to the Gemfile directory for the minima theme by running <br> 
`xdg-open $(bundle info --path minima)` or<br> 
`cd $(bundle info --path minima)`<br> 
in a terminal. 

On navigating to the minima template source location a user would see the following files-

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

These are folders of the minima template that contain all the files, scripts and templates required to put together your webpage. To override any of the defualt behavious, as is done in many of the add on tutorials linked to in this blog, we need to make the `_includes`, `_layouts` and `_sass` folders available in our project directory by copy-pasting them there.<br>
Hence forth, Jekyll would use the versions of these special name directories avaiable in our own project directory rather than the default theme `Gemfile` from which it was taking these files earlier. This allows us to make changes to the default `post.html` template. Modifying the template opens the door to things like adding a line that computes the estimated [read time](http://planetjekyll.github.io/snippets/reading-time), adding analytics and LaTeX typesetting support among other things. For more, see the [overriding](https://jekyllrb.com/docs/themes/#overriding-theme-defaults) tutorial from the Jekyll documentation.

#### **Monitoring Traffic with Google Analytics**
If you wish to see how many hits your blog is getting, you could choose to create a google analytics (GA) profile and add some code snippets as is explained in this well written blog post on [adding analytics to Jekyll](https://desiredpersona.com/google-analytics-jekyll/). The post assumes that the project has an `_includes` directory in case your project does not already have one by defualt, you could simply create an empty `_includes` directory and populate it with the `analytics.html` file as described in the tutorial. However, simply doing this might be insufficient since the tutorial also tells us to add a few lines of code to a certain `head.html` file. The simplest way to cater to all these requirements is to populate the project directory with the complete folder structure as was described in the previous section.
 
A caveat here is that the GA identification code for a website is publily accessible. This opens up the possibility of misuse through an deliberate or accidental attack that uses the same analytics code on another webpage. The good thing is that GA allows us to create filters to take care of such possiblities as has been described [here](https://stackoverflow.com/questions/49419250/is-sharing-google-analytics-tracking-id-safe). The instructions are as follows,<br>
1. In the google anayltics control view, in the lower left corner go to `admin` menu (The gear symbol)
2. Go to the `All Filters` menu in the left most panel and click `Add Filter` to create a new `custom-filter`
3. Create an `inclusive` type filter and choose *Hostname* as the Filter Field
4. In the hostname field enter the domain name of your website, for instance for this webpage I used `ishank-juneja\.github\.io`
5. Save the filer and head back to the three pane admin interface, in the right most pane select the `Filters` option, and apply the filter you created earlier to the GA code associated with the webpage you are protecting
6. This filter should exclude any spurious views arising from domain names other than your own

#### **Make your Blog Interactive**
If you're like me, you would agree that an interactive element is still missing from the current setup. [Disqus](https://disqus.com/) is a free to use blog comment hosting service that comes pre-integrated with Jekyll. To add disqus comments to a blog simply, one may follow the instructions [here](https://desiredpersona.com/disqus-comments-jekyll/) which happen to be by the same author that wrote the piece on adding analytics to a jekyll webpage.

A problem with Disqus is that, it asks for an email address from a viewer before it lets them leave a comment. Personally I feel this might be too much to ask for. A way around, to let viwers interact more seamlessly with your blog would be to enable [disqus reactions](https://help.disqus.com/en/articles/2199501-reactions). Be sure to enable this before pushing the code changes mentioned in the linked tutorial that make Disqus initiate. This is because the inclusion of reactions, does not apply retrospectively to blog posts (rather URLs) that existed prior to its enabling.

#### **LaTeX for the Web**

- #### **With MathJax**

Technical posts inevitably involve mathematical symbols, equations and relationships. The software [LaTeX](https://www.latex-project.org/about/) is perhaps the most popular and efficient method of producing documents with typesetted mathematics. [MathJax](https://www.mathjax.org/#gettingstarted) is a type setting service for rendering mathematics typed using LaTeX on the web. It works by placing typeset mathematics at the appropriate location by first rendering the graphic object on their server and then delivering it to your website using a [CDN](https://www.cloudflare.com/en-in/learning/cdn/what-is-a-cdn/). The setup required to make MathJax start working is quite minimal and is expalined in [this blog](http://sgeos.github.io/github/jekyll/2016/08/21/adding_mathjax_to_a_jekyll_github_pages_blog.html).<br> 
Once the setup is done, you can start to typeset inline and centered mathematics in a manner similar to the examples below,<br>
Here we use inline math for the Pythagorean theorem: \\( a^2 + b^2 = c^2\\). This math in raw text looked like `\\( a^2 + b^2 = c^2\\)`. Next we have Euler's famous result as display math, 
\\[ e^{i \pi} + 1 = 0\\]
Which could have been typset as `\\[ e^{i \pi} + 1 = 0\\]`.

- #### **With Pandoc**

In case we wish to make an existing `.tex` file compatible with the web, we can use [Pandoc](https://pandoc.org/). Pandoc can be installed using `sudo apt-get install pandoc`. Once installed, we can covert an existing LaTeX file, with a `.tex` extension into a `.html` file using the command<br> 
`pandoc -s name-of-the-file.tex --mathjax -o name-of-the-file.html`.<br>
This is preferable if the contents of the document are to remain unchanged, however, if significant changes are to be made, it would be easier to make them to a `Markdown` file rather than an `html` file. Pandoc provides the option to convert a `.tex` file to Markdown directly using the command<br> `pandoc -s name-of-the-file.tex --mathjax -o name-of-the-file.md`.<br> 
Though alright in principle, I found two practical difficulties with this approach.
- Display math is not rendered correctly since `Markdown + MathJax` requires the use of formatting of the type 
{% highlight latex %}
Some text prior

$$
\Big(\sum_{i = 1}^{n} a_i b_i\Big)^2 \leq 
\Big(\sum_{i = 1}^{n} a_i^2\Big) \Big(\sum_{i = 1}^{n} b_i^2\Big) 
$$

Some text after
{% endhighlight %}
Some text prior

$$
\Big(\sum_{i = 1}^{n} a_i b_i\Big)^2 \leq 
\Big(\sum_{i = 1}^{n} a_i^2\Big) \Big(\sum_{i = 1}^{n} b_i^2\Big)
$$

Some text after<br>
- However the auto-generated output from pandoc would give something like the following instead
{% highlight latex %}
Some text prior $$\Big(\sum_{i = 1}^{n} a_i b_i\Big)^2 \leq 
\Big(\sum_{i = 1}^{n} a_i^2\Big) \Big(\sum_{i = 1}^{n} b_i^2\Big)$$ Some text after
{% endhighlight %}
Some text prior $$\Big(\sum_{i = 1}^{n} a_i b_i\Big)^2 \leq 
\Big(\sum_{i = 1}^{n} a_i^2\Big) \Big(\sum_{i = 1}^{n} b_i^2\Big)$$ Some text after
- Even with inline math, I observed that there were some problems with rendering some fonts, for instance `$\mathcal{V}$` was not producing correct results, moreover its presence was distorting subsequent text. 

None of these problems show up in the html version of these files. As I understand the reason for this is that the html file created by pandoc has explicit tags rather than \$ symbols which makes all code unambiguous to MathJax. In my opinion, to get the best of both worlds, an easy workaround is to first create an html file using pandoc, then change its file extension to `.md` so that it can be edited like a Markdown file while having all the math correctly rendered using html tags.

MathJax is not as powerful as native TeX and there would likely be some other compatibility issues between the contents of a `.tex` file and what MathJax is capable of. Some alternatives to MathJax to typeset math are [KaTeX](https://katex.org/) and [LaTeX.css](https://latex.now.sh/). These require some additional setup and I couldn't get them to work on my webpage.

#### **In Conclusion**
While getting my webpage up and running and subsequently adding snippets of copy-pasted html and JavaScript to implement the add ons, often times instructions were unclear. When stuck, or even when starting off, the best way to be done might be to either download or fork the [source repository](https://github.com/ishank-juneja/ishank-juneja.github.io) for this webpage.

