---
title: "Introduction to The Web & Servers"
permalink: /materials/introducing-humanities-computing/08-intro-web
excerpt: "An introduction to what the web is and how it works."
toc: true
---

<div class="notice--info">⚡️ This lesson draws from some of the fantastic materials from Ashley Blewer's <i>Halt and Catch Fire Syllabus</i>, which is a project designed around a fantastic television show that ran from 2014-2017 exploring the history of computing in the 1980s and 1990s. I would highly recommend the show to those interested in the topic, and even if you are not, I would highly recommend taking a look at the syllabus, which you can find here <a href="https://bits.ashleyblewer.com/halt-and-catch-fire-syllabus/">https://bits.ashleyblewer.com/halt-and-catch-fire-syllabus/</a>. In particular, this lesson draws from week 11 <a href="https://bits.ashleyblewer.com/halt-and-catch-fire-syllabus/classes/11.html">https://bits.ashleyblewer.com/halt-and-catch-fire-syllabus/classes/11.html</a></div>

In our last lesson, we learned how to create a webpage using HTML. But what is a webpage? How does it work? And how do we access it? These are the questions we will be exploring in this lesson.

## What is the Web?

We all use the web constantly, but what exactly is it?

Turning to our reliable source of information, Wikipedia, we find the following definition:

[**From Wikipedia:**](https://en.wikipedia.org/wiki/World_Wide_Web)

>"The World Wide Web (WWW), commonly known as the Web, is an information system where documents and other web resources are identified by Uniform Resource Locators (URLs, such as <https://www.example.com/>), which may be interlinked by hypertext, and are accessible over the Internet.[1][2] The resources of the WWW are transferred via the Hypertext Transfer Protocol (HTTP) and may be accessed by users by a software application called a web browser and are published by a software application called a web server."

While this is technically a definition, there's a lot to unpack here.

It is also helpful to return a bit to our HTML lesson and dive into the history of these web technologies. The core idea of Hypertext and the web is the idea of making it possible to link information across machines. One of the origins for this idea comes from Vannevar Bush's *Memex* that we talked about previously, which envisioned using microfilm to store and link information. While there are some earlier antecedents to this idea, it started to really take off in the 1950s and 60s. 

One famous example is *Project Xanadu*, which was developed by Ted Nelson in the 1960s and 1970s. Nelson's idea was to create a system where you could link any piece of information to any other piece of information.[^1] This idea was very influential, but it was never fully realized. You can read more about it here [https://en.wikipedia.org/wiki/Project_Xanadu](https://en.wikipedia.org/wiki/Project_Xanadu).

<figure>
    <a href="https://www.notion.so/cdn-cgi/image/format=webp,width=1920/https://images.ctfassets.net/spoqsaf9291f/e6NwTCm5KBL1YdnasYq0p/696dbb47785069c66cdb0e46563d104a/Xanadu_Schematic.png" class="image-popup">
    <img src="https://www.notion.so/cdn-cgi/image/format=webp,width=1920/https://images.ctfassets.net/spoqsaf9291f/e6NwTCm5KBL1YdnasYq0p/696dbb47785069c66cdb0e46563d104a/Xanadu_Schematic.png">
    </a>
</figure>

Other important figures include Douglas Engelbart, who developed the first computer mouse and the first hypertext system. However, it was really Tim Berners-Lee, who developed the World Wide Web in the 1980s and 1990s that we have to thank for the web as we know it today.

In his vision of the web, Tim Berners-Lee imagined a space where all information stored on computers could be interconnected. To achieve this, he developed key components: 

- the Universal Resource Identifier (URI) to identify any object on the web, 
- the HyperText Transfer Protocol (HTTP) for transferring hypertext, 
- and Hypertext Markup Language (HTML) as a universal language for creating web pages. 

Berners-Lee saw the URI as the web’s most fundamental innovation because it enables hypertext links to direct browsers to specific resources. HTML serves as the “fabric” or “connective tissue” of the web, with hypertext links at its core, allowing seamless connections between different web resources. For more on this history, I highly recommend Helmond, Anne. “A Historiography of the Hyperlink: Periodizing the Web through the Changing Role of the Hyperlink.” In *The SAGE Handbook of Web History*, by Niels Brügger and Ian Milligan, 227–41 2019. 

<figure>
    <a href="https://www.w3.org/History/1989/Image1.gif" class="image-popup">
    <img src="https://www.w3.org/History/1989/Image1.gif">
    </a>
</figure>

This diagram is of Tim Berners-Lee's initial vision for the web, and you can actually explore it via an emulator [https://worldwideweb.cern.ch/browser/](https://worldwideweb.cern.ch/browser/), which allows you to "party like it's 1989" and see the first web browser in action. The project is hosted by CERN, the European Organization for Nuclear Research, where Berners-Lee worked when he developed the web. You can read more about the history of the web here [https://worldwideweb.cern.ch/](https://worldwideweb.cern.ch/).

<figure>
    <a href="https://worldwideweb.cern.ch/images/www_project.png" class="image-popup">
    <img src="https://worldwideweb.cern.ch/images/www_project.png">
    </a>
</figure>

For those interested in a more detailed history of the internet and modern computing, there is also this timeline from the Computer History Museum, covering developments from the 1960s to the 1990s [https://www.computerhistory.org/internethistory/](https://www.computerhistory.org/internethistory/) that is helpful.

### What are Uniform Resource Locators (or URL)?

So now that we have a sense of the history of the web, let's dive into the core concept of the web, the **Uniform Resource Locator (URL)**. A URL is another term for a URI or a web address, which Berners-Lee envisioned as way to locate anything on the web. It is a string of characters that provides a way to access a specific resource on the web. URLs are made up of different components, which you can see in this diagram:

<figure>
  <a href="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL/mdn-url-all.png" class="image-popup">
    <img src="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL/mdn-url-all.png" alt="URL Encoding" />
  </a>
</figure>

This diagram is from the Mozilla Web Docs, which provide a great overview of the topic [https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL). You've already encountered URLs when we created an `a` tag in our HTML page.

```html
<a href="https://www.google.com/">Google</a>
```

This is a type of **Hyperlink** or **Link** that allows us to link to other webpages. In our case, we created something called an **external link** because we linked to a webpage that is not part of our website. But you can also create **internal links** that link to other pages on your website, or **anchors** that link to specific parts of a webpage (this is how the sidebar to navigate this page functions). You can read more about hyperlinks here [https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_are_hyperlinks](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_are_hyperlinks).

But URLs are more than just links; they are the way we access webpages. Mozilla describes URLs as akin to postal addresses, an analogy I find helpful. Just as you need an address to send a letter, you need a URL to send a request to a webpage. Before delving into the anatomy of URLs, understanding domain names and the web's functionality is beneficial.

While we often talk about the **cloud**, the concept of the web was originally based on the idea of a **web** of interconnected computers.

<figure>
  <a href="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/How_does_the_Internet_work/internet-schema-2.png" class="image-popup">
  <img src="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/How_does_the_Internet_work/internet-schema-2.png">
  </a>
</figure>

While this is a simplified explanation, essentially each computer operates as both a **client** and a **server**. A client is a computer that requests data from a server, and a server is a computer that provides data to a client in response.

<figure>
  <a href="https://2.bp.blogspot.com/_4l9wMe5bbSk/TMpvwVcMT3I/AAAAAAAAAK4/sCEjRQCkF1o/s1600/Client+Server+communication.GIF" class="image-popup">
  <img src="https://2.bp.blogspot.com/_4l9wMe5bbSk/TMpvwVcMT3I/AAAAAAAAAK4/sCEjRQCkF1o/s1600/Client+Server+communication.GIF">
  </a>
</figure>

Although our current internet is vastly more complex, this core idea of a client **requesting** data and a server **hosting** and **sending** data remains central to how the web functions.

Much of the data sent from servers to clients consists of HTML documents, but it can also include images, videos, audio, or virtually any type of data.

So, besides understanding that we have clients and servers, the other core concept is understanding how these computers communicate with each other, using something called the **Hypertext Transfer Protocol (HTTP).**

### What is HTTP?

Tim Berners-Lee developed the Hypertext Transfer Protocol (HTTP) as a way to transmit data across the world wide web. You'll notice that almost every URL starts with `http://` or `https://`. This indicates the the protocol used by the client and server use to communicate with each other.

I particularly like [Julia Evans' Zine on HTTP](https://jvns.ca/blog/2019/09/12/new-zine-on-http/) for explaining this concept:

<figure>
  <a href="https://pbs.twimg.com/media/EAiEGSgXsAELERE?format=jpg&name=large" class="image-popup">
  <img src="https://pbs.twimg.com/media/EAiEGSgXsAELERE?format=jpg&name=large">
  </a>
</figure>

This might seem technical, but the core idea is that when we enter a URL for a webpage (like google.com) into a web browser (like Chrome), we're actually sending an **HTTP request** to a **server** that hosts the HTML files and data. If the server validates our request (with a 200 OK status), it grants access to the webpage.

There's a number of different types of status codes, that you can see here:

<figure>
  <a href="https://pbs.twimg.com/media/D-bI-xyWkAAY0Qb?format=jpg&name=large" class="image-popup">
  <img src="https://pbs.twimg.com/media/D-bI-xyWkAAY0Qb?format=jpg&name=large">
  </a>
</figure>

But the most important are either `200` which means the request was successful, or `404` which means the request was not successful.

<figure>
  <a href="https://user-images.githubusercontent.com/2938045/56276896-af93b580-6103-11e9-9885-74981a49a5ae.png" class="image-popup">
  <img src="https://user-images.githubusercontent.com/2938045/56276896-af93b580-6103-11e9-9885-74981a49a5ae.png">
  </a>
</figure>

So, if you've ever seen an error message when you go to a webpage saying the page doesn't exist or `404 Not Found`, that means that you received a `404 error`, which is a type of status code you get back from an HTTP request when the server could not find the requested resource.

Now we can look more closely at the components of URL, through the Mozilla example here [https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL#scheme](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL#scheme).

So first we have the scheme or protocol, which tells the browser how to communicate with the server:

<figure>
  <a href="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL/mdn-url-protocol@x2_update.png" class="image-popup">
  <img src="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL/mdn-url-protocol@x2_update.png">
  </a>
</figure>

It's important to note that websites, web browsers, and search engines are all software applications that utilize the HTTP protocol to communicate with web servers, but they differ in function:

- a *website* is a collection of web pages and related content that is identified by a common **domain name** and published on at least one web server, like our course website. A website might have internal and external links on it as well.
- a *web browser* is a software application that is used to access websites and view web pages, like Google Chrome or Firefox or Safari. So while you can open any HTML document in a web browser, you can also use a web browser to access websites that are hosted from other servers.
- a *search engine* is a web service like Google or DuckDuckGo that allows users to search for content on the web, usually accessed from a web browser. These search engines often *crawl* websites via URLs to index the content of the web.

So now that we have some terminology, we can look at some of the rest of the URL.

### What is a Domain Name?

Next we have the **authority or domain name**, which is the name of the server that hosts the website:

<figure>
  <a href="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL/mdn-url-authority.png" class="image-popup">
  <img src="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL/mdn-url-authority.png">
  </a>
</figure>

Originally, when you used the internet you would access other websites using something called an **IP address**, which is a series of numbers that identifies a particular computer on the internet. You can see an example of an IP address here:

<figure>
  <a href="https://www.ipxo.com/app/uploads/2021/09/IPv4-anatomy.png" class="image-popup">
  <img src="https://www.ipxo.com/app/uploads/2021/09/IPv4-anatomy.png">
  </a>
</figure>

For example, the 1995 classic, thriller movie [*The Net*](https://en.wikipedia.org/wiki/The_Net_(1995_film)) starring Sandra Bullock as a systems analyst who "must use her computer skills to uncover the truth and clear her name." In the movie, she searches using IP addresses which you can watch a clip of here: [https://youtu.be/M1K-B8QVLyo?t=242](https://youtu.be/M1K-B8QVLyo?t=242).

IP Addresses are still used today, but they are not very human readable or user friendly (though we will be seeing some in action today), so instead, we use domain names. Domain names are part of the [**Domain Name System (DNS)**](https://en.wikipedia.org/wiki/Domain_Name_System) which is often described as a "phone book" or "address book" that translates IP addresses of servers into easier to understand domains.

Here's an example of a domain name:

<figure>
  <a href="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_domain_name/structure.png" class="image-popup">
  <img src="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_domain_name/structure.png">
  </a>
</figure>

In this explanation from Mozilla [https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_domain_name](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_domain_name), they outline that there are two general main components to a domain name:

- the **top-level domain (TLD)**, which is the last part or suffix of the domain name, like `.com` or `.org` or `.edu`. These are registered and managed by a central organization called ICANN (Internet Corporation for Assigned Names and Numbers). As you can imagine, there's a long history and fraught politics around TLDs, since they are often tied to nation states (for example, `.uk` or `.ca`). Notably, while there is `.us` for the United States, most US websites use `.com` or `.gov` if they are government-related. If you're curious to learn more I would recommend this blog post on dead-TLDs [https://astrid.tech/2022/04/05/1/dead-tlds/](https://astrid.tech/2022/04/05/1/dead-tlds/) and checking out the rich literature on Internet Governance with books like Milton Mueller's *Networks and States* or Laura DeNardis' *The Global War for Internet Governance*.
- the other part of a domain name is the **second-level domain (SLD)**, which is the part of the domain name that is to the left of the TLD, like `mozilla` in `mozilla.org`. These are managed by domain name registrars, like GoDaddy, and are registered by individuals or organizations. One key thing to understand is that you can purchase a domain name through registrars but not in perpetuity. This leads to not only needing to renew your domain names, but also to the rise of domain name speculation, where people purchase domain names in the hopes of selling them for a profit. You can read more about this in this fascinating article by Ingrid Burrington on visiting a Domain-Names Conference [https://www.theatlantic.com/technology/archive/2017/02/domain-names-dot-horse/516438/](https://www.theatlantic.com/technology/archive/2017/02/domain-names-dot-horse/516438/).

So in the case of the Mozilla example, we have a subdomain `developer` and a second-level domain `mozilla` and a top-level domain `org`.

This is all very technical, but we can start to try this out if we attempt to host our HTML pages.

## Hosting HTML Pages with GitHub Pages

While we have primarily been using GitHub for its versioning and collaboration features, it also has a feature called **GitHub Pages** that allows you to host static websites for free. This is a great way to share your work with others, and to learn more about how the web works. We will be building static web sites later this semester, but to get our feet wet, we can start by hosting our HTML pages.

### How to Host HTML Pages with GitHub Pages

First, you need to create a new repository on GitHub. You can do this by clicking the `+` in the top right corner and selecting `New Repository`.

As a user on GitHub, you can create a custom website that is hosted on GitHub Pages. This is a great option for creating a personal website, or a website for a project. This website is hosted at the `github.io` domain name so you don't have to pay for a domain name. However, to make this domain you need to create a repository with a specific name: **your username**. So for example, my GitHub username is `ZoeLeBlanc` so I need to create a repository called `ZoeLeBlanc.github.io` to generate my website. Since I already did that many moons ago, to show you what this looks like, I've created a new a username called `TestZoe`, so my repository is called `testzoe.github.io`.

<figure>
  <a href="{{site.baseurl}}/assets/images/githubio_repo.png" class="image-popup">
  <img src="{{site.baseurl}}/assets/images/githubio_repo.png">
  </a>
</figure>

When you create your repository, you need to make sure it is `Public` and you can add a `README.md` (though that is optional).

<figure>
  <a href="{{site.baseurl}}/assets/images/create_new_file.png" class="image-popup">
  <img src="{{site.baseurl}}/assets/images/create_new_file.png">
  </a>
</figure>

Once you have your repository, the next step is to add some HTML files. The most basic file you can add is an `index.html` file.

<figure>
  <a href="{{site.baseurl}}/assets/images/create_index_html.png" class="image-popup">
  <img src="{{site.baseurl}}/assets/images/create_index_html.png">
  </a>
</figure>

Once you've created your `index.html` file, you can either leave it blank or add some HTML code. Then once you do our standard git workflow of `add`, `commit`, and `push`, your website will be live as GitHub should start automatically hosting your website at `https://yourusername.github.io`. 

<figure>
  <a href="{{site.baseurl}}/assets/images/github_pages_settings_html.png">
  <img src="{{site.baseurl}}/assets/images/github_pages_settings_html.png">
  </a>
</figure>

You'll notice you don't need to configure anything, and that's because this is a special type of repository on GitHub, so any HTML files you add will automatically be hosted but that will **not** happen in other repos.

You can still see how GitHub is deploying your site by clicking on the tab called `Actions`:

<figure>
  <a href="{{site.baseurl}}/assets/images/actions_tab.png">
  <img src="{{site.baseurl}}/assets/images/actions_tab.png">
  </a>
</figure>

You should see some green lines and checks, all of which indicates your site is live.

<figure>
  <a href="{{site.baseurl}}/assets/images/live_website.png">
  <img src="{{site.baseurl}}/assets/images/live_website.png">
  </a>
</figure>

You can find see this live here [https://testzoe.github.io/](https://testzoe.github.io/).

## Homework: Doing It Live

Now that you understand how the web works, how to write HTML, and how to create a website on GitHub Pages, it is time to put your knowledge to the test. Your assignment is to follow the steps above to create a repository with your GitHub username, upload your HTML file from the previous [Source & Style Assignment]({{site.baseurl}}/materials/introducing-humanities-computing/07-intro-html#assignment-2-styling-the-web), and then have it correctly host via GitHub Pages.

You are welcome to customize your HTML file, but it should include at least:

- Your name
- A brief introduction about yourself
- A link to your GitHub profile

Once you're site is live, post the link to your HTML page via `github.io` to our next GitHub Discussion thread [https://github.com/CultureAsData-UIUC/is310-fall-2024/discussions/5](https://github.com/CultureAsData-UIUC/is310-fall-2024/discussions/5).

*Already hosting a website via GitHub Pages?*

You should instead update your existing website, either through adding either some sort of additional content (maybe a link to your group's GitHub?) or styling (maybe more interactivity?). You should then post a link to the updated site in the GitHub Discussion thread and share some details about when you initially created the site and what you updated.

----
[^1]: Interestingly, the Product Management/Note Taking app *Notion* has a great interview with Nelson for those that are interested in his work [https://www.notion.so/blog/ted-nelson](https://www.notion.so/blog/ted-nelson).