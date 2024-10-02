---
title: "Craft Whiskey Festival Countdown"
date: 2024-10-02T14:39:00-05:00
draft: false
tags : [nerd,physical therapy]
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

# Is It [Whiskey Festival](https://www.henmick.com/ohio-craft-whiskey-festival) Time???




<!--more--> 

{{< rawhtml >}}
<div id="WHISKEY"> &nbsp; </div>

<script>

    var whiskey = document.getElementById("WHISKEY");


    const date1 = new Date('October 19, 2024 00:00:00');
    //const date1 = new Date('October 19, 2023 00:00:00');
    if (Date.now() > date1) {
        whiskey.innerHTML = "YES &#128516;"
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



    } else {
        whiskey.innerHTML = "No &#128542;";

    }

</script>
{{< /rawhtml >}}






