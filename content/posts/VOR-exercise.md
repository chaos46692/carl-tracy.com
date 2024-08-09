---
title: "Vestibular Ocular Reflex Therapy"
date: 2024-08-09T14:39:00-05:00
draft: false
tags : [nerd,physical therapy]
categories : [nerd]
chart: true
---

## VOR Physical therapy

I recently have had some vestibular issues related to how my brain and eyes communicate (badly). One of my physical therapy tasks involves 
staring at a letter and turning my head side to side in time with a metronome for a certain period of time. The stupid little script lets 
me do that on my computer. Hooray!
<!--more--> 

{{< rawhtml >}}
<style>
    .target {
        font-size:60pt;
        position:absolute;
        border: solid 1px red;
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
  </style>
<div class='target' id="theletter">A
<br>
<form>
<button type="button" onclick="stop();">Stop</button>
</form>
</div>
<form>
    <!-- <button  type="button" onclick="javascript:runProj();">Run</button > <br/> -->
    <label>Metronome Speed=</label>
    <input type="range" min="30" max="150" value="70" class="slider" id="speed" onchange="setSpeed(this.value)" style="width:18em">
    <input type="text" id="speedText" value="70" onchange="setSpeed2(this.value)"></input> bpm
    <br/>
    <br/>
    <label>Total Time=</label>
    <input type="text" id="timelimit" value="3.0" onchange="setTime(this.value)"></input> minutes
    <br/>
    <br/>
    <button type="button" onclick="run();">Run</button>
    <button type="button" onclick="stop();">Stop</button>
</form>

<script>
    var running = false;
    var timeLimit = 3.0;
    var speed = 70;
    var delay = 1000.0 * 60.0 / speed;
    var tick = new Audio('/metronome.mp3');
    var booong = new Audio('/boooong.mp3');
    var endTime = Date.now();
    //audio.play();

    function isNumber(value) {
    return typeof value === 'number';
    }

    function setSpeed(v) {
        speed = v
        delay = 1000.0 * 60.0 / speed;
        document.getElementById("speedText").value = speed;
        //runProj();
    }


    function setSpeed2(v) {
        speed = v;
        delay = 1000.0 * 60.0 / speed;
        document.getElementById("speed").value = speed;
        //runProj();
    }

    function setTime(v) {
        timiLimit = v;
    }    
 
    function run() {
        document.getElementById("theletter").style.display="block";
        var dt = new Date();
        endTime = new Date(dt.getTime() + 1000 * timeLimit); // Date.now() + timeLimit*60000;
        console.log('run');
        running = true;
        runSub();
    }

    function runSub() {
        //console.log('runSub');
        console.log("Current time: " + Date.now().toString() + " :: End Time: " + endTime.toString() );
        if (Date.now() > endTime) {
            running = false;
            document.getElementById("theletter").style.display="none";
            booong.play();
        }
        if (running) { 
            tick.play();
            setTimeout(runSub, delay);
        } else {

        }
    }

    function stop() {
        document.getElementById("theletter").style.display="none";
        running = false;
    }


</script>

{{< /rawhtml >}}
