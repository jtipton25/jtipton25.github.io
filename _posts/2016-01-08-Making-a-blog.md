---
title: "Making a Github pages Blog with RStudio and Jekyll on a Mac"
author: "John Tipton"
date: "January 8, 2016"
layout: post
---

In this post, I will detail the steps needed to make a website using Rstudio and Jekyll. For this tutorial, we host our website on Github pages, although this method does build a site that you can host on your own. First, we assume that you have a github account. If not, signing up is free for a public account and is a good idea for anyone who actively develops code.


## Github Pages account

[Github Pages](https://pages.github.com/) will host your site for free. Follow the link to sign up for github if you wish.

## Installing Jekyll
This section of the tutorial is based on the [Github website](https://help.github.com/articles/using-jekyll-with-pages/)
### Installing Ruby
First, we have to make sure that Ruby language (version greater than `2.0.0`) is installed. To check, open the Terminal application and type

```bash
ruby --version
```
If Ruby is not installed, follow the [install directions](https://www.ruby-lang.org/en/downloads/)

### Installing Bundler
Bundler is a package manager for Ruby. To install bundler, type in the Terminal

```bash
gem install bundler
```
### Installing Jekyll
First, change to your site's repository directory, mine is `~/jtipton25.github.io`

```bash
cd yourusername.github.io
```
Next, we need to create a file called `Gemfile` using a text editor. I use `emacs`, which is built into any *nix computer. To open the file `Gemfile` and create one if it doesn't already exist, open Terminal and type

```bash
emacs Gemfile
```
Then type into the emacs file 

```bash
source 'https://rubygems.org'
gem 'github-pages'
```
To close and save the file use the emacs command for save

```bash
ctrl-x ctrl-s
```
where, while holding the `ctrl` key you type `x` then `s`. Then exit emacs using

```bash
ctrl-x ctrl-c
```
If at anytime you are unsure, just hit `esc` a few times to clear out the `emacs` buffer. To complete the installation type into the Terminal

```bash
bundle install
```

# Seting up your blog
To setup a new blog, open `Terminal` and make sure you are in your Github pages username directory

```bash
cd yourusername.github.io
```
and create the blog using the command 

```bash
jekyll new .
```
If you don't have a Github Pages directory, you can create a new directory `myBlog` for your blog using 

```bash
jekyll new myBlog
```
Once your blog is created, you can explore the files using `ls`. For use with `RMarkdown`, I create a new folder

```bash
mkdir _Rposts
```
You should see the files 

```bash
Gemfile 
_config.yaml
about.md
feed.xml
index.html
```
and the folders

```bash
_drafts
_includes
_layouts
_Rposts
_posts
_sass
css
```
#### Overview of the important file structure
For more see the [jekyll site structure page](http://jekyllrb.com/docs/structure/)
##### Files
- `_config.yaml` stores the configuation information and allows for powerful customization of your site. For more information see [jekyll configuration page](http://jekyllrb.com/docs/configuration/).
- `about.md` is the page that is shown as the `README` when users navigate to your Github repository.
- `index.html` is the file that `jekyll` uses to build the blog page links.
#####
- `_drafts` is where you can write and test your content without posting. Files should be named `title.Rmd`
- `_includes` is for small snippets of code that can be repeatedly reused. Think of headers, footers, title bars, sidebars, etc. Default files include `footer.html`, `head.html`, and `header.html`
- `_layouts` are the templates that are used for your website. Layouts are chosen in the `YAML Front Matter` of each page. Defalut layouts include `default.html`, `page.html`, and `post.html`
- `_Rposts` is needed when you use `.Rmd` files to generate your website. Files **MUST** be named `YEAR-MONTH-DAY-title.Rmd` where `YEAR` is the four digit year, `MONTH` is the two digit month, and `DAY` is the two digit day. Examples include `2016-01-24-first-post.Rmd`
- `_posts` is where you can use markdown `.md` files to generate webpages. Files **MUST** be named `YEAR-MONTH-DAY-title.md`
-`_site` is where the generated site is placed if you want to host your site locally. This is created when you build your site


## First Test Build
To test your new site, open `RStudio`


```r
library(servr)
jekyll(dir = "~/path/to/yourusername.github.io", command = "bundle exec jekyll build")
```
and notice the website pops up in the `Viewer` window for your testing. You can even use the button in the `Viewer` window top left corner to pop the website into your web-browser. Now we can go about customizing the site and making posts.

### Making your first website
To make your first website, open a new `RMarkdown` file using the menu by selecting `File -> New File -> R Markdown`. Give your file a title like *My First Blog* and save it in the folder `~/path/to/yourusername.github.io/_drafts`. Feel free to edit the file, then once you are satisfied, move the file to the `_Rposts` folder and rename the file to something like `YEAR-MONTH-DAY-My-First-Post.Rmd`. To finish building the blog, type into `RStudio`

```r
library(servr)
jekyll(dir = "~/path/to/yourusername.github.io", command = "bundle exec jekyll build")
```
I keep the above code in the file `~/path/to/yourusername.github.io/makeBlog.R`

# How to customize your website

## Configure your `_config.yaml` file.
Your `_config.yaml` serves as a template for configuring the basics of your site. You can use a text editor (I use `emacs`) to edit your `_config.yaml` file. Mine looks like


```bash
# Site settings
title: The posterior
email: jtipton25@gmail.com
description: > # this means to ignore newlines until "baseurl:"
  Here I will post commonly used code examples and statistical problems 
  
baseurl: "" # the subpath of your site, e.g. /blog/
url: "http://jtipton25.github.io" # the base hostname & protocol for your site
github_username:  jtipton25

# Build settings
markdown: redcarpet
highlighter: pygments
```

There are many options here. You can add social media tags like Twitter (`twitter_username:`) and others under the `github_username:`. There are two `markdown` options, `kramdown` amd `redcarpet`.  
 
 
## Configure `head.html` to add mathematical equations to the blog
If you have the desire to add $\LaTeX$ style equations to your blog, add the following to the file `head.html` in the `_includes/` folder between the lines `<head>` and `<\head>`.

```bash
  <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
  <script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    TeX: {
      equationNumbers: {
        autoNumber: "AMS"
      }
    },
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'] ],
      processEscapes: true
    }
  });
  </script>
```

My entire `head.html` file is given below. Feel free to copy and replace.

```bash
{% raw %}<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <title>{% if page.title %}{{ page.title }}{% else %}{{ site.title }}{% endif %}</title>
  <meta name="description" content="{% if page.excerpt %}{{ page.excerpt | strip_html | strip_newlines | truncate: 160 }}{% else %}{{ site.description }}{% endif %}">

  <link rel="stylesheet" href="{{ "/css/main.css" | prepend: site.baseurl }}">
  <link rel="stylesheet" href="{{ "/css/jekyll-github.css" }}">
  <link rel="canonical" href="{{ page.url | replace:'index.html','' | prepend: site.baseurl | prepend: site.url }}">
  <link rel="alternate" type="application/rss+xml" title="{{ site.title }}" href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" />
  <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
  <script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    TeX: {
      equationNumbers: {
        autoNumber: "AMS"
      }
    },
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'] ],
      processEscapes: true 
    }
  });
  </script>
</head>{% endraw %}
```


## Customizing the webpage using RMarkdown
## R code
Adding `R` code to your website is super easy. For example, to genereate a histogram using an `R` chunk in `RStudio` use:



```
     ```{r}
     hist(rnorm(100))
     ```
```

to produce the output


```r
hist(rnorm(100))
```

![plot of chunk unnamed-chunk-22](/figure/Rposts/2016-01-08-Making-a-blog/unnamed-chunk-22-1.png) 

## Equations

You can also include inline equations like $Y = X \beta + \epsilon$ or inset equations using 

```bash
\begin{align} 
Y\_{t} = \phi Y\_{t-1} + \epsilon\_{t} 
\end{align}
```
to get
\begin{align}
Y\_{t} = \phi Y\_{t-1} + \epsilon\_{t}
\end{align}

or by typing 

```bash
$$Y\_{t} = \phi Y\_{t-1} + \epsilon\_{t}$$
```

to get

$$Y\_{t} = \phi Y\_{t-1} + \epsilon\_{t}$$

Note that to get the underscore character `_` to render a subscript properly, you need to use the command `\_` in markdown to use markdown's escape mechanism. This means to get $Y\_{t}$ to render, type `Y\_{t}`.
