---
title: "Setting Up Hugo with SSL and MathJax"
date: 2021-03-06T09:44:41-05:00
draft: false
tags : [website,hugo,ssl]
categories : [nerd]
---
I've had a lot of fun setting up Hugo to set it up how I want. I will caveat this with a few things - I _tried_ to do things the right way (not editing the theme directly) but I'm  *really* **really** ***really*** fucking impatient and in a lot of cases I just edited the theme because I don't want to deal with this shit. I'm never going to update it so screw it.

After I initial got the site up and running there were a couple of things I wanted to get working:
- [Set up an SSL certificate]({{< ref "setting-up-ssl.md" >}} "Setting up SSL")
- Get the site to use https by default
- Set up the site so I can use [MathJax](https://www.mathjax.org/) (I am a *huge* nerd after all)
- Set up the website to allow search engines to track it (remove NOFOLLOW / NOINDEX)

## Redirecting to SSL
Now that I have done *all that work* to set up SSL, it would be a shame if some schmuck came along and was reading this with plain ole HTTP, wouldn't it? Yup! That would be tragic, let's make sure it doesn't happen. It's been a while since I futzed with .htaccess rules, but a little Google-fu lead me to [this post](https://geekflare.com/http-to-https-redirection/) to set it up. Basically you need to add this:

    RewriteEngine On 
    RewriteCond %{HTTPS} off 
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}

to your .htaccess file.  **BAM!** Redirected

If you would like to deploy the .htaccess file from Hugo, just drop it in the "static" folder in the root project of your Hugo site and it will automatically be added to your site when you build.

## Setting up MathJax**
I set up MathJax with the help of [this post](https://geoffruddock.com/math-typesetting-in-hugo/). Basically you need to put code to pull in a new html file and inject that into layouts\\_default\\baseof.html in the Ananke theme. All you need then is some custom css to make it work.

### Add mathjax_support.html
Add this code to \\layouts\\partial\\mathjax_support.html

    <script>
      MathJax = {
        tex: {
          inlineMath: [['$', '$'], ['\\(', '\\)']],
          displayMath: [['$$','$$'], ['\\[', '\\]']],
          processEscapes: true,
          processEnvironments: true
        },
        options: {
          skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre']
        }
      };
    
      window.addEventListener('load', (event) => {
          document.querySelectorAll("mjx-container").forEach(function(x){
            x.parentElement.classList += 'has-jax'})
        });
    
      </script>
    
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script type="text/javascript" id="MathJax-script" async
      src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
      </script>

###    Inject code into the head
Copy \\themes\\ananke\\layouts\\_default\\baseof.html to \\layouts\\_default\\baseof.html and add the following line just before the \<\/head\> tag

    {{ partial "mathjax_support.html" . }}

### Add CSS
Add the following to your custom css

    code.has-jax {
    -webkit-font-smoothing: antialiased;
    background: inherit !important;
    border: none !important;
    font-size: 100%;
    }

## Set the model as production
Out of the box Hugo is set up as a test environment, which meant this line was included in the output:

    <META NAME="ROBOTS" CONTENT="INDEX, FOLLOW">

That's bad because it basically means that search engines aren't indexing my site. WTF am I doing this for if I can't have access to the sweet, sweet Googles??!?! The fix was pretty easy, at least for my template (Ananke), essentially, I just needed to add this line to my config.toml in the [params] section

    [params]
      env="production"
    