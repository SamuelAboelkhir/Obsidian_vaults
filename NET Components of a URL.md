---
tags:
- NET
- CYBER
MOC: Cybersecurity
---
[[_0000 Home|Home]] | [[_0005 Cybersecurity MOC|Back to Cybersecurity MOC]] | [[NET Networking index|Back to index]]
- Acquired from [https://www.geeksforgeeks.org/computer-networks/components-of-a-url/](https://www.geeksforgeeks.org/computer-networks/components-of-a-url/)
# Components of a URL
If you are SpongeBob then your URL is Bikini Bottom on the floor of the Pacific Ocean but if you’re not, your URL is definitely the address of the house you live in! URL stands for [**Uniform Resource Locator**](https://www.geeksforgeeks.org/blogs/url-full-form/)**.** For a website, a URL is basically where the website lives online and it helps visitors to identify the site easily as well as get an idea about its contents.

A typical website has **at least 3 parts** in its URL like *www.google.com* but some complex URLs might also have 8 to 9 parts namely scheme, subdomain, domain name, top-level domain, port number, path, query, parameters, and fragment.

![](https://media.geeksforgeeks.org/wp-content/uploads/20210625160610/urldiag.PNG)

Components of a URL

**1\. Scheme:**

```
https://
```

The protocol or scheme part of the URL and indicates the **set of rules that will decide the transmission and exchange of data**. HTTPS which stands for Hyper Text Transfer Protocol Secure tells the browser to **display the page in Hyper Text (HTML) format** as well as **encrypt any information** that the user enters in the page. Other protocols include the **FTP** or File Transfer Protocol which is used for transferring files between client and server, **SMTP** or Single Mail Transfer Protocol which is used for sending emails.

**2\. Subdomain:**

```
https://www.
```

The subdomain is used to separate different sections of the website as it **specifies the type of resource** to be delivered to the client. Here the subdomain used ‘www’ is a general symbol for any resource on the web. Subdomains like ‘blog’ direct to a blog page, ‘audio’ indicates the resource type as audio.  
  
**3\. Domain Name:**

```
https://www.example.
```

Domain name **specifies the organization or entity** that the URL belongs to. Like in *www.facebook.com* the domain name ‘facebook’ indicates the organization that owns the site.

**4\. Top-level Domain:**

```
https://www.example.co.uk
```

The TLD (top-level domain) **indicates the type of organization the website is registered to**. Like the **.com** in *www.facebook.com* indicates a **commercial entity**. Similarly,.org indicates organization,.co.uk a commercial entity in the UK.

  
**5\. Port Number:**

```
https://theukdomain.uk/:443
```

A port number specifies **the type of service** that is requested by the client since **servers often deliver multiple services**. Some default port numbers include 80 for HTTP and 443 for HTTPS servers.

**6\. Path:**

```
https://theukdomain.uk//blog/article/search
```

Path **specifies the exact location** of the web page, file, or any **resource that the user wants access to**. Like here the path indicates a specific article in the blog webpage.

**7\. Query String Separator:**

```
https://theukdomain.uk/article/search?
```

The **query string** which contains specific parameters of the search is **preceded by a question mark (?).** The question mark tells the browser that **a specific query is being performed**.

**8\. Query String:**

```
https://theukdomain.uk/article/searchdocid=720&hl=en
```

The query string **specifies the parameters of the data that is being queried from a website’s database.** Each query string is **made up of a parameter and a value** joined by the equals (=) sign. In case of multiple parameters, query strings are joined using the ampersand (&) sign. The parameter can be a number, string, encrypted value, or any other form of data on the database.

**9\. Fragment:**

```
https://theukdomain.uk/article/searchdocid=720&hl=en#dayone
```

The fragment identifier of a URL is **optional**, usually appears at the end, and begins with a hash (#). It indicates a **specific location within a page such as the ‘id’ or ‘name’ attribute for an HTML element**.

You might be surprised that though URLs seem to be trivial in nature, what your URL looks like is actually a significant factor in Search Engine Optimization (SEO). Feel free to check out more on URLs from here:

- *https://developer.mozilla.org/enUS/docs/Learn/Common\_questions/What\_is\_a\_URL*
- *https://www.hostgator.com/blog/best-url-structure-seo/*
- Source: https://amberwilson.co.uk/blog/urls/