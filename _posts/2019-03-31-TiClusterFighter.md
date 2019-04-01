---
layout: post
title: TI Controlled ClusterFighter!
---

So it is done! I got my TI99/4A controlling my 
[FPGA based ClusterFight entry](https://github.com/ClusterFights/MunchMan)
just in time for the 3/30/2019 [Fight Night](http://clusterfights.com/).

Here is snapshot of the setup.

![ClusterFightSetup]({{ site.baseurl }}/images/2019-03-31/clusterfight_setup1.jpg "ClusterFight Setup")

And here is a snapshot of the menu system, and sample run, that is 
displayed on the TI99/4a screen.

![iFindMenu]({{ site.baseurl }}/images/2019-03-31/ifind_match3.jpg "ifind menu")

In the end I decided to use the [TIPI board](http://ti994a.cwfk.net/TIPI.html) 
to telnet into my Munchman Cluster. The MunchMan Cluster is composed of a
[Arty S7: Spartan-7 FPGA board](https://store.digilentinc.com/arty-s7-spartan-7-fpga-board-for-hobbyists-and-makers/) 
and a Raspberry Pi connected together via a 16-bit databus using the Raspberry Pi GPIO.

The main trick to get it working was to connect the TIPI 
Raspberry Pi to the MunchMan Raspberry Pi via a ethernet cable.  Then I assigned
a static IP addresses to both Raspberry Pies.  Then I could use the
TIPI telnet application to log directly into the MunchMan Raspberry Pi.

Below is a video taken at Fight Night where I explain how
it is working.

<iframe width="560" height="315" src="https://www.youtube.com/embed/2ANZJTDc5s4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

And here is a video showing round 1 action.
The goal was to find a 19 character string that has the MD5 hash
of 0xedf09aa5db5f3986a9fdca080e9d9871 out of a 430MB dataset.

<iframe width="560" height="315" src="https://www.youtube.com/embed/bvCHjYiBQgI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

It was a very short round in which MunchMan won in 1.59 seconds.  
Second place was not far behind at 6 seconds. For this
round MunchMan computed 93,960,001 MD5 hashes in 1.59 seconds.
This gives a rate of about 59 million hashes per second! 
The FPGA design supports up to 100 million hashes per second,
but the bottleneck is how fast the Raspberry Pi can send
data over the GPIO 16-bit databus.

## Path not taken

I actually went pretty far down the path of trying to create an rs232 sideport board.
I drew up schematics that had a TMS9902 UART, Flash Memory, and 74LS Series chips
for address decoding.  I ordered some TMS9902 parts off of ebay, plus obtained
the other parts I needed.  My plan was to wire wrap a board, or possibly use
a solderless breadboard for the prototype.

After I received the TIPI board, I saw that it offered a much quicker
and cleaner path to my end goal of getting the TI99/4a to communicate
with my compute cluster.

## Thanks for reading

So this concludes my RetroChallenge 2019/03 entry.
It was a great excuse to play with my old
TI99/4A again.  I'll probably continue this blog
as my son and I started writing a game in TuroboForth.
I'll let you know how it goes.  Thanks for reading!

