---
layout: post
comments: false
categories: jekyll
---

### Jekyll Quick Start

I have thought about developing my own website, however website approve is too troublesome for me. Finally, I chose to configure my blog on github.

Well, it is my first post in jekyll, hence I decided to introduced how to build such a website.
I will write down the process in a simple way, though there is a official tutorial [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html#toc_0).

1. Create a new repository named `username.github.io`(Practice has proved, it must be io instead of com if the branch used is master)

2. Create SSH key

    cd ~/\.ssh#	//to check whether there is already a ssh key
	
    ssh\-keygen \-t rsa -C youremail@sample\.com	//create key with your email
	
    \.\.\.	//input as prompt
	
    clip &lt; ~/\.ssh/id_rsa\.pub	//copy your key
	
3. paste your key into your github's settings

`Settings -> SSH keys -> new SSH key`
