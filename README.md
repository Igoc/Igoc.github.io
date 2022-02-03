# Blog

#### &nbsp; Jekyll blog based on [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) <br/><br/>

## Prerequisite

``` bash
# Install ruby packages
gem install jekyll bundler
```

<br/>

## How to use

``` bash
# Install bundle
bundle install

# Update bundle
bundle update

# Serve with hot reload at localhost:4000
bundle exec jekyll serve --watch
```

<br/>

## Usage

### Post

``` markdown
---
title: "<TITLE>"

categories: [<MAIN CATEGORY>, <SUB CATEGORY>]
tags: [<TAG>, ...]

date: <DATE>
last_modified_at: <LAST MODIFIED DATE>
---

<SUBTITLE>
{:.notice--primary}

<CONTENT>
```

&nbsp; Create markdown files with the above format in **_posts/\<MAIN CATEGORY>/\<SUB CATEGORY>/**. <br/>
&nbsp; The file name should be the same as **\<DATE>-\<TITLE>.md**. <br/><br/>

### Category

``` html
---
title: "<TITLE>"
layout: archive
permalink: /category/<CATEGORY>/
---

{% assign posts = site.categories["<CATEGORY>"] %}

{% for post in posts %}
    {% include archive-single.html type=page.entries_layout %}
{% endfor %}
```

&nbsp; Create HTML files with the above format in **_pages/categories/**.