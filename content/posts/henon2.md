---
title: "Hénon map"
date: 2024-03-15T11:39:00-05:00
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
## Source Code
I put the basic source code in [this gist](https://gist.github.com/chaos46692/f9d00cef34f9193c17c32477fc647506)

## Some interesting parameters
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
/* FFT code from from https://github.com/indutny/fft.js/ 
FFT allows us to calculate the base period of the itteration
*/
var FFTJS=function(t){function r(e){if(i[e])return i[e].exports;var o=i[e]={i:e,l:!1,exports:{}};return t[e].call(o.exports,o,o.exports,r),o.l=!0,o.exports}var i={};return r.m=t,r.c=i,r.i=function(t){return t},r.d=function(t,i,e){r.o(t,i)||Object.defineProperty(t,i,{configurable:!1,enumerable:!0,get:e})},r.n=function(t){var i=t&&t.__esModule?function(){return t.default}:function(){return t};return r.d(i,"a",i),i},r.o=function(t,r){return Object.prototype.hasOwnProperty.call(t,r)},r.p="",r(r.s=0)}([function(t,r,i){"use strict";function e(t){if(this.size=0|t,this.size<=1||0!=(this.size&this.size-1))throw new Error("FFT size must be a power of two and bigger than 1");this._csize=t<<1;for(var r=new Array(2*this.size),i=0;i<r.length;i+=2){var e=Math.PI*i/this.size;r[i]=Math.cos(e),r[i+1]=-Math.sin(e)}this.table=r;for(var o=0,n=1;this.size>n;n<<=1)o++;this._width=o%2==0?o-1:o,this._bitrev=new Array(1<<this._width);for(var s=0;s<this._bitrev.length;s++){this._bitrev[s]=0;for(var a=0;a<this._width;a+=2){var h=this._width-a-2;this._bitrev[s]|=(s>>>a&3)<<h}}this._out=null,this._data=null,this._inv=0}t.exports=e,e.prototype.fromComplexArray=function(t,r){for(var i=r||new Array(t.length>>>1),e=0;e<t.length;e+=2)i[e>>>1]=t[e];return i},e.prototype.createComplexArray=function(){for(var t=new Array(this._csize),r=0;r<t.length;r++)t[r]=0;return t},e.prototype.toComplexArray=function(t,r){for(var i=r||this.createComplexArray(),e=0;e<i.length;e+=2)i[e]=t[e>>>1],i[e+1]=0;return i},e.prototype.completeSpectrum=function(t){for(var r=this._csize,i=r>>>1,e=2;e<i;e+=2)t[r-e]=t[e],t[r-e+1]=-t[e+1]},e.prototype.transform=function(t,r){if(t===r)throw new Error("Input and output buffers must be different");this._out=t,this._data=r,this._inv=0,this._transform4(),this._out=null,this._data=null},e.prototype.realTransform=function(t,r){if(t===r)throw new Error("Input and output buffers must be different");this._out=t,this._data=r,this._inv=0,this._realTransform4(),this._out=null,this._data=null},e.prototype.inverseTransform=function(t,r){if(t===r)throw new Error("Input and output buffers must be different");this._out=t,this._data=r,this._inv=1,this._transform4();for(var i=0;i<t.length;i++)t[i]/=this.size;this._out=null,this._data=null},e.prototype._transform4=function(){var t,r,i=this._out,e=this._csize,o=this._width,n=1<<o,s=e/n<<1,a=this._bitrev;if(4===s)for(t=0,r=0;t<e;t+=s,r++){var h=a[r];this._singleTransform2(t,h,n)}else for(t=0,r=0;t<e;t+=s,r++){var f=a[r];this._singleTransform4(t,f,n)}var u=this._inv?-1:1,_=this.table;for(n>>=2;n>=2;n>>=2){s=e/n<<1;var l=s>>>2;for(t=0;t<e;t+=s)for(var p=t+l,v=t,c=0;v<p;v+=2,c+=n){var d=v,m=d+l,y=m+l,b=y+l,w=i[d],g=i[d+1],z=i[m],T=i[m+1],x=i[y],A=i[y+1],C=i[b],E=i[b+1],F=w,I=g,M=_[c],R=u*_[c+1],O=z*M-T*R,P=z*R+T*M,j=_[2*c],S=u*_[2*c+1],J=x*j-A*S,k=x*S+A*j,q=_[3*c],B=u*_[3*c+1],D=C*q-E*B,G=C*B+E*q,H=F+J,K=I+k,L=F-J,N=I-k,Q=O+D,U=P+G,V=u*(O-D),W=u*(P-G),X=H+Q,Y=K+U,Z=H-Q,$=K-U,tt=L+W,rt=N-V,it=L-W,et=N+V;i[d]=X,i[d+1]=Y,i[m]=tt,i[m+1]=rt,i[y]=Z,i[y+1]=$,i[b]=it,i[b+1]=et}}},e.prototype._singleTransform2=function(t,r,i){var e=this._out,o=this._data,n=o[r],s=o[r+1],a=o[r+i],h=o[r+i+1],f=n+a,u=s+h,_=n-a,l=s-h;e[t]=f,e[t+1]=u,e[t+2]=_,e[t+3]=l},e.prototype._singleTransform4=function(t,r,i){var e=this._out,o=this._data,n=this._inv?-1:1,s=2*i,a=3*i,h=o[r],f=o[r+1],u=o[r+i],_=o[r+i+1],l=o[r+s],p=o[r+s+1],v=o[r+a],c=o[r+a+1],d=h+l,m=f+p,y=h-l,b=f-p,w=u+v,g=_+c,z=n*(u-v),T=n*(_-c),x=d+w,A=m+g,C=y+T,E=b-z,F=d-w,I=m-g,M=y-T,R=b+z;e[t]=x,e[t+1]=A,e[t+2]=C,e[t+3]=E,e[t+4]=F,e[t+5]=I,e[t+6]=M,e[t+7]=R},e.prototype._realTransform4=function(){var t,r,i=this._out,e=this._csize,o=this._width,n=1<<o,s=e/n<<1,a=this._bitrev;if(4===s)for(t=0,r=0;t<e;t+=s,r++){var h=a[r];this._singleRealTransform2(t,h>>>1,n>>>1)}else for(t=0,r=0;t<e;t+=s,r++){var f=a[r];this._singleRealTransform4(t,f>>>1,n>>>1)}var u=this._inv?-1:1,_=this.table;for(n>>=2;n>=2;n>>=2){s=e/n<<1;var l=s>>>1,p=l>>>1,v=p>>>1;for(t=0;t<e;t+=s)for(var c=0,d=0;c<=v;c+=2,d+=n){var m=t+c,y=m+p,b=y+p,w=b+p,g=i[m],z=i[m+1],T=i[y],x=i[y+1],A=i[b],C=i[b+1],E=i[w],F=i[w+1],I=g,M=z,R=_[d],O=u*_[d+1],P=T*R-x*O,j=T*O+x*R,S=_[2*d],J=u*_[2*d+1],k=A*S-C*J,q=A*J+C*S,B=_[3*d],D=u*_[3*d+1],G=E*B-F*D,H=E*D+F*B,K=I+k,L=M+q,N=I-k,Q=M-q,U=P+G,V=j+H,W=u*(P-G),X=u*(j-H),Y=K+U,Z=L+V,$=N+X,tt=Q-W;if(i[m]=Y,i[m+1]=Z,i[y]=$,i[y+1]=tt,0!==c){if(c!==v){var rt=N,it=-Q,et=K,ot=-L,nt=-u*X,st=-u*W,at=-u*V,ht=-u*U,ft=rt+nt,ut=it+st,_t=et+ht,lt=ot-at,pt=t+p-c,vt=t+l-c;i[pt]=ft,i[pt+1]=ut,i[vt]=_t,i[vt+1]=lt}}else{var ct=K-U,dt=L-V;i[b]=ct,i[b+1]=dt}}}},e.prototype._singleRealTransform2=function(t,r,i){var e=this._out,o=this._data,n=o[r],s=o[r+i],a=n+s,h=n-s;e[t]=a,e[t+1]=0,e[t+2]=h,e[t+3]=0},e.prototype._singleRealTransform4=function(t,r,i){var e=this._out,o=this._data,n=this._inv?-1:1,s=2*i,a=3*i,h=o[r],f=o[r+i],u=o[r+s],_=o[r+a],l=h+u,p=h-u,v=f+_,c=n*(f-_),d=l+v,m=p,y=-c,b=l-v,w=p,g=c;e[t]=d,e[t+1]=0,e[t+2]=m,e[t+3]=y,e[t+4]=b,e[t+5]=0,e[t+6]=w,e[t+7]=g}}]);

// https://stackoverflow.com/questions/17242144/javascript-convert-hsb-hsv-color-to-rgb-accurately/54024653#54024653
    // input: h in [0,360] and s,v in [0,1] - output: r,g,b in [0,1]
    function hsv2rgb(h,s,v) 
    {                              
    let f= (n,k=(n+h/60)%6) => v - v*s*Math.max( Math.min(k,4-k,1), 0);     
    return [f(5)*255,f(3)*255,f(1)*255];       
    } 

    function plotjshsv2rgb(h,s,v) {
        var x = hsv2rgb(h,s,v)
        var ret = ''
    }

    var a = 1.4;
    var b = 0.3; 
    const aMult = 0.02;
    const bMult = 0.005;
    var thechart = 0;

    function setValues(a1,b1) {
        setA2(a1);
        setB2(b1);

        document.getElementById("lbla").value = a1;
        document.getElementById("lblb").value = b1;
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

