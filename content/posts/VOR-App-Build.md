---
title: " Building the VOR Rehab App"
date: 2024-08-14T12:00:00-05:00
draft: true
tags : [nerd,code]
categories : [nerd]
chart: false
---
{{< rawhtml >}}
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js"></script>

<style>
    pre {
        font-size: 10pt;
        font-weight: normal;
    }
</style>
{{< /rawhtml >}}

Previously I had build a [Vestibulo-ocular Reflex Therapy App]({{<ref VOR-exercise>}}). I had put the details about building it 
in the page, but it got kind of long after I started adding features so I figured why have one article when you can have two, right?


### The Form
The first thing we need is a form. Each of the inputs will call a javascript function to perform it's function. The 
tempo can be input with a slider or a text box, and each will update the other. We also need a time limit and a 
font scale. If I knew a better way to calculate the actual size of the letter on the screen I could get around this, 
but alas I could not figure out a reliable way to do that. Maybe in the future!
<!--more--> 
{{< rawhtml >}}
<pre><code class="language-html">
&lt;label&gt;Metronome tempo&lt;/label&gt;&lt;br/&gt;
&lt;input type="range" min="30" max="150" value="70" 
    class="slider" id="tempo" onchange="settempo(this.value)"  &gt;
&lt;br/&gt;
&lt;input type="text" id="tempoText" value="70" 
    onchange="settempo2(this.value)"&gt;&lt;/input&gt; bpm
&lt;br/&gt;
&lt;br/&gt;
&lt;label&gt;Total Time&lt;/label&gt;&lt;br/&gt;
&lt;input type="text" id="timelimit" value="60" 
    onchange="setTime(this.value)"&gt;&lt;/input&gt; seconds
&lt;br/&gt;
&lt;br/&gt;
&lt;label&gt;Font Scale&lt;/label&gt;&lt;br/&gt;
&lt;input type="text" id="fontSize" value="100" 
    onchange="setFontScale(this.value)"&gt;&lt;/input&gt; pt
&lt;br/&gt;
&lt;br/&gt;
&lt;button type="button" onclick="run();"&gt;Run&lt;/button&gt;
&lt;button type="button" onclick="stop();"&gt;Stop&lt;/button&gt;
</code></pre>
{{< /rawhtml >}}


### The Code
The first thing we need are our variables. 
* running is a boolean that shuts it down when we click "stop" or when the timer runs out
* timeLimit is the total time we input in the form
* tempo is the input tempo
* delay is the total number of milliseconds until the next metronome click
* tick and booong are audio files that play for the metronome and the bell that chimes when we're done
* endTime is the time at which we stop running
* calcTop, calcLeft, and fontScalar are variables that we will save in a cookie that determine where the letter is and it's size when we run
{{< rawhtml >}}
<pre><code class="language-javascript">
var running = false;
var timeLimit = 60.0;
var tempo = 70;
var delay = 1000.0 * 60.0 / tempo;
var tick = new Audio('/metronome.mp3');
var booong = new Audio('/boooong.mp3');
var endTime = Date.now();

var calcTop = 0;
var calcLeft = 0;
var fontScalar = 24;
</code></pre>
{{< /rawhtml >}}

#### Adding the Target div
First we need some CSS that will put our div at the top of the screen and takes up the entire screen. Display is set to 
none to keep it hidden until we need it.
{{< rawhtml >}}
<pre><code class="language-css">
<style>
    .container {
        position:absolute;
        top:0;
        left:0;
        width:100%;
        height:100%;
        border: none; 
    }
    .target {
        font-size:36pt;
        font-weight:bold;
        position:sticky;
        border: none;  
        background:white;
        width:100%;
        height:100vh;
        top:0;
        left:0;
        z-index:100000;
        text-align: center;
        vertical-align: middle;  
        padding-top:calc(50vh - 30pt);
        display:none;
    }
    #drag{
        border:none; 
        display:block;
        position:absolute;
    }
  </style>
</code></pre>
{{< /rawhtml >}}
For the purposes of our code we need a div that is at the top of the body before any other elements. If I was using just HTML I could do that easily:
{{< rawhtml >}}
<pre><code class="language-html">
&lt;html&gt;
&lt;body&gt;
&lt;div class="target" id="theletter"&gt;
  &lt;div class="container" "&gt;
    &lt;div id="drag"&gt;&lt;span id="TargetSpan"&gt;A&lt;/span&gt;&lt;br/&gt;&lt;form&gt; 
    &lt;button type="button" onclick="stop();"&gt;Stop&lt;/button&gt;&lt;/form&gt;&lt;/div&gt;
  &lt;/div&gt;
&lt;/div&gt;

&lt;!--&gt; Blog article goes here &lt;/!--&gt;

&lt;/body&gt;
&lt;/html&gt;
</code></pre>
{{< /rawhtml >}}

However, because [Hugo](https://hugo.io) builds a bunch of other stuff that I don't have control over, I need to add some javascript that injects that div at the very beginning of my body:
{{< rawhtml >}}
<pre><code class="language-javascript">
var parent = document.body;  
var div = document.createElement('div');
div.classList.add('target');
div.id='theletter'
div.innerHTML = '&lt;div class="container"&gt;'+
    '&lt;div id="drag"&gt;&lt;span id="TargetSpan"&gt;A&lt;/span&gt;&lt;br/&gt;' +
    '&lt;form&gt; &lt;button type="button" onclick="stop();"&gt;Stop&lt;/button&gt;' +
    '&lt;/form&gt;&lt;/div&gt;&lt;/div&gt;'
parent.insertBefore(div, parent.firstChild);  
</code></pre>
{{< /rawhtml >}}

#### Handling events
Now we need to update our inputs with some javascript. These functions correspond to the "onchange" events in the form we set up earlier.
{{< rawhtml >}}
<pre><code class="language-javascript">
function settempo(v) {
    tempo = v
    delay = 1000.0 * 60.0 / tempo; 
    document.getElementById("tempoText").value = tempo;
}

function settempo2(v) {
    tempo = v;
    delay = 1000.0 * 60.0 / tempo - 24.0;
    document.getElementById("tempo").value = tempo;
    //runProj(); 
}

function setFontScale(v) {
    fontScaler = v;
    console.log(fontScaler);
    document.getElementById("TargetSpan").style.fontSize = v + "pt"

}

function setTime(v) {
    console.log(v);
    timeLimit = v;
} 
</code></pre>
{{< /rawhtml >}}

#### Dragging
Because we want to have this letter directly in the middle of the screen and we have no idea of 
where folks have their screen mounted, I added a drag function that allows us to drag around the 
target to the center of the screen. Later on we will call this function on the div with the id "theletter".
We will also save the location in a cookie for the next time the user visits.

You'll notice that when we finish dragging there is code to save a cookie, I will cover that below.
{{< rawhtml >}}
<pre><code class="language-javascript">
// drag element
function dragElement(elmnt) {
    var pos1 = 0, pos2 = 0, pos3 = 0, pos4 = 0;
    elmnt.onmousedown = dragMouseDown;
    // These should be loaded from a cookie to save the position we want
    // If they are zero there's no cookie saved so set it to the approximate
    // center of the screen
    if ((calcTop<1) || (calcLeft<1)) {
        calcTop = screen.height / 2.0 - elmnt.offsetHeight;
        calcLeft = screen.width / 2.0 - elmnt.offsetWidth;
        console.log(calcLeft);
    }
    elmnt.style.top = calcTop + "px";
    elmnt.style.left = calcLeft + "px";


    function dragMouseDown(e) {
        e = e || window.event;
        e.preventDefault();
        // get the mouse cursor position at startup:
        pos3 = e.clientX;
        pos4 = e.clientY;
        document.onmouseup = closeDragElement;
        // call a function whenever the cursor moves:
        document.onmousemove = elementDrag;
    }

    function elementDrag(e) {
        e = e || window.event;
        e.preventDefault();
        // calculate the new cursor position:
        pos1 = pos3 - e.clientX;
        pos2 = pos4 - e.clientY;
        pos3 = e.clientX;
        pos4 = e.clientY;
        // set the element's new position:

        calcTop = (elmnt.offsetTop - pos2);
        calcLeft = (elmnt.offsetLeft - pos1);
        elmnt.style.top = calcTop + "px";
        elmnt.style.left = calcLeft + "px";


    }

    function closeDragElement() {
        // stop moving when mouse button is released:
        document.onmouseup = null;
        document.onmousemove = null;

        console.log(calcTop);
        console.log(calcLeft);
        var cookie = createCookieValue(calcTop,calcLeft, fontScaler);
        console.log(cookie);
        setCookie("vor.carltracy.com",cookie,24);

    }
}
</code></pre>
{{< /rawhtml >}}

#### Cookies!
No, these aren't the type of cookies that spy on you. All I'm doing with this one is to store the position of our target 
as well as the font size. I tried to roll my own, but the end of the day [js-cookie](https://github.com/js-cookie/js-cookie) was just way easier. To add js-cookie all I needed to so was add this line to the top of my code:
{{< rawhtml >}}
<pre><code class="language-html">
&lt;script src="https://cdn.jsdelivr.net/npm/js-cookie@3.0.5/dist/js.cookie.min.js">
&lt;/script&gt;
</code></pre>
{{< /rawhtml >}}
Then I can use the "Cookies" object to save and load cookies. I used the javascript JSON encode/decode feature to store the 
data in an array.
{{< rawhtml >}}
<pre><code class="language-javascript">
function createCookieValue(top,left,fs) {
    var obj = new Object();
    obj.top = top;
    obj.left = left;
    obj.fontScaler = fs;
    console.log(fs);
    var ret = JSON.stringify(obj);
    return obj;
}

function setCookie(cname,obj,exdays) {
    Cookies.set(cname,JSON.stringify(obj),{expires: exdays});
}

window.onload = function() {
    var test = Cookies.get("vor.carltracy.com"); // getCookie("vor.carltracy.com");
    var test2 = JSON.parse(test);       

    if ( (typeof test2["top"] !== 'undefined') && (typeof test2["left"] !== 'undefined'))   {
        console.log("Cookie!")
        calcTop = test2["top"];
        calcLeft = test2["left"];
    } else {
        calcTop = 0;
        calcLeft = 0;
    }

    if (typeof test2["fontScaler"] !== 'undefined') {
        fontScaler = test2["fontScaler"];
    } else {
        fontScaler = 24;
    }
    document.getElementById("fontSize").value =   fontScaler;
}
</code></pre>
{{< /rawhtml >}}

#### Running the code!
Now, finally we get run the code! When the user presses the "Run" button on the form it calls the run() function. The run function 
displays our target, adds the dragging ability to the letter so we can reposition it, calculates when it should stop running, 
sets the "running" boolean to true, and calls runSub().

runSub() does one of two things: if we are still running then play the metronome sound and then call itself with the delay that 
was calculated earlier, otherwise play the bong that indicates we are done and exit. 

The stop() function is hooked up to the stop button. It's mostly there so you can start fresh after repositioning the letter 
or to end it if you accidentally set the runtime to something silly.
{{< rawhtml >}}
<pre><code class="language-javascript">
function run() {
    tick = new Audio('/metronome.mp3');
    console.log(timeLimit);
    document.getElementById("theletter").style.display="block";
    dragElement(document.getElementById("drag"));

    var dt = new Date();
    endTime = new Date(dt.getTime() + 1000 * timeLimit);  
    console.log('run');
    running = true;
    runSub();
    
}

function runSub() {
    console.log("Current time: " + Date.now().toString() + " :: End Time: " + endTime.toString() );
    if (Date.now() > endTime) {
        running = false;
        document.getElementById("theletter").style.display="none";
        booong.play();
    }
    if (running) { 
        tick.play();
        setTimeout(runSub, delay);
    } 
}

function stop() {
    document.getElementById("theletter").style.display="none";
    running = false;

    var cookie = createCookieValue(calcTop,calcLeft, fontScaler);
    console.log(fontScaler);
    setCookie("vor.carltracy.com",cookie,24);


}
</code></pre>


<script>
    hljs.highlightAll();
</script>
{{< /rawhtml >}}



