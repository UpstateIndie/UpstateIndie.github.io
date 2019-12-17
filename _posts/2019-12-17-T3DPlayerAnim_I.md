---
layout: post
section-type: post
title: Torque3D Player Animation I
category: tutorials
tags: [ 'tutorials' ]
---

<h3>DISCLAIMER</h3>
<i>I am not responsible for anything you mess up with your project as a result of following this.
<br><br>
Throughout this tutorial I will refer to Torque3D MIT as Torque.
<br><br>
Not every feature that Torque has to offer is going to be covered in this first tutorial, and the ones that are covered can be expanded upon and implemented in other ways.
<br><br>
This tutorial is for character animations as viewed in third person camera mode.
<br><br>
This is the first part in a series. Don't get the fire and pitchforks out until you've completed the entire series.
<br><br>
A base understanding of downloading the engine and installing it is a prerequisite to this.
<br><br>
This tutorial is intended to be performed using the current release of Torque3D MIT(3.10.1) in its binary form. Version 4.0 is fast on the horizon, introducing a myriad of new features and additions, but we are going to keep it old school here and introduce those things as they are released. By binary I mean don't download and build Torque just grab the precompiled .exe from [here]. I recommend a brand new download and we're just gonna take off right away with that. We're not worrying about customization, we are getting our Players running around and animated FAST.
<br><br>
In the future we may spruce things up a bit with a custom build in order to introduce new features but for now we are going to start from the ground up. We are not even going to have to use any sort of modeling software or animate our own models! By the end of this tutorial series we will have a vast library of animations at our disposal and the knowledge to use them in Torque3D MIT.
</i><br><br>
<h3 text-align="center">INTRO</h3>
Hi everyone! I wanted to take some time out of the busy Holidays to give a little back to the Community. In this tutorial, we will be covering animation of new Players in Torque3D. Torque really does make it easy to start having fun with your game ideas! Here I will be sharing a workflow that I currently use to get Player objects up and running quickly and easily. While this tutorial is intended for beginners, the information covered here could be useful to others as well.
The goal here is to introduce newcomers to the engine, so I want to keep this clear and concise. There is a lot of information to cover, but I'll do my best to present it all in an approachable order so it's easiest to follow. I'd like to reiterate that this is the first tutorial of a series that will become increasingly complex with each addition, so not all Player features will be availabe at the end of Part I. As we progress through each tutorial, additional functionality will be added to our Player. This is set up as a learn as you go process, so please join me as we explore the FUN side of Torque!
<br><br>
<b>What You're Gonna Need:</b>
Torque3D MIT 3.10.1 BINARY: <a href="http://wiki.torque3d.org/main:downloads" target="_blank">Torque3D Binaries</a>
<br><br>
A Mixamo account at <a href="https://www.mixamo.com/#/" target="_blank">mixamo</a>. No I don't work for Mixamo, they just offer something amazing for us to leverage here.
<br><br>
A text editor. I personally use <a href="https://notepad-plus-plus.org/" target="_blank">Notepad++</a> but there's a strong case for using Torsion.
<br><br>
That's it! In the future we may use <a href="https://www.blender.org/" target="_blank">Blender</a> briefly for simple collision shapes but aside from that this is showing off the power of Torque, not Blender. I could forego using Blender at all because Torque can do anything we need it to anyway!
<br><br>
<h2>PART I</h2>
Let's address the elephant in the room. Mixamo. I could go on and on about the benefits of using this FREE service, but they have provided enough information on their site. Check out the <a href="https://helpx.adobe.com/creative-cloud/faq/mixamo-faq.html" target="_blank">FAQ</a> there and let it soak in that all of that truly is free for you to use. They have a huge and growing database of sleek characters and animations. This all probably sounds like a plug at this point so I'll stop, but by the time you are done with this tutorial I think you'll appreciate what they are putting on the table for us. Go ahead, get yourself an account there. I am going to have to insist that you have a bit of fun with that while looking at all the cool animations you are about to be using in Torque.
<br><br>
Done? Sweet. Moving along then, let's get this stuff into Torque!
<br><br>
In Part I we are adding the TPose(root/idle) and basic character movement to our Player. At the end of this tutorial, we'll have a mixamo character animated in Torque that is able to be expanded upon by any of the animations on the mixamo site. Don't worry! Footsteps, jumping, additional nodes, collision, and weapons  will all be covered as we progress through the series.
<br><br>
<h3>STEP 1-A: Intro to Mixamo</h3>
Pick a character for your project. I grabbed up Ybot to use while making this tutorial, but the value here is that you don't have to. Pick any character you want and let's go!
<br><br>
When you click the download link you'll be presented with a popup with 2 options:<br>
<img src="https://github.com/UpstateIndie/UpstateIndie.github.io/raw/master/.github/tposeDownload.PNG" height="207">


