---
title: "Craft Whiskey Festival Countdown"
date: 2024-10-02T14:39:00-05:00
draft: false
tags : [nerd,whiskey]
categories : [nerd]
chart: false
---

{{< rawhtml >}}
<script src="https://cdn.jsdelivr.net/npm/@tsparticles/confetti@3.0.3/tsparticles.confetti.bundle.min.js"></script>

<style>
    #WHISKEY {
        font-size: 40pt;
        color: black;

    }
</style> 
{{< /rawhtml >}}

# Is It Whiskey Festival Time???
Is it time for the [Ohio Craft Whiskey Festival](https://www.henmick.com/ohio-craft-whiskey-festival)??



<!--more--> 

{{< rawhtml >}}
<div id="WHISKEY"> &nbsp; </div>
<div id="NOTE"> &nbsp; </div>

<script> 

    var whiskey = document.getElementById("WHISKEY");
    var note = document.getElementById("NOTE");
    

    const date1 = new Date('October 18, 2024 00:00:00');
    const date2 = new Date('October 21, 2024 00:00:00')
    //const date1 = new Date('October 18, 2023 00:00:00');
    
    if (Date.now() > date2) {
        whiskey.innerHTML = '<p>The whiskey festival is over &#128542; </p>'
        note.innerHTML = ' '
    }
    if ((Date.now() > date1) && (Date.now() < date2)) {
        whiskey.innerHTML = '<p>YES &#128516; </p>'
        note.innerHTML = ' <p>The <a href="https://www.henmick.com/ohio-craft-whiskey-festival" target="_blank">Ohio Craft Whiskey Festival </a> is this weekend. <b>Start driving Louise! </b></p>'
        const duration = 20 * 1000,
        animationEnd = Date.now() + duration,
        defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 0 };

        function randomInRange(min, max) {
        return Math.random() * (max - min) + min;
        }

        const interval = setInterval(function() {
        const timeLeft = animationEnd - Date.now();

        if (timeLeft <= 0) {
            return clearInterval(interval);
        }

        const particleCount = 50 * (timeLeft / duration);

        // since particles fall down, start a bit higher than random
        confetti(
            Object.assign({}, defaults, {
            particleCount,
            origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.2 },
            })
        );
        confetti(
            Object.assign({}, defaults, {
            particleCount,
            origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.2 },
            })
        );
        }, 250);



    } 
    
    if (Date.now() < date1)    {
        whiskey.innerHTML = "No &#128542;";

    }

</script>
{{< /rawhtml >}}






