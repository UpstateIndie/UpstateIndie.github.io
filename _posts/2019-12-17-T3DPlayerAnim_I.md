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
<br>
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
<p>
  <div align="center">
  <img src="/img/tposeDownload.PNG" height="207">
  </div>
</p><br>
Here you want to choose <b>Collada(Dae)</b> and <b>T-Pose</b>. Just drop the .zip file in a handy place where you can find it to unzip it soon. A lot of times I'll just drop it right on the desktop.
<br><br>
While we're at the mixamo site we may as well grab up some animations. Click the <b>'Animations'</b> button at the top left. You'll notice in many cases there will be several different animations for a single type of movement(i.e. different kinds of Idle or Walking). We will be choosing one animation for each basic movement action. Here's what's important:
<br><br>
For each animation we download, a popup will appear when we click the Download button:<br>
<p>
  <div align="center">
  <img src="/img/animDownload.PNG" height="281">
  </div>
</p><br>
<b>Format:</b> Choose <b>Collada(.Dae)</b> here. It is important to note that Torque can also use the .fbx format in 4.0, but that is beyond the scope of this tutorial.
<br><br>
<b>Skin:</b> Choose <b>Without Skin</b> here. This just means that each animation we download is going to contain only the bones and no model. Our model is in the T-Pose file we downloaded earlier.
<br><br>
<b>Frames per Second</b> and <b>Keyframe Reduction</b> are optional but I just usually leave them at the default settings. You can get really creative here if it's necessary!
<br><br>
<i>The above options will be the same for every animation we download. If you leave the browser up while you are downloading these, every time you pick a new animation to download it will remember the settings you had(so you don't have to keep updating your selections).</i>
<br><br>
<h3>STEP 1-B: Movement Animations</h3>
We need an animation for each action the Player is going to perform. Right now, specifically, we are going to choose the animations for the actions I've listed below.
<br><br>
<b>Root:</b> Type Idle in the search bar and pick an animation to download.
<br><br>
<b>Run:</b> Type Walking in the search bar and pick an animation to download. Remember that in mixamo terms this animation is called Walking and in Torque this animation is called Run.
<br><br>
<b>Back:</b> Type Walking Backward in the search bar and pick an animation to download. Remember that mixamo calls this animation Walking Backward and Torque calls this animation Back.
<br><br>
<b>Sprint_forward:</b> Type Running in the search bar and pick an animation to download. Remember that mixamo calls this animation Running and Torque calls this animation Sprint_forward.
<br><br>
<b>Sprint_backward:</b> Type Running Backward in the search bar and pick an animation to download. Remember that mixamo calls this animation Running Backward and Torque calls this animation Sprint_backward.
<br><br>
<b>Side:</b> Type Left Strafe in the search bar and pick an animation to download. Remember that mixamo calls this animation Left Strafe Walking(or similar) and Torque calls this animation Side. It is important to note that Side is strafing to the left.
<br><br>
<b>Side_Right:</b> Type Right Strafe in the search bar and pick an animation to download. Remember that mixamo calls this animation Right Strafe Walking(or similar) and Torque calls this animation Side_Right.
<br><br>
<i>Here are a few things to think about for the future, when you are doing this on your own. Any of these animations can be played backwards. This means it is entirely possible to use a Walking animation from mixamo to be your Run and Back animation. Also a Strafe animation can be used for both Side and Side_Right if played backwards. And so on. We'll go over this more in Step 2-B below.
</i><br><br>
<h3>STEP 2-A: Importing T-Pose</h3>
Okay, so that's enough animations to get us through this first tutorial and get our Player moving about. Now we're into the good stuff, getting this all working in Torque! For this tutorial, do NOT import/export these models through Blender or another modeling app. Remember, we're not going to do that here - the import/export process there is more advanced and is reserved for a future tutorial, if at all in this series. Steve has some information <a href="https://forums.torque3d.org/viewtopic.php?t=1680" target="_blank">here</a> about the latest version of Blender for those who are curious. He gives some pointers there but personally I have an older version of Blender that works fine for me and hadn't attempted to import/export using this newer version of Blender.
<br><br>
Instead we are going to be using Torque's <b>TSShapeConstructor</b>. While I am about to make this all look easy because it should be clear that it IS easy...the TSShapeConstructor is your best friend and is extremely powerful. Much of its functionality is beyond the scope of this tutorial, but we will be using it more as we progress through the series. I strongly encourage newcomers to Torque to get familiar with this class later on so that they might realize the potential around using it. <a href="https://torque-3d.readthedocs.io/en/latest/script/class/TSShapeConstructor.html" target="_blank">TSShapeConstructor Reference</a>
<br><br>
<i>So what is this and how is it used?</i> Well, when you import a model into Torque the TSShapeConstructor sort of takes over in the background without you really noticing. What is important here is you realize that anytime you import a model into Torque, the TSShapeConstructor is called and it will spit out a new file in the folder where your imported .DAE file is located. This is crucial, let's step through this process so that we can get a closer look:
<br><br>
<b>1-</b> Open up the <filepath>art/shapes/actors/</filepath> folder. You'll see the original Soldier folder in there.
<br><br>
<b>2-</b> Create a new folder inside named <b>ybot</b>(or whatever you call your model). Note that whatever you name this folder, you are going to need to remember very soon.
<br><br>
<b>3-</b> Open up your newly created folder. You should now be at <filepath>art/shapes/actors/ybot/</filepath>(or whatever you call your model folder). Inside create one additional folder and name it Anims so you now have an <filepath>art/shapes/actors/ybot/Anims/</filepath> directory.
<br><br>
<b>4-</b> Extract all of the .zip animation files into the <filepath>art/shapes/actors/ybot/Anims/</filepath> directory and the single ybot.zip file into the <filepath>art/shapes/actors/ybot/</filepath> directory. This structure is going to be important in a bit when we use the script to hook all this into Torque.
<br><br>
<b>5-</b> Open the file <filepath>art/datablocks/player.cs</filepath> and scroll down nearer the bottom to the Player datablock. The datablock holds information about our Player, and it starts off like this:<br>
<br>
<b>datablock PlayerData(DefaultPlayerData)<br>
{<br>
renderFirstPerson = false;<br>
firstPersonShadows = true;<br>
computeCRC = false;<br>
// Third person shape<br>
shapeFile = "art/shapes/actors/Soldier/soldier_rigged.DAE";<br>
...</b><br>
<br>
<b>6-</b> Update the shapefile entry so that it points to your new <b>ybot.DAE</b> file like so:
<br><br>
<b>datablock PlayerData(DefaultPlayerData)<br>
{<br>
renderFirstPerson = false;<br>
firstPersonShadows = true;<br>
computeCRC = false;<br>
<br>
// Third person shape<br>
//shapeFile = "art/shapes/actors/Soldier/soldier_rigged.DAE";<br>
shapeFile = "art/shapes/actors/ybot/ybot.DAE";<br>
...</b><br>
<br>
Notice that I just commented out the original soldier_rigged.DAE line and added a new one below it. By commenting, I mean that I placed <b>//</b> in front of that line so that Torque will ignore that line when executing this file. This way, if you ever want to reference the existing Soldier model in the future it's easy to just comment out your new line and uncomment the original one to go right back to the original Soldier.
<br><br>
<b>7-</b> Before we start up Torque we are going to make a small change to our spawn code so that we aren't in first person mode. Open the file <filepath>scripts/server/gameCore.cs</filepath> and search for the function:
<br><br>



