---
title: "Setting Up an SSL cetificate"
date: 2021-03-06T09:02:38-05:00
draft: false
tags : [website,hugo,ssl,cPanel]
categories : [nerd]
---
While setting up this website I was constantly greeted by this in my address bar:

![site not secure](/images/not-secure.png) 

I have no real reason for it to piss me off but it surely did. This must be remedied.

My hosting is different from my registrar/DNS so accomplishing it took a little more work than it might if you have all that stuff in one place.  I use [namecheap](http://namecheap.com/) as my registrar/DNS and [HostPapa](https://www.hostpapa.com/) for my hosting (mostly because I used to use Lunarpages and they got bought by HostPapa). I don't necessarily recommend any of these services - I am lazy and interia accounts for *a lot* of why I'm with them, I'm just telling you where I'm coming from.
# Getting your SSL certificate
Namecheap provides SSL certificates from PositiveSSL. I'm probably not the same process everywhere, but I'd imagine the process is pretty similar. There are two parts to setting up your SSL: activating it, and installing it.
## Activate your SSL
Activating your SSL will involve generating something called a CSR code. In cPanel under Security click on "SSL/TLS"

![cPanel SSL/TLS](/images/cPanel-ssl.png)

Off to the right there will be a section for "Private Keys", we want to click on " Generate, view, or delete SSL certificate signing requests."

![cPanel CSR](/images/cPanel-CRT.png)
## Fill in your information
It will request that you fill in information about your domain, once you fill in the required inputs and click "Generate" you will get a block of text in a format like this:
![Certificate Signing Request (CSR)](/images/CSR.png)

(note that I've blured out the text for security reasons). Now we need to activate our SSL. What I did was go to Namecheap and select SSL Certificates and click "Activate" for one of my free SSLs. It will ask for your CSR, just paste that text in there and click "Next".
## Validation methods
Next we have to validate that we own the domain. This is where it got dicey for me - I tried to use a [CNAME DNS](https://www.cloudflare.com/learning/dns/dns-records/) record, but for some reason it didn't work. I'm not sure if it had to do with Namecheap handling my DNS or not, but luckily after a few hours I tried something else that worked almost instantly - I uploaded a file on the server.

![Upload a file to validate your site on Namecheap](/images/ssl-confirmation.png)

## Validating with a file on your webserver (HTTP)
Supposedly the SSL issuer will send you the file to the email you used to register the SSL certificate, but I'm impatient as hell and realized in the status page it takes you to after you submit everything you can get the file manually

![get SSL validation file](/images/ssl-validation-1.png)

Just select down on "Edit Methods" and click "Download File"

![Download SSL validation file](/images/ssl-downloadfile.png)

The file will be a long string of alphanumeric characters. Upload it to the folder the issuing authority requested (usually  /.well-known/acme-challenge/ or /.well-known/pki-validation/ ). In Hugo you can put these in the Static/ folder and they will work just fine. It's probably best to test that you can see that file from a web browser just to be sure. For me it only took a few minutes before my certificate was validated. Hooray!

![SSL complete](/images/evil-SSL.png)

## Upload your certificate
There is just one more step! On the status page we need to download our SSL certificate and upload it to cPanel. If you click the "Download" button next to the Active status in the picture above, it will download a zip file. Extract that to your your machine. Now in cPanel go back to SSL/TLS and click " Generate, view, upload, or delete SSL certificates." (off to the right). You'll need to scroll down until you see "Choose a certificate file"

![Upload an SSL certificate](/images/ssl-upload.png)

Click "Choose File" and select the .crt from your zip file and click "Upload certificate". You should see something like this:

![SSL complete](/images/ssl-complete.png)

## Holy shit that was a lot of steps!
Yup! It's no wonder they've streamlined the process for folks. Not sure I really want to deal with that very often!
