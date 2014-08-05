#REUSABLE LAUNCH SYSTEM README#
A model of SpaceX's Grasshopper Program in the Kerbal Space Program, using the kOS mod.

##CONTENTS##

####1. INTRODUCTION####

####2. SOFTWARE####
  *	Kerbal Space Program (KSP)
  *	KSP Mods
    -	kOS
    -	KW Rocketry
    -	Vehicle

####3. GRASSHOPPER PROGRAM SUMMARY####

####4. MODEL PROGRAM####

####5. OTHER PROJECTS OF NOTE####
  *	BahamutoD
  *	SnakeSign
  *	LazarusLuan

<hr/>

####I. INTRODUCTION####

I started playing KSP about two months ago. After an entire night on the demo version and the following weekend on the full version, I started to look into the available mods. I was looking for an autopilot mod from the beginning because I wanted a less time-consuming way to transfer orbits than to sit at the computer while my nuclear powered engines slowly burned in the same direction for fifteen minutes. I encountered MechJeb, but the automation was too much of a black box. I didn't want an autopilot that did the thinking for me. I wanted to tell the autopilot how to think and then have it repeat what I told it as many times as I wanted. Then I ran into kOS, where the user can write scripts for the autopilot to run. The syntax is meant to be accessible to non-programmers as well, but it is good enough to get the job done.

About thirty minutes after playing around with kOS, I thought it would be really cool to make a recoverable rocket a la SpaceX's plans. I looked around online to make sure that I wasn't doing anything that had been done before. It turns out there are several other people who have done very impressive similar things and I make note of some of the similarities and differences of those projects at the end of this.

In the end, I decided to replicate SpaceX's Grasshopper program. Kerbal Space Program taught me more about orbital mechanics in half an hour than I had ever learned in any class. (And while I'm not a physics, aero, or astro major, I didn't shy away from learning about that kind of stuff in school.) I was hoping that by modeling the Grasshopper program, I would learn similarly about AI, control theory, and other applied programming concepts.

I certainly did learn a lot and I've marked the progress of my discoveries at each flight in my project.


####II. SOFTWARE####

* [Kerbal Space Program (KSP)](https://kerbalspaceprogram.com/)

	Kerbal Space Program is a game in which the player runs a space agency for the fictional Kerbal species of the planet Kerbin.

* Mods
	
	I used a couple of mods to model the Grasshopper program, most notably kOS. Installation instructions are found in the links.

	-	[kOS](http://www.curse.com/ksp-mods/kerbal/220265-kos-scriptable-autopilot-system)

      kOS is a scriptable autopilot system. This is the mod that enables autonomous operation of the rocket. A program is loaded into the kOS terminal on a custom ship part and the rocket is entirely controlled via that program while it executes.

	-	[KW Rocketry](http://www.curse.com/ksp-mods/kerbal/220894-kw-rocketry)

      This mod adds some parts to KSP. I originally got this because I was looking for fairings. I ended up not using the fairings, but using the fuel tanks and engines that KW Rocketry includes.

	-	Vehicle

      It's not a mod, but I've included the Grasshopper rocket I built in KSP.


####III. GRASSHOPPER PROGRAM SUMMARY####

  Flight 1 - September 21, 2012  
  Duration: 3 seconds  
  Altitude: ~2.5 meters  

  Flight 2 - November 1, 2012  
  Duration: 8 seconds  
  Altitude: ~5.4 meters  

  Flight 3 - December 17, 2012  
  Duration: 29 seconds  
  Altitude: 40 meters  

  Flight 4 - March 7, 2013  
  Duration: 34 seconds  
  Altitude: 80 meters  

  Flight 5 - April 19, 2013  
  Duration: 58 seconds  
  Altitude: 250 meters  

  Flight 6 - June 14, 2013  
  Duration: 67 seconds  
  Altitude: 325 meters  

  Flight 7 - August 13, 2013  
  Duration: 60 seconds  
  Altitude: 250 meters  
  Lateral Diversion: 100 meters  

  Flight 8 - October 7, 2013  
  Duration: 78 seconds  
  Altitude: 744 meters  


####IV. MODEL PROGRAM####

  Flight 1  
  Duration: 3.02 seconds  
  Altitude: 2.82 meters  

  Notes: The code for my first flight was pretty crude. The basic idea was to increase the thrust until there was a detectable liftoff, decrease the thrust for the rest of the flight, and cut the throttle when the target mission time had been reached. I knew it was an inconsistent approach and I intended to fix it after I had written the more advanced flights. But because my programs became incrementally more sophisticated, I decided to not rewrite my early flights and instead keep their original form to showcase the learning process I went through.

  Flight 2  
  Duration: 8.02 seconds  
  Altitude: 5.75 meters  

  Notes: The second flight was so inconsistent that I spent more time tweaking this program than any other except for the seventh flight (the lateral diversion one). Similar in structure to the first flight, but with dramatically higher error, this program sometimes shoots 15 meters into the air and other times doesn't lift off the pad. Part of the inconsistency stems from KSP's inconsistency in launch conditions - but the real world isn't consistent either. This program proves the need for a different approach.

  Flight 3  
  Duration: 29.34 seconds  
  Altitude: 40.03 meters  

  Notes: It was during this program that I discovered what PID controllers were. This flight was a breeze after I implemented one (along with integral windup prevention). Thrust is obviously the output of this PID, and I decided to use altitude as my input. Although I didn't know what it was called at the time, I implemented a linear setpoint ramping on the descent. This led to small, repeated bursts of thrust as the rocket comes down. I didn't want my flights to sound like V-1s, so I looked for a way to smooth out the thrust in the next flight.

  Flight 4  
  Duration: 33.94 seconds  
  Altitude: 80.92 meters  

  Notes: I solved the thrust burst problem in this flight by using a PID controller with velocity as the input instead. I also put much of the PID code into a separate file and called it from the driver (kOS's closest thing to writing functions/methods). Now I could have a steady ascent and descent, and just change the desired velocity.

  Flight 5  
  Duration: 57.88 seconds  
  Altitude: 250.47 meters  

  Notes: This flight was one of the most exciting to program. After reading up about them on Wikipedia, I implemented a cascading PID control system. The first one takes in desired altitude, compares it to the current altitude, and outputs a suggested velocity for the ship to get to the desired altitude. The second controller takes in this desired velocity, compares it to the current velocity, and outputs throttle to get to the desired velocity. The further from the desired altitude the ship is, the greater magnitude of desired velocity is output (and the thrust adjusts to this ideal velocity). When the ship's altitude is close to the desired altitude, the desired velocity is very small in magnitude and the ship slowly approaches it's target.

  Flight 6  
  Duration: 66.82 seconds  
  Altitude: 325.01 meters  

  Notes: Flight 6 added a few tweaks to the control system. Set point ramping led to steadier ascents and descents. The derivative term is also based on the process variable, rather than the error. This prevents bump during a set point change.

  Flight 7  
  Duration: 61.50 seconds  
  Altitude: 250.07 meters  
  Lateral Diversion: 99.37 meters  
  Landing Distance from Launch Position: 0.398 meters  

  Notes: This was the most challenging flight to program, due entirely to the lateral diversion. After much trial (and error) with different ways of steering, I settled on a cascaded PID system for desired latitudinal and longitudinal each. Like the altitude PID system, these systems took in position, calculated an ideal latitude velocity or longitude velocity, and output pitch or yaw.This worked the best partly because each system only concerns itself with one dimension. Another method that I initially tried was rolling towards the waypoint and tilting the nose of the rocket towards it. This failed for a number of reasons, some due to KSP/kOS, some due to the complexity, and I'm sure some due to the programmer. This program has a total of 6 PID controllers: 2 each for altitude, latitude, and longitude.

  Flight 8  
  Duration: 78.04 seconds  
  Altitude: 744.21 meters  

  Notes: With both altitude and steering mastered, this last flight was one of the easiest to program. It is also hands down the most consistent flight. The duration typically varies by less than a quarter of a second and the altitude by less than a tenth of a meter. The rocket also has a more precise landing than any other program.

####V. OTHER PROJECTS OF NOTE####

*	BahamutoD, Fully Recoverable Rocket - [YouTube](http://www.youtube.com/watch?v=QKEq5pu0txA)

  This guy's video was the first one I found when searching for recoverable rockets using kOS. To be honest, I was a little depressed after watching because he seemed to have done exactly what I intended to do six months before I had the idea. In his program, though, the script is a sequence of commands tailored exactly to the specific rocket under specific conditions and was developed through straight trial and error. I wanted to make programs that would be more flexible and would calculate their way to different speeds, altitudes, and locations. I also decided to make replicating the Grasshopper program the focus of my project, rather than appearing to copy someone else's project, even if the code is significantly different.

*	SnakeSign, PID Hovering with kOS - [Imagur](http://imgur.com/a/W1chH)

  This guy developed a kOS program that uses a PID controller to make a rocket hover, ascend, or descend slowly. I stumbled across his project while looking for a way to make a rocket hover at a specific altitude. Again, I was a little dispirited that I had been beaten to the punch at making a kOS hovering algorithm, but even the small insights that his project gave me (he posted no code) proved invaluable to mine. For one, I had never heard of a PID controller. After some Googling, Wikipedia-ing, and forum lurking, I was elated that I had found exactly what I had been missing - or at least a good solution. The more I learned, the more incredulous I became that I had never heard of them before. I was kind of upset at myself and started asking some of my classmates if they were familiar with them at all. Only a couple were, but the real kicker was when my mother casually contacted me: "Didn't you know? Dad and I used those all the time at work." Thaaaaanks mom. Glad I know not only that I'm the stupid one in the family, but also that my lifelong goal of not becoming my parents has taken another hit. Anyway, this guy's program uses kOS to control throttle, but pilot input to control steering, whereas mine takes no pilot input. More significantly, I implemented cascading PID controllers (thank you Wikipedia) after a few flights. Still, my program is largely a collection of PID-based control systems and this guy's project is where I saw a PID first.

*	LazarusLuan, LazTek SpaceX Collection - [Parts Pack](http://www.curse.com/ksp-mods/kerbal/221420-laztek-spacex-launch-pack-3-0)

  I didn't draw much from this guy's project since I had already completed all of my Grasshopper flights before coming across it. Mostly, this guy's project help me decide when to stop. I knew from the beginning that I wanted to do something that was different from everything else that had been done. After I finished the Grasshopper flights I intended to replicate the a few Falcon 9 Reusable Dev 1 vehicle flights (F9R Dev1), as well as the first few ocean soft landings of recent Falcon 9 v1.1s. I specifically wanted to have a Falcon 9 with nine engines, three of which are used on the F9R Dev1's ascent, and one of which is used on descent. After some wrestling with the parts for a while in the Vehicle Assembly Building, I did manange to get a rocket that had nine engines on it, although the nozzles were all overlapping. However, girders were required to make the landing legs long enough, the landing legs weren't strong enough, the rocket could not lift off on three engines (and certainly not one), and it rolled uncontrollably when it launched on all nine engines. Also, there weren't any stock parts that could act like the steering fins present on some of the F9R Dev1's flights (the small canards were too large and didn't have the right properties). It became clear that if I wanted to replicate the F9R Dev1 flights, I would need to either modify existing parts or make some new ones. Modifying existing parts would make it impossible for anyone to copy my project in their KSP environment and my project was intended to be a software exercise, not a new mod or parts pack. My project began to suffer a bit of an identity crisis and I wasn't sure what the next target was. That's when I stumbled across this guy's video and parts pack. A third time, I was impressed and depressed. This guy's custom parts impressively replicate SpaceX rockets (even down to the one engine descent) and automatically return to the pad after use (not sure if that's part of his parts pack/mod, or just his video). By this point, my project had served its primary purpose - to give me a crash course in control theory like KSP had done for orbital mechanics - and I decided to move on to a new project.