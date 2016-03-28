---
layout: post
comments: false
categories: python
---

Recently I start to learning Python programming language.
Indeed I don't like to read a book introduce the programming language syntax. It's too boring and ineffective for me. I prefer learning by doing a small side project. This time, I intent to learn web crawler with Python.
Before beginning, we need to install [Python 2.7](https://www.python.org/downloads/windows/).

Remember to add the installation path to the environment variable 'path'.

Well, let's begin.
We know, usually our Web-browser requests a web page from the server, and it will return a HTML text, then the browser parse it into a web page. We can mimic this behavior with Python like:

```
import urllib2
request = urllib2.Request('http://www.google.com')
response = urllib2.urlopen(request)
print response.read()
```

or

```
import urllib2
response = urllib2.urlopen(‘http://www.baidu.com’)
# urllib2.urlopen(url, data, timeout)
print response.read()
```

Sometimes, we need post some key-value pairs to server.

```
# post
import urllib
import urllib2
values = {"key1":"value1", "key2":"value2"}
data = urllib.urlencode(values)
url = "https://www.google.com"
request = urllib2.Request(url, data)
response = urllib2.urlopen(request)
print response.read()
```

```
# get
import urllib
import urllib2
values = {"key1":"value1", "key2":"value2"}
data = urllib.urlencode(values)
url = “https://www.google.com”
request = urllib2.Request(url+'?'+ data)
response = urllib2.urlopen(request)
print response.read()
```

We know http headers is important for HTTP request and HTTP response. It includes the information about the client browser, the request page, the server, and so on.
For example, some server applications would judge whether the request is from the local by reading http headers([anti-leech](http://www.zzbaike.com/wiki/%B7%C0%B5%C1%C1%B4)). In this case, we need to set the value of "referer" as the server.

```
import urllib
import urllib2
url = 'http://www.server.com/login'
values = {"key1":"value1", "key2":"value2"}
headers = {'User-Agent': 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)',
			'Referer': 'http://www.google.com/'}
data = urllib.urlencode(values)
request = urllib2.Request(url, data, headers)
response = urllib2.urlopen(request)
page = response.read()
```

Some server applications would detect the requested rate with a same IP address. If the frequency is too high, the server will ban your request. So, we should set a proxy server to change the IP address.

```
import urllib2
enable_proxy = True
proxy_handler = urllib2.ProxyHandler({'http' : 'http://some-proxy.com:8080'})
null_proxy_handler = urllib2.ProxyHandler({})
if enable_proxy:
    opener = urllib2.build_opener(proxy_handler)
else:
    opener = urllib2.build_opener(null_proxy_handler)
urllib2.install_opener(opener)
```

Now, we can get the HTML document we need, and the next step is to [parse the document](../beginning-python-2-BS4).
