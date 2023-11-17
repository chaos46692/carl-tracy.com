---
title: "Lorentz attractor"
date: 2025-11-01T10:39:00-05:00
draft: true
tags : [nerd,math,chaos]
categories : [nerd,math]
chart: true
---

## My Lorentz attractor
I love the Lorentz attractor, but can I simulate it in javascript? Let's find out!
<!--more-->
{{< rawhtml >}}
<script
src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.4/Chart.js">
</script>

<style>
@-webkit-keyframes rotate {
  from {
    -webkit-transform: rotate(360deg);
  }
  to { 
    -webkit-transform: rotate(0deg);
  }
}

.spin {
    -webkit-animation-name:            rotate; 
    -webkit-animation-duration:        2.0s; 
    -webkit-animation-iteration-count: infinite;
    -webkit-animation-timing-function: linear;
	max-width: 200px;
	height: auto;
}
</style>


<script>
    function resetProj() {
        //alert("CLEAR!");
    }
</script>

<canvas id="Lorentz"></canvas>

<form>
    <button  type="button" onclick="javascript:resetProj();">Clear</button >
</form>

{{< /rawhtml >}}

![](/images/wheel2.png ) 
{.spin}

## Behold my Lorenz attractor!
I've been collecting a group of meditation videos on YouTube that I use to manage anxiety. This is mostly a place for me to come, press a button and get a random meditaiton video.
So if you'd like a random meditation video to help with anxiety click the button below!


## That's not a wheel!

Shut up! Okay fine, fair point but "Wheel o' Meditation" sounds much better than "Meditation button". Maybe some day I'll actually have a wheel that spins, but that's too much effort for now =P 

Shout out to [Ana Ulin](https://anaulin.org/blog/hugo-raw-html-shortcode/) who provided the [Hugo](https://gohugo.io/) shortcode that allowed me to build this easily 
and to whoever at roneo.org for [this post](https://roneo.org/en/hugo-custom-css-classes-images-markdown-attributes/) that let me make my image spin =)

