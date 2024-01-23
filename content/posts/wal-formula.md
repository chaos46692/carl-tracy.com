---
title: "Alternative formulation of Weighted Average Life"
date: 2024-01-22
draft: false
tags : [finance]
categories : [nerd]
---
Normally when folks calculate an asset's weighted average life they use the formula
$$
WAL = {{\sum_n{n \cdot Prin_n}} \over Par_0} 
$$
where
$$
Par_n = \hbox{Par value at time n} \\\\[7pt]
$$
and
$$
Prin_n = \hbox{Principal payment at time n} \\\\[7pt]
$$


In [a previous blog post]({{< ref "mortgage-wal-formula.md" >}} ) I had presented this formula 
$$
WAL = {\sum{Par_n} \over Par_0} \\\\
$$
without any context, other than to leave it to the reader to prove. <!--more--> I first encountered this formula in a [Milliman](https://integrate.milliman.com/en/) 
MG-ALFA model, and it took me a bit to understand how it works. Computationally it is much, _much_ easier to calculate than the traditional way and that 
sometimes makes it easier to use when calculating closed form solutions to things. I certainly hadn't seen it before (or at least had completely forgotten 
that I had) and I think that's mostly because while it's computationally convenient, it doesn't give folks much of a feel for what the weighted average life 
actually _means_, whereas the "traditional" one is much clearer about what the intention of the formula is - the average time the asset will be around.

### Derivation of the formula
If we let $Prin_n$ be the principal payment at the beginning of time n then:

$$
\begin{aligned}
WAL \cdot Par_0 =& \sum_{n=1}^{N}{n \cdot Prin_n} \\\\[7pt]
    =& Prin_1 + 2 Prin_2 + 3 Prin_3 + \ldots \\\\[7pt]
\end{aligned}
$$
rearranging the terms and recognizing that $\sum_{i=n}^N{Prin_i}$ is just the principal remaining at the beginning of time i, which is $Par_i$:
$$
\begin{aligned}
WAL= & Prin_1 &+ Prin_2&+ Prin_3 &+ Prin_4 & + \ldots & + Prin_N  & \qquad \left( = Par_0 \right) \\\\[7pt]
    &  & + Prin_2& + Prin_3 &+ Prin_4 & + \ldots & + Prin_N& \qquad \left( = Par_1 \right) \\\\[7pt]
    &  &  & + Prin_3 &+ Prin_4 & + \ldots & + Prin_N & \qquad \left( = Par_2 \right) \\\\[7pt]
    &  &  &   &+ Prin_4 & + \ldots & + Prin_N & \qquad \left( = Par_3 \right) \\\\[7pt]
    & \ldots \\\\
    &  &  &   &  &   & + Prin_N & \qquad \left( = Par_N \right) \\\\[7pt]
\end{aligned}
$$

Leaving us with our final formula:

$$
\begin{aligned}
WAL \cdot Par_0 &=& \sum_{N=1}^{N}{Par_n} \\\\[7pt]
WAL &=& {  \sum_{n=0}^{N}{Par_n} \over  Par_0  } 
\end{aligned}
$$
