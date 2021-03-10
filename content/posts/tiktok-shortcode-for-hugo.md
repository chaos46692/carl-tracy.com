---
title: "Tiktok Shortcode for Hugo"
date: 2021-03-10T10:15:30-05:00
draft: false
tags : [website,hugo,tiktok,shortcode]
categories : [nerd]
---
# Tiktok shortcode for Hugo
When i was trying to post my **adorable** [Tiktok of a wee baby duck]({{< ref "baby-duck.md" >}})  that was just hatching, I couldn't find anything about embedding Tiktoks into Hugo. I tried just pasting the embed code into a markdown page, but it didn't work so I wrote a quick and dirty shortcode to do it:

 ## tiktok.html:
 Save this code in \\layouts\\shortcodes\\tiktok.html
 ~~~html
<script async src="https://www.tiktok.com/embed.js"></script> 
    
<blockquote class="tiktok-embed" cite="https://www.tiktok.com/{{ .Get "usr" }}/video/{{ .Get "id" }}" data-video-id="{{ .Get "id" }}" style="max-width: 605px;min-width: 325px;" > 
<section> 
		<a target="_blank" title="@chaos46692" href="https://www.tiktok.com/@chaos46692">@chaos46692</a> 
		<p></p> 
		<a target="_blank" title="Music" href="https://www.tiktok.com/music/{{ .Get "music" }}">Music</a> 
</section> 
</blockquote> 
~~~
## Parameters
On a tiktok video when you select "Share" there will be an option to Embed

![tiktok embed](/images/tiktok.webp)

Click that and it will give you code that looks like this:
~~~html
    <blockquote class="tiktok-embed" cite="https://www.tiktok.com/@chaos46692/video/6938028068116237573" data-video-id="6938028068116237573" style="max-width: 605px;min-width: 325px;" > 
      <section> 
        <a target="_blank" title="@chaos46692" href="https://www.tiktok.com/@chaos46692">@chaos46692</a> 
        <p></p> 
        <a target="_blank" title="♬ ily (i love you baby) - Surf Mesa" href="https://www.tiktok.com/music/ily-i-love-you-baby-6798329661525854210">♬ ily (i love you baby) - Surf Mesa</a> ]
        </section> 
    </blockquote> 
    <script async src="https://www.tiktok.com/embed.js"></script>
~~~
There are 3 parameters we need:
- usr - this is the user which in my case would @chaos46692, which comes from the "cite" section in the first blockquote, the part that comes after **tiktok.com/** and before **/video**. This is the user that created the tiktok
- id - this is the video id that we want to link, it can also be found in the blockquote section, the part reference by "data-video-id"
- music - the href link that contains "music"

In theory there I could put something in there for the music title but I'm lazy =)

## Implementation
To use shortcode just fill it in like this:
~~~md
{{< tiktok id="6938028068116237573"  usr="@chaos46692" music="ily-i-love-you-baby-6798329661525854210" >}}
~~~

