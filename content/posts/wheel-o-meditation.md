---
title: "Wheel o Meditation"
date: 2023-11-01T10:39:00-05:00
draft: false
tags : [mental health,meditation]
categories : [mental health]
---

## My Random Meditation Video Generator
{{< rawhtml >}}
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
{{< /rawhtml >}}

![](/images/wheel2.png ) 
{.spin}

I've been collecting a group of meditation videos on YouTube that I use to manage anxiety. This is mostly a place for me to come, press a button and get a random meditaiton video.
So if you'd like a random meditation video to help with anxiety click the button below!

{{< rawhtml >}}



<script>
	const pages = [];
	pages.push("https://www.youtube.com/watch?v=aG6JbaAPQd4");
	pages.push("https://www.youtube.com/watch?v=BFqs75OW-7I");
	pages.push("https://www.youtube.com/watch?v=LLeqY9ingRY");
	pages.push("https://www.youtube.com/watch?v=lVx3mFxML80");
	pages.push("https://www.youtube.com/watch?v=O-6f5wQXSu8");
	pages.push("https://www.youtube.com/watch?v=pBoAquxhspA");
	pages.push("https://www.youtube.com/watch?v=rG_mpEJcOtg");
	pages.push("https://www.youtube.com/watch?v=syx3a1_LeFo");
	pages.push("https://www.youtube.com/watch?v=Xl_B45DpMLU");
	pages.push("https://www.youtube.com/watch?v=xwPpafaq6aQ");
	pages.push("https://www.youtube.com/watch?v=YF_P1ZzYgjA");
	pages.push("https://www.youtube.com/watch?v=z6X5oEIg6Ak");
	pages.push("https://www.youtube.com/watch?v=ZToicYcHIOU");
	pages.push("https://youtu.be/XkkxNN4SSO4?si=iW_wJxymTiI1P3AU");
	
	function g() {
		var n = Math.floor(Math.random() * pages.length) 
		//alert(n)
		var url = pages[n];
		console.log("selected file #"+n+" url:"+url)
		//alert(url);
		window.open(url, '_blank');
	}
</script>

<button type="button" onclick="javascript:g();">Let's Meditate!</button>


{{< /rawhtml >}}

## That's not a wheel!

Shut up! Okay fine, fair point but "Wheel o' Meditation" sounds much better than "Meditation button". Maybe some day I'll actually have a wheel that spins, but that's too much effort for now =P 

Shout out to [Ana Ulin](https://anaulin.org/blog/hugo-raw-html-shortcode/) who provided the [Hugo](https://gohugo.io/) shortcode that allowed me to build this easily 
and to whoever at roneo.org for [this post](https://roneo.org/en/hugo-custom-css-classes-images-markdown-attributes/) that let me make my image spin =)

