---
title: What Can Student Results Tell Us About School Performance?
layout: post
category: thoughts
---

It seems likely that most people believe schooling somehow affects student ability. After all, we as a society invest significant amounts of time and money in various endeavours trying to measure exactly this. Yet these endeavours face an epistemic challenge. Because we can't measure the causal impact of schools directly, we can't know this causal impact with certainty. Instead, we infer the causal impact of schools on student ability by way of proxy measures such as student results. If student results improve, then we can infer that some aspect of schooling caused this. Perhaps.

How warranted is this inference from student results to school performance? With the aid of computer simulation, I investigate the robustness of this inference mechanism in a variety of common scenarios. Simulation allows us to stipulate causal mechanisms that cannot be observed in the real word and measure how well our empiric inferences map actual causes. I show that when selectivity, either by student or school, is present, the inference mechanism from student results to school performance is very poor. And if our causal inferences fail when causes are known, they must also fail when causes are not known.

Written in Literate Coffeescript, this paper is an argument for epistemic scepticism about school performance measurement. Simultaneously a philosophical argument and a computer simulation that demonstrates the argument.

I'll be presenting these ideas later in the year at PESA2015 in Melbourne. In the meantime, you can get a [read the draft][1] or [view the code and comment here][2].


<figure class="simulation">
<div id="shifting-averages"></div>
<figcaption>Is school performance anything more than shifting averages?</figcaption></figure>

Above we can see a simulation random ability students in two schools with identical impact on student performance but where some students can enrol in what they perceive to be the _best performing_ school.  High ability students (blue) quickly congregate in one school rather than the other and the inference from student results to school performance breaks down.  Click on the simulation to start or stop it.

[1]: http://dave.kinkead.com.au/school-performance/
[2]: https://github.com/davekinkead/school-performance

<script type="text/javascript" src="assets/d3.min.js"></script>
<script type="text/javascript" src="https://raw.githubusercontent.com/davekinkead/school-performance/gh-pages/assets/simulation.js"></script> 