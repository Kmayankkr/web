# Introduction

08/06/2018

## About me

My name is Araceli Vega Magro, I'm 24 years old and I'm currently studying a Master in Telecommunication Engineering. This is my second year at GSOC and I'm planning to continue the work started last year.

## Social Navigation

Currently, the Social Navigation algorithm employed in Robocomp uses an asymmetric gaussian function to define the personal space of humans. In last year's GSOC posts I explain in detail how the personal space of each human is created.

The main problem of the current algorithm is that the personal space of human is fixed, what makes it inconvenient for certain situations, for example, in a corridor, where the personal space may obstruct the robot path. 

I have created an algorithm, that I'll explain in my next post, in order to transform the fixed personal space used actually in Robocomp into an adaptive space, depending of the spatial context. 


### What I'm planning to do this GSOC

There is a lot of work to be done in order to achieve the goals set for this GSOC:

* In the last year, the navigation component has been modified. So, first of all, it will be necessary to integrate in this component the social navigation algorithm.

* Once it has been done, I would like to integrate the created algorithm in the Social Navigation Agent, for this aim, it will be necessary to extract the information of the room from the DSR, which is a model of the world.
 
* Besides, I would like to integrate new social behaviors during the navigation, such as approaching to a person located in the room or starting an interaction with humans if the path is blocked by them.

* * *
Araceli Vega Magro



