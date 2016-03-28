---
layout: post
comments: false
categories: python
---

We know how to get a HTML document in [previous post](2015-11-26-beginning-python-1-crawler.md). Now, we need a tool to parse the HTML document.

Beautiful Soup is a third-party Python module used to parsing the Web page. Now download [beautifulsoup4](http://www.crummy.com/software/BeautifulSoup/bs4/download/4.3/) first. Unzip and execute a CMD command `python setup.py install`.

Here are some usual basic usage.

***

##### Create an instance

Import BS4 first.

```
from bs4 import BeautifulSoup
```

e.g. there is a web page, then we create a BS4 instance.

```
html = '''
<html>
<head>
	<title>test</title>
</head>
<body>
	<img src="http://xxx.com/xxxx.jpg" alt="blablabla" />
	<p>Halo, 3Q</p>
	<div id="t">
    	<!--comment here-->
		<p>R U O K</p>
	</div>
</body>
</html>
'''
soup = BeautifulSoup(html)
```

Of course, we can get a page from local html file.

```
soup = BeautifulSoup(open('/var/www/index.html'))
```

***

##### Get the specified tag

formatted output

```
print soup.prettify()
```

get img tag

```
print soup.img

```

get the attributes of img

```
print soup.img.attrs
```

get the name of the tag

```
print soup.img.name
# print img
```

get the specified attribute

```
print soup.img.get('src')
# or
print soup.img['src']
```

get the content in the tag while it will ignore the comment.

```
print soup.p.string
```

Attention! `print soup.div.string`It will print NONE, as this tag has sub-tag. So we should judge the type first.

```
if type(soup.div.string)==bs4.element.Comment:
	pass
```

get the second sub-tag of body tag

```
print soup.body.contents[1]
```

list all the sub-tag of body

```
list(soup.body.children)
for child in soup.body.children:
	print child
```

get all contents

```
print soup.strings
print soup.stripped_strings
for string in soup.strings:
	print (repr(string))
```

***

##### Node operation

get parent node

```
p = soup.p.content
print p.parent.name
for parent in p.parents:
	print parent.name
```

get sibling node

```
p.next_sibling
p.next_siblings

p.previous_sibling
p.previous_siblings
```

get adjacent node(include children node)

```
p.next_element
p.next_elements

p.previous_element
p.previous_elements
```

***

##### find\_all

```
soup.find_all(name, attrs, recursive, text, * *kwargs)
```

```
soup.find_all(re.compile('')) # regular expression
```

```
soup.find_all(True) # return all tag
```

```
def has_class_but_no_id(tag):
	return tag.has_attr('class') and not tag.has_attr('id')

soup.find_all(has_class_but_no_id)
```

```
soup.find_all(id='link')
soup.find_all(href=re.compile("elsie"))
```

```
soup.find_all("a", class_="sister")
```

```
soup.find_all(text=re.compile("include"))
soup.find_all(text=re.compile("include"), limit=2)
```

```
soup.find_all("title", recursive=False) # only find among direct children nodes
```

***

##### find

`find` the usage is similar to find_all, while it only return one node

In addition, there are other function.
`find_next` & `find_all_next`
`find_parents` & `find_parent`
`find_previous` & `find_all_previous`
`find_next_siblings` & `find_next_sibling`
`find_previous_siblings` & `find_previous_sibling`

***

##### CSS selector

```
soup.select('title')
soup.select('#link')
soup.select('head > title')
soup.select('.sister')
soup.select('p a')
soup.select('p a[href="xxx"]')
```

***
All we need are ready. The [next post](beginning-python-3-crawler-pixiv.md), I'll start my project.
