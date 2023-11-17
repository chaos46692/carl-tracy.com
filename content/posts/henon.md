---
title: "Hénon map"
date: 2023-11-16T17:39:00-05:00
draft: true
tags : [nerd,math,chaos]
categories : [nerd,math]
chart: true
---

## Building a Hénon map with javascript
I first found out about the Hénon map as an undergrad and have loved it ever since. Can I simulate it in javascript? Let's find out!

The [Hénon map](https://en.wikipedia.org/wiki/H%C3%A9non_map) is a dynamical system that exhibits chaotic behavior. It takes two 
parameters a and b. Depending on the values the map can exhibit 
{{<rawhtml>}} <a href="javascript:setValues(0.5,0.2);">periodic</a> 
, <a href="javascript:setValues(1.4,0.2);">chaotic</a>  or intermittent {{</rawhtml>}} (switching back and forth between 
chaotic and stable orbits).  It is described by the formula:

$$ 
\begin{aligned}  
x_{n+1} &=  1 - ax_n^2+y_n\\\\
y_{n+1}  &= bx_n 
\end{aligned}  
$$ 

The Hénon map is basically taking the x-y plane and stretches, folds and squishes it like kneading bread. For some parameters it will 
tend toward a fixed point or set of points, but for others it exhibits chaotic dynamics - which means that no matter how accurately 
you describe the current position, it will be impossible to predict where it will be in the future (other than within the bounds 
of the attractor).
<!--more--> 
Some interesting parameters:
{{<rawhtml>}}
<ul>
    <li> <a href="javascript:setValues(1.4,0.2);"> Chaotic (1.4, 0.3)</a> </li>
    <li> <a href="javascript:setValues(0.5,0.2);">Period 2 (0.5,0.2)</a> </li>
    <li> <a href="javascript:setValues(1.01,0.3);">Period 4 (1.01,0.3)</a> </li>
    <li> <a href="javascript:setValues(0.97,0.5);"> Period 7 (0.97, 0.5)</a> </li>
</ul>
{{</rawhtml>}} 


{{< rawhtml >}}
<script
src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.4/Chart.js">
</script>


<form>
    <!-- <button  type="button" onclick="javascript:runProj();">Run</button > <br/> -->
    <label>a=</label>
    <input type="range" min="1" max="100" value="70" class="slider" id="henonA" onchange="setA(this.value)" style="width:18em">
    <input type="text" id="lbla" value="1.4" onchange="setA2(this.value)"></input>
    <br/>
    <label>b=</label><input type="range" min="1" max="100" value="30" class="slider" id="henonB" onchange="setB(this.value)"  style="width:18em">
    <input type="text" id="lblb" value="0.3" onchange="setB2(this.value)"></input><br/>
</form>

<div id="Warn" style="color:red"> &nbsp; </div>
<canvas id="plot" style="width:600px; "></canvas>


<script>
    var a = 1.4;
    var b = 0.3; 
    const aMult = 0.02;
    const bMult = 0.005;
    var thechart = 0;

    function setValues(a1,b1) {
        setA2(a1);
        setB2(b1);
    }

    function isNumber(value) {
    return typeof value === 'number';
    }

    function setA(v) {
        a = v * aMult;
        document.getElementById("lbla").value = a;
        runProj();
    }

    function setB(v) {
        b = v * bMult;
        document.getElementById("lblb").value = b;
        runProj();
    }

    function setA2(v) {
        a = v;
        document.getElementById("henonA").value = a / aMult;
        runProj();
    }

    function setB2(v) {
        b = v;
        document.getElementById("henonB").value = b / bMult;
        runProj();
    }    


    function projectHenon(a,b) {
        var xx = Math.random() / 100.0;
        var yy = Math.random() / 100.0;
        var ret = [];

        var i;
        var tmp
        // run for a bit to approach the attractor
        for(i=1;i<5000;i++) {
            tmp = 1.0 - a * xx*xx + yy;
            yy = b * xx;
            xx = tmp;
        }

        var pt;
        for(i=1;i<1000;i++) {
            tmp = 1.0 - a * xx*xx + yy;
            yy = b * xx;
            xx = tmp;

            pt = {x: xx, y: yy};
            ret.push(pt);
        }

        if (Math.abs(xx) > 100) {
            document.getElementById("Warn").innerHTML = "Projection goes to infinity";
            //Warn
        } else {
            document.getElementById("Warn").innerHTML = "&nbsp;";
        }

        return ret;

    }


    function runProj() {
        //alert("CLEAR!");
        var chrt  = document.getElementById("plot").getContext("2d");
        //var data = [{x:1,y:2},{x:10,y:-2},{x:5,y:3}];
        var mapdat = projectHenon(a,b);
        //console.log(mapdat);
        const data = {
            datasets: [{
                data: mapdat,
                backgroundColor: 'rgb(0,0,220)'
                , radius: 1
                , color: 'rgb(255,255,255)'
            }],
        };        
        var config = {
            type: 'scatter',
            data: data,
            options: {
                tooltips: {enabled: false},
                hover: {mode: null},
                responsive: true,
                maintainAspectRatio: true,     
                aspectRatio: 1.0,           
                scales: {
                    yAxes : [{
                        ticks : {
                            max : 1.5,    
                            min : -1.5
                        }
                    }],
                    xAxes : [{
                        ticks : {
                            max : 1.5,    
                            min : -1.5
                        }
                    }],

                },                

                radius:1,
                legend: {
                    display:false
                }
            }
        };
        //var chartID = new Chart(chrt , config);
        if (thechart) {
            thechart.destroy();
            //var ctx = (a canvas context);
            chrt.width  = 600; // window.innerWidth;
            chrt.height = 600; //window.innerHeight;

        }
        thechart = new Chart(chrt , config);
    }



    setValues(1.4,0.3);
</script>



{{< /rawhtml >}}