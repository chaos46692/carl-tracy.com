---
title: "Formula for the Weighted Average Life of a Mortgage"
date: 2021-03-08T11:35:08-05:00
draft: false
tags : [finance]
categories : [nerd]
---
A while ago for work I had created a process that required a formula for the weighted average life (WAL) of a mortgage with level payments. I found one online:

$$ W = N D_N - \frac{1}{r} $$

where:

$$ 
\begin{aligned}  
d &= { 1 \over 1 + r} = \hbox{one month discount factor} \\\\[7pt] 
r &= {a \over 12} = \hbox{monthly discount} \\\\[10pt]
N &= \hbox{Loan period in months}  \\\\[10pt]
D_n &= {1 \over 1-d^n}
\end{aligned}  
$$ 

That formula is pretty convenient because it means given a rate and a WAL we can solve pretty easily for how long the loan has to be in order to have a particular WAL without doing a bunch of infinite sums. So where does that fomula come from? Originally I found it [here](https://welltemperedspreadsheet.wordpress.com/2011/07/14/fast-formulas-1-average-life-of-mortgage-as-scheduled/), but I wanted to see if I could derive it.

## Definitions
Let's start with defining a few things:

$$
\begin{aligned}
 W &= \hbox{Weighted average life in months} \\\\[3pt]
 P_n &= \hbox{Outstanding principal at time n} \\\\[3pt]
 I_n &= \hbox{Interest payment at time n} \\\\[3pt]
 N &=\hbox{Loand period in months} \\\\[3pt]
 a &=\hbox{Annual interst rate} \\\\
 r &=\hbox{Monthly interest rate} = {a \over 12} \\\\[3pt]
 L &= \hbox{Level mortgage payment}  
\end{aligned}
$$

## Deriving the formula
It can be shown[^1] that the WAL of any instrument can is given by

$$
W = {\sum{P_n} \over P_0}
$$

expand the formula in a very particular way:

$$
\begin{align}
W &= {\sum{r P_n} \over r P_0} \\\\[10pt]
 &= {\sum{I_n} \over r P_o} \\\\[10pt]
  & = {P_0 - P_0 + \sum{I_n} \over r P_0}
\end{align}
$$

now we can note that the initial principal $ P_0 $ is just the sum of all principal payments $ \sum{P_n} $ and that for a mortgage with level payments $ P_n + I_n = L $ so:

$$
\begin{align}
W & = { - P_0 + \sum{P_n + I_n} \over r P_0} \\\\[10pt]
 &= { - P_0 + \sum{L} \over r P_0} \\\\[10pt]
 &= {nL - P_0 \over r P_0 } \\;\\;\\;\\;\\;  \hbox{   (1)}
\end{align}
$$

## Solving for L
Our last equation works, but it includes the level payment $ L $, and we don't want to deal with that now do we? We know that the initial principal is just the discounted value of future payments, so we start from there and solve for L

$$
\begin{align}
P_0 & = Ld + Ld^2+Ld^3 + \dots + Ld^N \\\\[10pt]
 &= L \sum_{i=1}^{N}{d^i} \\\\[10pt]
 &= L {d - d^{N+1} \over 1 - d} \\\\[10pt]
 &= d L {1 - d^{N} \over 1 - d}  \\\\[10pt]
\hbox{Sove for L} \\\\[10pt]
L &= {P_0 \over d} \cdot {1-d \over 1-d^N} \\\\[10pt]
 &= P_0 \left( {1-d \over d} \right) {1 \over 1-d^N} \\\\[10pt]
 &= P_0 r {1 \over 1-d^N} \\\\[10pt]
 &= P_0 r D_N \label{PMT1} \\;\\;\\;\\;\\;  \hbox{   (2)}
\end{align}
$$

## Final formula for WAL
Inserting (2) into (1) gives u:
$$
\begin{align}
W & = { N \\left[P_o r D_N \\right] - P_0 \over r P_0 } \\\\[10pt]
 & = {N D_N \cdot r P_0 \over r P_0} - {P_0 \over r P_0} \\\\[10pt]
  &= N D_N - {1 \over r}
\end{align}
$$

[^1]: This is left as an exercise to the reader