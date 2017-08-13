---
layout: page
title:  Philosophy
permalink: /philosophy/
---

I'm a computational philosopher at the [University of Queensland](http://hapi.uq.edu.au) with a focus on moral and political philosophy.  Most of my work involves the use of Agent Based Models to test the claims of other theorists and to examine philosophical questions from a unique perspective. 


### [Modelling the Boundary Problem][1]

This is an argument about the relationship between democratic inclusion and instrumental justifications of democracy. I show that any account of democracy that relies on the outcomes of democracy processes to demonstrate democracy's value must have a congruent account of inclusion. Not only that, but because different accounts of democracy rely on different and incompatible accounts of inclusion, these accounts of democracy are themselves incompatible.

This paper is unique in that is simultaneously a philosophical argument and a computer simulation. Written in literate coffeescript, the code described in the paper can also be run by the coffeescript compiler to demonstrate the argument being described by the paper. The reader may generate both the argument in PDF, or the simulation and results in HTML with the command coffee paper.coffee.md.

[1]: http://dave.kinkead.com.au/modelling-the-boundary-problem/


### [What Can Student Results Tell Us About School Performance?][2]

It seems likely that most people believe schooling somehow affects student ability. After all, we as a society invest significant amounts of time and money in various endeavours trying to measure exactly this. Yet these endeavours face an epistemic challenge. Because we can't measure the causal impact of schools directly, we can't know this causal impact with certainty. Instead, we infer the causal impact of schools on student ability by way of proxy measures such as student results. If student results improve, then we can infer that some aspect of schooling caused this. Perhaps.

How warranted is this inference from student results to school performance? With the aid of computer simulation, I investigate the robustness of this inference mechanism in a variety of common scenarios. Simulation allows us to stipulate causal mechanisms that cannot be observed in the real word and measure how well our empiric inferences map actual causes. I show that when selectivity, either by student or school, is present, the inference mechanism from student results to school performance is very poor. And if our causal inferences fail when causes are known, they must also fail when causes are not known.

[2]: http://dave.kinkead.com.au/school-performance/


### [Models of Segregation][3]

A reproduction of Thomas Schelling's seminal 1969 work using agent based modelling with coffeescript, D3.js, and HTML.  In it, I demonstrate Schelling's insight about complex systems in which the interplay of individual choices bear no close relation to individual intent.

[3]: http://dave.kinkead.com.au/models-of-segregation/

### [Spatialised Prisoners Dilemma][5]


An agent based simulation of an spatialised evolutionary prisoner's dilemma written in coffeescript. A prisoner's dilemma is a game where players can either cooperate or defect with each other.  Each action has a payoff based on the other player's action and there are 8 possible deterministic strategies given an initial move, a response to cooperation, and a response to defection.

[5]: http://dave.kinkead.com.au/spatialised-prisoners-dilemma/


### [Computational Philosophy Workshop][4]


A semester long workshop on computational approaches to philosophy covering agent based modelling, game theory, and information theory.  We start of with some basic simulation, move on the dynamic mutli-strategy games, and finish up with information cascades.

[4]: https://github.com/davekinkead/computational-philosophy-workshop
