---
title: " Vestibulo-ocular Reflex Therapy"
date: 2024-08-09T14:39:00-05:00
draft: false
tags : [nerd,physical therapy]
categories : [nerd]
chart: false
---

{{< rawhtml >}}
<script src="
https://cdn.jsdelivr.net/npm/js-cookie@3.0.5/dist/js.cookie.min.js
"></script>

<style>
    .target {
        font-size:36pt;
        font-weight:bold;
        position:sticky;
        border: none; /* solid 1px red; */
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

    .target button {
        font-size:12pt;
    }

    label, button {
        font-weight:bold;
    }

    pre {
        font-size:80%;
    }

    .container {
        position:absolute;
        top:0;
        left:0;
        width:100%;
        height:100%;
        border: 1px solid blue;
    }

    #note {
        font-size:12pt;
        font-weight:normal;
        color:black;
        position:absolute;
        top:0;
        left:0;
    }

    #drag{
        border:none; 
        display:block;
        position:absolute;
    }

    #TargetSpan{
        border: none;
    }
  </style>
<!--
<div class='target' id="theletter">A
<br>
<form>
<button type="button" onclick="stop();">Stop</button>
</form>
</div>
-->
{{< /rawhtml >}}

## VOR Physical therapy

I recently have had some vestibular issues related to how my [brain and eyes communicate](https://www.physio-pedia.com/Vestibulo-Ocular_Reflex) (badly). My issues are basically due to my gaze not locking correctly / some issues with peripheral vision. Once the source of the issues were identified I was given some exercises to do to help with the gaze stabilization. 
<!--more--> 

The exercise basically is staring at a letter that is straight in front of me while turning my head side to side
[like in this video](https://youtu.be/Mk7v9r4acQU?t=236). Now I could just download a metronome app for my phone that's going to steal 
my personal information, or I could have way more fun building a javascript app to do it. So I build a javascript app because as we all 
know I'm a big giant nerd.

## Behold my invincible nuclear VOR App!
Yeah it looks like shit, whatever. It works.

{{< rawhtml >}}
<form>
    <!-- <button  type="button" onclick="javascript:runProj();">Run</button > <br/> -->
    <label>Metronome tempo</label><br/>
    <input type="range" min="30" max="150" value="70" class="slider" id="tempo" onchange="settempo(this.value)" style="width:18em">
    <br/>
    <input type="text" id="tempoText" value="70" onchange="settempo2(this.value)"></input> bpm
    <br/>
    <br/>
    <label>Total Time</label><br/>
    <input type="text" id="timelimit" value="60" onchange="setTime(this.value)"></input> seconds
    <br/>
    <br/>
    <label>Font Scale</label><br/>
    <input type="text" id="fontSize" value="100" onchange="setFontScale(this.value)"></input> pt
    <br/>
    <br/>
    <button type="button" onclick="run();">Run</button>
    <button type="button" onclick="stop();">Stop</button>
    <!-- <button type="button" onclick="show();">Show!</button> -->
</form>

<script>
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
    //audio.play();

    function isNumber(value) {
    return typeof value === 'number';
    }

    function settempo(v) {
        tempo = v
        delay = 1000.0 * 60.0 / tempo - 24.0;
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
  
    function run() {
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

    function show() {
        document.getElementById("theletter").style.display="block";
        dragElement(document.getElementById("drag"));
        console.log(document.getElementById("TargetSpan").offsetWidth)
    }

    function stop() {
        document.getElementById("theletter").style.display="none";
        running = false;

        var cookie = createCookieValue(calcTop,calcLeft, fontScaler);
        console.log(fontScaler);
        setCookie("vor.carltracy.com",cookie,24);


    }

    
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
    //console.log(obj);
    Cookies.set(cname,JSON.stringify(obj),{expires: exdays});
   }

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


    window.onload = function() {
        var test = Cookies.get("vor.carltracy.com"); // getCookie("vor.carltracy.com");
        //console.log("COOKIE!");
        //console.log(test);
        var test2 = JSON.parse(test);
        //console.log(test2);
        

        if ( (typeof test2["top"] !== 'undefined') && (typeof test2["left"] !== 'undefined'))   {
            console.log("Cookie!")
            calcTop = test2["top"];
            calcLeft = test2["left"];
        } else {
            calcTop = 0;
            calcLeft = 0;
            console.log("No cookie!");
        }

        if (typeof test2["fontScaler"] !== 'undefined') {
            fontScaler = test2["fontScaler"];
        } else {
            fontScaler = 24;
        }
        document.getElementById("fontSize").value =   fontScaler

        

    }
/*
    function findFirstDescendant(parent, tagname)
    {
        parent = document.getElementById(parent);
        var descendants = parent.getElementsByTagName(tagname);
        if ( descendants.length )
            return descendants[0];
        return null;
    }
*/
    var parent = document.body;  
    var div = document.createElement('div');
    div.classList.add('target');
    div.id='theletter'
    div.innerHTML = '<div class="container"><div id="note">Drag the "A" so that it is directly in front of your eyes. The page will save the position in a cookie for the next time you visit!</div>'+
        '<div id="drag"><span id="TargetSpan">A</span><br/><form> <button type="button" onclick="stop();">Stop</button></form></div></div>'
    parent.insertBefore(div, parent.firstChild);    

</script>


{{< /rawhtml >}}

## So how does it work?
The metronome is not all that complicated. Basically we just need to calculate the delay in milliseconds between ticks, which will be used later with a javascript setTimeout call using that delay
{{< rawhtml >}}

<pre>
function settempo(v) {
    tempo = v
    delay = 1000.0 * 60.0 / tempo -24.0;
    document.getElementById("tempoText").value = tempo;
}
</pre>

{{< /rawhtml >}}

the parameter v is just the value coming from the form inputs. delay is the timeout between clicks, the -24 is to account for the 
length of the clicking audio clip. That probably doesn't make a huge difference, but if we're going to do it we might as well do it right.

## DIVs are a pain in my ass
The absolute biggest problem in building this was getting the damn div to take up the entire screen. The first thing we need is for the 
div to stick to the top left and take up the entire screen.
{{< rawhtml >}}

<pre>
&lt;style&gt;
.target {
    position:sticky;
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

&lt;/style&gt;
</pre>

{{< /rawhtml >}}
That gives me most of what I want:
* puts the div at the top left
* puts it in front (z-index) and makes the background white
* takes up the entire screen 
* puts the first element right in the center with the text-align and padding-top
* * calc(50hv - 30pt) is being extra nerdy - because I'm using 60pt font the -30pt gets it (mostly) centered vertically
* display:none ensures that it doesn't pop up until the Run button is clicked.

The problem with this is that the position:sticky puts it within the containing div, which if I just add it to the top of my article 
doesn't really work. I need it to be at the very tippity top of the body before any other html. To do that I can't just put it in the article, I need to insert it with javascript:
{{< rawhtml >}}

<pre>
var parent = document.body;  
var div = document.createElement('div');
div.classList.add('target');
div.id='theletter'
div.innerHTML = 'A&lt;br/&gt;&lt;form&gt;' +
 &lt;button type="button" onclick="stop();"&gt;Stop&lt;/button&gt;&lt;/form&gt;'
parent.insertBefore(div, parent.firstChild);

</pre>

{{< /rawhtml >}}
The "A" is the target that I'm staring at when it runs and the stop button is just in case I need to stop for whatever reason. 

The rest is just calculating how long it should run, showing the div with our target and stop button, then calling a subroutine 
which calls itself every "delay" milliseconds until we reach the total time. Of course then we have to play a ridiculous bell sound.
{{< rawhtml >}}
<pre>
var running = false;
function run() {
    console.log(timeLimit);
    document.getElementById("theletter").style.display="block";

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
</pre>
{{< /rawhtml >}}

Overall it works pretty well for what I need. I even learned some fun CSS stuff along the way.


