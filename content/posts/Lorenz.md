---
title: "Lorentz attractor"
date: 2060-11-01T10:39:00-05:00
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


## Behold my Lorenz attractor!
The [Lorenz system](https://en.wikipedia.org/wiki/Lorenz_system) was one of the dynamical systems that I really loved 

$$ 
\begin{aligned}  
\frac{dx}{dt} &= \sigma \left( y - x \right) \\\\
\frac{dy}{dt} &= x \left( \rho - z \right) - y \\\\
\frac{dz}{dt} &= xy - \beta z
\end{aligned}  
$$ 