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
        border: none; 
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

    .point:hover {
        cursor:pointer;
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


The exercise basically is staring at a letter that is straight in front of me while turning my head side to side
[like in this video](https://youtu.be/Mk7v9r4acQU?t=236). Now I could just download a metronome app for my phone that's going to steal 
my personal information, or I could have way more fun building a javascript app to do it. So I build a javascript app because as we all 
know I'm a big giant nerd.

You can [read more about how I built it here]({{<ref "vor-app-build">}}) test
<!--more--> 
{{< rawhtml >}}
<script>
    var mutants = new Audio('/inm.mp3');

    function playMutants() {
        mutants.play();
    }
</script>

<h2 class="point" onClick='playMutants()'>Behold my invincible nuclear VOR App!</h2>
{{< /rawhtml >}}
Yeah not the sexiest thing I've ever built, whatever. It works.

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
    var updatingFontSize = false;

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
        delay = 1000.0 * 60.0 / tempo; // - 24.0;
        document.getElementById("tempo").value = tempo;
        //runProj(); 
    }

    function setFontScale(v) {
        if (updatingFontSize) {return;}

        fontScaler = parseInt(v);
        //console.log(fontScaler);
        document.getElementById("TargetSpan").style.fontSize = v + "pt"
    }

    function setFontScale2(v) {
        //alert(v);
        //setFontScale(v);
        fontScaler = v;
        updatingFontSize = true;
        document.getElementById("TargetSpan").style.fontSize = v + "pt"
        document.getElementById("fontSize").value = v;
        updatingFontSize = false;
    }

    function setTime(v) {
        //console.log(v);
        timeLimit = v;
    }    
  
    function run() {
        keycapon();
        disableScroll();
        //console.log(timeLimit);
        document.getElementById("theletter").style.display="block";
        dragElement(document.getElementById("drag"));
        //console.log('run');
        running = true;

        tick = new Audio('/metronome.mp3');
        tick.addEventListener("ended",runFirst);
        tick.play();
    }

    function runFirst() {
        //tick.onended = nothing;
        tick.removeEventListener("ended",runFirst);
        var dt = new Date();
        endTime = new Date(dt.getTime() + 1000 * timeLimit);  
        //runSub();
        setTimeout(runSub, delay);

    }

    function runSub() {
        //console.log("Current time: " + Date.now().toString() + " :: End Time: " + endTime.toString() );
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
        //console.log(document.getElementById("TargetSpan").offsetWidth);
    }

    function stop() {
        keycapoff();
        document.getElementById("theletter").style.display="none";
        running = false;

        updatingFontSize = true;
        //document.getElementById("TargetSpan").style.fontSize = fontScaler + "pt"
        document.getElementById('fontSize').value = fontScaler;
        updatingFontSize = false;


        var cookie = createCookieValue(calcTop,calcLeft, fontScaler);
        //console.log(fontScaler);
        setCookie("vor.carltracy.com",cookie,24);
        enableScroll();


    }

    
    function createCookieValue(top,left,fs) {
        var obj = new Object();
        obj.top = top;
        obj.left = left;
        obj.fontScaler = fs;
        //console.log(fs);
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
            //console.log(calcLeft);
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

            //console.log(calcTop);
            //console.log(calcLeft);
            var cookie = createCookieValue(calcTop,calcLeft, fontScaler);
            //console.log(cookie);
            setCookie("vor.carltracy.com",cookie,24);

        }
    }

    // capture keydown events
    function keycapon() {
        document.addEventListener('keydown', keycapcallback);

    }

    function keycapoff() {
        document.removeEventListener('keydown',keycapcallback);
    }

    function keycapcallback(event) {

        kc = event.keyCode;
        //console.log(kc);

        if (kc == 38) {
            setFontScale2(parseInt(fontScaler) + 1);
        }
        if (kc== 40) {
            setFontScale2(parseInt(fontScaler) - 1);
        }
        if (kc== 27) {
            stop();
            return;
        }

        var cookie = createCookieValue(calcTop,calcLeft, fontScaler);
        //console.log(cookie);
        setCookie("vor.carltracy.com",cookie,24);

    }

    var yOffset = 0;
    function disableScroll() {
        //alert('disable scroll');
        console.log("disable scroll");
        console.log(window.pageYOffset);
        yOffset = window.pageYOffset;
        // Get the current page scroll position
        scrollTop =
            window.pageYOffset ||
            document.documentElement.scrollTop;
        scrollLeft =
            window.pageXOffset ||
            document.documentElement.scrollLeft,

            // if any scroll is attempted,
            // set this to the previous value
            window.onscroll = function () {
                window.scrollTo(scrollLeft, scrollTop);
            };
    }

    function enableScroll() {
        //console.log(window.pageYOffset);
        //window.pageYOffset = yOffset;  
        window.scrollTo(0,yOffset);    
        console.log(window.pageYOffset);

        window.onscroll = function () { };

    }

    window.onload = function() {
        var test = Cookies.get("vor.carltracy.com"); // getCookie("vor.carltracy.com");
        var test2 = JSON.parse(test);       

        if ( (typeof test2["top"] !== 'undefined') && (typeof test2["left"] !== 'undefined'))   {
            //console.log("Cookie!")
            calcTop = test2["top"];
            calcLeft = test2["left"];
        } else {
            calcTop = 0;
            calcLeft = 0;
            //console.log("No cookie!");
        }

        if (typeof test2["fontScaler"] !== 'undefined') {
            fontScaler = test2["fontScaler"];
        } else {
            fontScaler = 24;
        }
        setFontScale2(fontScaler);

        //document.getElementById("fontSize").value =   fontScaler;
    }

    var parent = document.body;  
    var div = document.createElement('div');
    div.classList.add('target');
    div.id='theletter'
    div.innerHTML = '<div class="container"><div id="note"><p>Drag the "A" so that it is directly in front of your eyes. The page will save the position in a cookie for the next time you visit!</p>' + 
    '<p><ul style="text-align:left"><li>Use the up arrow to increase font size</li>' + 
    '<li>Use the down arrow to decrease font size</li>' + 
    '<li>Use the escape key or the stop button to stop</li></ul></p>' + 
    '<div style="clear:both"><form style="text-align:left"> <button type="button" onclick="stop();">Stop</button></form></div>' + 
        '<div id="drag"><span id="TargetSpan">A</span><br/></div></div>'
    '</div>'+
    parent.insertBefore(div, parent.firstChild);    

    updatingFontSize = false;
</script>


{{< /rawhtml >}}





