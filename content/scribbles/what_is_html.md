---
title: What is HTML?
description: Hyper Text Markup Language
draft: false 
tags: ['html','dev']
date: 2025-12-01
---

HTML helps to add structure to the webpage.  
current version of html = [html5](https://html.com/html5/)
The first file to parse will be ==index.html==.  
Common terms to note down are element (set of start, end tags and content in b/w).  
For example: ```<p> This is a an example of element</p>```  
The start tag is : ```<TAG_NAME>```  
The end tag is : ```</TAG_NAME>```
Concept of parent-child relation is important in HTML

#### Basic Structure of an HTML document

```html

<!DOCTYPE html> <!-- requried preamble -->
<html lang="en">
  <head> <!-- to delcare metadata, styles, script etc.. -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>My Website</title>
    <link rel="stylesheet" href="./style.css">
    <link rel="icon" href="./favicon.ico" type="image/x-icon">
  </head>
  <body> <!-- page content goes here -->
    <main>
        <h1>Welcome to My Website</h1>  
    </main>
    <script src="index.js"></script>
  </body>
</html>

```

### Definitions

HTML syntax information can be found [here](https://html.spec.whatwg.org/#syntax)

> [!tip] HTML indendation or linebreaks are not considered by Browser engines while rendering the webpage

#### Text Elements
- Comments : start with ```<!--``` and end with ```-->``` 
- Headings : ```<h1>``` to ```<h6>```, H1 > H2 .. > H6 
- Paragaraph : ```<p>```, in-line element
- Bold text : ```<b>``` : no semantic meaning, usage is discouraged
            : ```<strong>``` : *recommended*
- Italics text : ```<i>``` : no sematic meaning, usage is dicouraged  
               : ```<em>``` : *recommended*
- List
    - Orderd list, start with ```<ol>``` and individual list items are enclosed within ```<li> ... </li>``` 
    - Unorderd list, start with ```<ul>``` and individual list items are enclosed within ```<li> ... </li>```

#### Image Elements
- Image : element with no content, hence self closing. Element is ```<img />```
        : closing slash is not mandatory
    - Common attributes of **img** are:  
        - ==src== to define the source of image
        - ==alt== to define the alternative text
        - ==height== to define the height of image
        - ==width== to define the width of image

> [!info] src, alt and similar properties of an element is called Attributes. They are used to describe the element

#### Hyperlinks
- Links : elements that can point to an external or internal resource. Element starts with ```<a>```
    - Common attributes of **a**(anchor) element
        - ==href== to define the url of the target; URL to external resources or internal page path;  
          example: ```href="https://developer.mozilla.org/en-US/docs/Web/HTML"``` or ```href="blog.html"```
                 : To have link that does not direct anywhere, use ```href="#"```
        - ==target== to define the behavior of the anchor; ```target="_blank"``` will open the link in a new tab

#### Page Structuring
- Navigation : element starts with ```<nav>```   
- Header : defines the part where, logo, title and navigation bar are grouped together. Element starts with ```header```
- Article : container element for article. Element starts with ```<article>```
- Footer : continer element for footer text/contents. Element starts with ```<footer>```
- Div : container element used when we does not want to attach a certain meaning to a part of page. Element starts with ```<div>``` 
- Aside : defines secondary information that compliments the primary content of page. Element starts with ```<aside>```

>[!note] Html Entitties
> Reserve characters in HTML must be replaced with entitites  
> Examples would be copyright, less than, greater than, non breaking space etc.  
> Common list of HTML entities can be referenced from here - [Mozilla Developer - Character Reference](https://developer.mozilla.org/en-US/docs/Glossary/Character_reference)

#### Semantic HTML

Semantic HTML refers to the use of HTML tags that convey the meaning and structure of web content, rather than just its appearance.  
Semantic elements clearly describe their purpose both to the browser and to developers, making web pages more accessible, easier to maintain, and better for SEO.

- Why use Semantic HTML ?
    - ==Accessibility==: Screen readers and assistive technologies can better interpret the content.
    - ==SEO==: Search engines can better understand and rank your content.
    - ==Maintainability==: Code is easier to read and maintain for developers.

##### Non-Semantic

```html
<div id="header">My Website</div>
<div id="nav">Home | About | Contact</div>
<div id="content">Welcome to my website!</div>
```

##### Semantic

```html
<header>My Website</header>
<nav>Home | About | Contact</nav>
<main>Welcome to my website!</main>
```
