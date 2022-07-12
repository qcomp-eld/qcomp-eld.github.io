# QComp

This is a blog maintaned by Eldorado's Quantum Comptuting study group.

The blog uses Casper thene for Jekyll.

## Kasper

This is a port of Ghost's default theme [Casper v1](https://github.com/tryghost/casper) for Jekyll. Here is a live [demo](https://rosario.io/kasper). 

Feel free to fork, change, modify and re-use it.

## Installation

Install ruby

Fork https://github.com/qcomp-eld/qcomp-eld.github.io to a personal repository on github

Download your personal repository

    gem install jekyll
    gem install jekyll-paginate

    
## Change _config.yml

Create an entry for your user information inside `authors` section.


## How to use it

Build page and start local web server

    jekyll serve

Build page into `_site` folder

    jekyll build

## How to create a post

Inside `_posts` folder, create a file with file mae format: `YYYY-MM-DD-title.markdown`
Add a header inside de file as the sample bellow:

```

---
layout: post
title:  "O Experimento da Dupla Fenda"
date:   2022-05-10 14:24:00
author: carla_dias
categories: teoria computacao-quântica educativo
usemathjax: true
---

```


## How to publish your post

Make sure you have referenced all images and context. 

Create a merge request.