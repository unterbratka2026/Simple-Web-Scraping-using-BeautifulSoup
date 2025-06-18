# Simple-Web-Scrapping-using-BeautifulSoup
Simple Web Scraping using BeautifulSoup on Python

First of all we need to install beautifulsoup4. 
4 is essential, since there is an older version of beautifulsoup, which may give different result.  
In order to do that just use pip command.

```python
pip install beautifulsoup4
```

Once that is done, we need to make sure that we have a parser. 
There are a lot of parsers, and the results (depending which parser we are using) may vary depending on the HTML which you are trying to parse. 
If we are trying to parse perfectly formed HTML, then there will a little or no difference in the parsed HTML, however, if we are trying to parse non-perfect HTML, then these parsers will try to fill in the holes, missing information differently. 
We are going to use LXML parser, which is recommended by the BeautifulSoup
https://www.crummy.com/software/BeautifulSoup/bs4/doc/

We just need to install LXML using pip
```python
pip install lxml
```

---

In order to pass html into our python using BeautifulSoup there are a couple of ways to do that. We pass our HTML into python, because we need to get BeautifulSoup object

We can either pass html as a string or we can pass html as a file 

```python
from bs4 import BeautifulSoup  
import requests  
	  
with open('simple.html') as html_file:  
	soup = BeautifulSoup(html_file, 'lxml')  
	  
print(soup.prettify())
```

This will print out just all of the html that we have

```html
<!DOCTYPE html>
<html class="no-js" lang="">
 <head>
  <title>
   Test - A Sample Website
  </title>
  <meta charset="utf-8"/>
  <link href="css/normalize.css" rel="stylesheet"/>
  <link href="css/main.css" rel="stylesheet"/>
 </head>
 <body>
  <h1 id="site_title">
   Test Website
  </h1>
  <hr/>
  <div class="article">
   <h2>
    <a href="article_1.html">
     Article 1 Headline
    </a>
   </h2>
   <p>
    This is a summary of article 1
   </p>
  </div>
  <hr/>
  <div class="article">
   <h2>
    <a href="article_2.html">
     Article 2 Headline
    </a>
   </h2>
   <p>
    This is a summary of article 2
   </p>
  </div>
  <hr/>
  <div class="footer">
   <p>
    Footer Information
   </p>
  </div>
  <script src="js/vendor/modernizr-3.5.0.min.js">
  </script>
  <script src="js/plugins.js">
  </script>
  <script src="js/main.js">
  </script>
 </body>
</html>
```

---

In order to get specific match of some specific tag and work with it we can use 

```python
match = soup.title  
print(match)
```

This will print out 

```html
<title>Test - A Sample Website</title>
```

If we want to access only the text of that tag just use 

```python
match = soup.title.text  
print(match)
```

Accessing the tag like an attribute (example above), will just give us the first appearance of that tag in the html. 

We can use find() method to do something similar but this method allow us to pass in some arguments in order to find exact tag that we are looking for 

```python
match = soup.find('div', class_='footer')  
print(match)
```

So as we can see find method will search for a div with class attribute of footer. We can also specify the id. class_ is written with underscore since we already have class keyword in python itself. 

```python
article = soup.find('div', class_='article')  
print(article)  
	  
headline = article.h2.a.text  // Here we used h2.a.text since there is no point of searching it using soup when we can find it inside article
print(headline)
```


So instead of using find() and search for all specific tags and then parse the information from it (above example) we can use find_all() method. This method will return a list that match those arguments. 

```python
for article in soup.find_all('div', class_='article'):  
	headline = article.h2.a.text  
	summary = article.p.text  
	print(headline)  
	print(summary)
```

This will get us:

	Article 1 Headline
	This is a summary of article 1
	Article 2 Headline
	This is a summary of article 2

---

If the html file is not inside the same directory we are working at we must get the html file using requests

For example:
```python
source = requests.get('https://youtube.com').text # Now the source variable shoul be equal to the HTML version of the website.
soup = BeautifulSoup(source, 'lxml')
```

