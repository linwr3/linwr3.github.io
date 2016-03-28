---
layout: post
comments: false
categories: jekyll
---

### Jekyll Quick Start

I have thought about developing my own website to detail what I learn at college, of course, it is not intended as a tutorial. However the website approve is too troublesome for me. Finally, I chose to configure my blog on github.

Well, it is my first post in github, hence I decided to introduced how to build such a website.
I will write down the process in a simple way, though there is a official tutorial [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html#toc_0).

1. Create a new repository named `username.github.io`(Practice has proved, it must be ==io== instead of ==com== if the branch used is master, otherwise, the branch must be gh-pages)
2. Install git in your laptop
2. Create SSH key

```
cd ~/.ssh#	<!--to check whether there is already a ssh key-->
ssh-keygen -t rsa -C youremail@sample.com	<!--create key with your email-->
...	<!--input as prompt-->
clip < ~/.ssh/id_rsa.pub	<!--copy your key-->
```

4\. Paste your key into your github's settings
`Settings -> SSH keys -> new SSH key`
5\. Select an automatic page
`settings -> Automatic page generator`
6\. Parked domains. Create a File name 'CNAME' and write down your domain name, and resolve your domain name to username.github.io.
6. If you are Chinese user, and try to [run jekyll locally](http://jekyllbootstrap.com/usage/jekyll-quick-start.html#toc_6), uses this [gem](https://ruby.taobao.org/).

Well, now the website is finish, and you can start to write your posts. What? There is no template you prefer, look at [here](http://jekyllthemes.org/).

Here are some commands of git that always be used. [more about git](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

```
git clone https://github.com/username/username.github.io.git username.github.io
<!--copy the repository to local-->

git config --global user.email user@example.com
git config --global user.name  "username"
<!--set your name and email-->

cd username.github.com
git checkout --orphan gh-pages
<!--if the name of repository is username.github.com then the branch must be gh-pages-->

git remote add origin git@github.com:username/username.github.io.git
<!--set url(origin)-->

git add .
git commit -m "message"
git push origin master
<!--push local file to github-->
```
