---
layout: post
section-type: post
title: Torque3D Player Animation I
category: tutorials
tags: [ 'tutorials' ]
---

<h3>DISCLAIMER</h3>
<i>Throughout this tutorial I will refer to Torque3D MIT as Torque.
<br><br>
Not every feature that Torque has to offer is going to be covered in this first tutorial, and the ones that are covered can be expanded upon and implemented in other ways.
<br><br>
This tutorial is for character animations as viewed in third person camera mode.
<br><br>
This is the first part in a series. Don't get the fire and pitchforks out until you've completed the entire series.
<br><br>
A base understanding of downloading the engine and installing it is a prerequisite to this. I am not responsible for anything you mess up with your project.
<br><br>
This tutorial is intended to be performed using the current release of Torque3D MIT(3.10.1) in its binary form. Version 4.0 is fast on the horizon, introducing a myriad of new features and additions, but we are going to keep it old school here and introduce those things as they are released. By binary I mean don't download and build Torque just grab the precompiled .exe(link below). I recommend a brand new download and we're just gonna take off right away with that. We're not worrying about customization, we are getting our Players running around and animated FAST.
<br><br>
In the future we may spruce things up a bit with a custom build in order to introduce new features but for now we are going to start from the ground up. We are not even going to have to use any sort of modeling software or animate our own models! By the end of this tutorial series we will have a vast library of animations at our disposal and the knowledge to use them in Torque3D MIT.
<br><br>
If you need LODs for your character model, I've put together a tutorial about that <a href="https://www.upstateindie.com/tutorials/2019/12/22/T3DPlayerLOD.html">here</a>. The LOD tutorial requires a basic knowledge of 
Blender and Torque3D's LOD system.
</i><br><br>
<h3 text-align="center">INTRO</h3>
Hi everyone! I wanted to take some time out of the busy Holidays to give a little back to the Community. In this tutorial, we will be covering animation of new Players in Torque3D. Torque really does make it easy to start having fun with your game ideas! Here I will be sharing a workflow that I currently use to get Player objects up and running quickly and easily. While this tutorial is intended for beginners, the information covered here could be useful to others as well.
The goal here is to introduce newcomers to the engine, so I want to keep this clear and concise. There is a lot of information to cover, but I'll do my best to present it all in an approachable order so it's easiest to follow. I'd like to reiterate that this is the first tutorial of a series that will become increasingly complex with each addition, so not all Player features will be availabe at the end of Part I. As we progress through each tutorial, additional functionality will be added to our Player. This is set up as a learn as you go process, so please join me as we explore the FUN side of Torque!
<br><br>
<b>What You're Gonna Need:</b>
<br>
Torque3D MIT 3.10.1 BINARY: <a href="https://github.com/GarageGames/Torque3D/archive/v3.10.1.zip">Torque3D Binaries</a>
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
<b>Root:</b> Type Idle in the search bar and pick an animation to download. Extract the animation file and rename it <b>PlayerAnim_Root.DAE</b>.
<br><br>
<b>Run:</b> Type Walking in the search bar and pick an animation to download. Remember that in mixamo terms this animation is called Walking and in Torque this animation is called Run. Extract the animation file and rename it <b>PlayerAnim_Run.DAE</b>.
<br><br>
<b>Back:</b> Type Walking Backward in the search bar and pick an animation to download. Remember that mixamo calls this animation Walking Backward and Torque calls this animation Back. Extract the animation file and rename it <b>PlayerAnim_Back.DAE</b>.
<br><br>
<b>Sprint_forward:</b> Type Running in the search bar and pick an animation to download. Remember that mixamo calls this animation Running and Torque calls this animation Sprint_forward. Extract the animation file and rename it <b>PlayerAnim_Sprint_forward.DAE</b>.
<br><br>
<b>Sprint_backward:</b> Type Running Backward in the search bar and pick an animation to download. Remember that mixamo calls this animation Running Backward and Torque calls this animation Sprint_backward. Extract the animation file and rename it <b>PlayerAnim_Sprint_backward.DAE</b>.
<br><br>
<b>Side:</b> Type Left Strafe in the search bar and pick an animation to download. Remember that mixamo calls this animation Left Strafe Walking(or similar) and Torque calls this animation Side. It is important to note that Side is strafing to the left. Extract the animation file and rename it <b>PlayerAnim_Side.DAE</b>.
<br><br>
<b>Side_Right:</b> Type Right Strafe in the search bar and pick an animation to download. Remember that mixamo calls this animation Right Strafe Walking(or similar) and Torque calls this animation Side_Right. Extract the animation file and rename it <b>PlayerAnim_Side_Right.DAE</b>.
<br><br>
To note here is that each animation has been renamed with the <b>PlayerAnim_</b> prefix. This isn't completely necessary, but it can be useful later on. Also the names of the actions are renamed from mixamo names to fit with Torque naming conventions.
<br><br>
<i>Here are a few things to think about for the future, when you are doing this on your own. Any of these animations can be played backwards. This means it is entirely possible to use a Walking animation from mixamo to be your Run and Back animation. Also a Strafe animation can be used for both Side and Side_Right if played backwards. And so on. We'll go over this more in Part II of the tutorial.
</i><br><br>
<h3>STEP 2-A: Importing T-Pose</h3>
Okay, so that's enough animations to get us through this first tutorial and get our Player moving about. Now we're into the good stuff, getting this all working in Torque! For this tutorial, do NOT import/export these models through Blender or another modeling app. Remember, we're not going to do that here - the import/export process there is more advanced and is reserved for a future tutorial, if at all in this series. Steve has some information <a href="https://forums.torque3d.org/viewtopic.php?t=1680" target="_blank">here</a> about the latest version of Blender for those who are curious.
<br><br>
Instead we are going to be using Torque's <b>TSShapeConstructor</b>. While I am about to make this all look easy because it should be clear that it IS easy...the TSShapeConstructor is your best friend and is extremely powerful. Much of its functionality is beyond the scope of this tutorial, but we will be using it more as we progress through the series. I strongly encourage newcomers to Torque to get familiar with this class later on so that they might realize the potential around using it. <a href="https://torque-3d.readthedocs.io/en/latest/script/class/TSShapeConstructor.html" target="_blank">TSShapeConstructor Reference</a>
<br><br>
<i>So what is this and how is it used?</i> Well, when you import a model into Torque the TSShapeConstructor sort of takes over in the background without you really noticing. What is important here is you realize that anytime you import a model into Torque, the TSShapeConstructor is called and it will spit out a new file in the folder where your imported .DAE file is located. This is crucial, let's step through this process so that we can get a closer look:
<br><br>
<b>1-</b> Open up the <b>art/shapes/actors/</b> folder. You'll see the original Soldier folder in there.
<br><br>
<b>2-</b> Create a new folder inside named <b>ybot</b>(or whatever you call your model). Note that whatever you name this folder, you are going to need to remember very soon.
<br><br>
<b>3-</b> Open up your newly created folder. You should now be at <b>art/shapes/actors/ybot/</b>(or whatever you call your model folder). Inside create one additional folder and name it Anims so you now have an <b>art/shapes/actors/ybot/Anims/</b> directory.
<br><br>
<b>4-</b> Move all of the renamed animation files into the <b>art/shapes/actors/ybot/Anims/</b> directory and the single ybot.zip file into the <b>art/shapes/actors/ybot/</b> directory. This structure is going to be important in a bit when we use the script to hook all this into Torque.
<br><br>
<b>5-</b> Open the file <b>art/datablocks/player.cs</b> and scroll down nearer the bottom to the Player datablock. The datablock holds information about our Player, and it starts off like this:<br>
<pre><code class="cs">
datablock PlayerData(DefaultPlayerData)
{
   renderFirstPerson = false;
   firstPersonShadows = true;
   computeCRC = false;

   // Third person shape
   shapeFile = "art/shapes/actors/Soldier/soldier_rigged.DAE";
   ...
</code></pre>
<br>
<b>6-</b> Update the shapefile entry so that it points to your new <b>ybot.DAE</b> file like so:
<br>
<pre><code class="cs">
datablock PlayerData(DefaultPlayerData)
{
   renderFirstPerson = false;
   firstPersonShadows = true;
   computeCRC = false;

   // Third person shape
   //shapeFile = "art/shapes/actors/Soldier/soldier_rigged.DAE";
   shapeFile = "art/shapes/actors/ybot/ybot.DAE";
   ...
</code></pre>
<br>
Notice that I just commented out the original soldier_rigged.DAE line and added a new one below it. By commenting, I mean that I placed <b>//</b> in front of that line so that Torque will ignore that line when executing this file. This way, if you ever want to reference the existing Soldier model in the future it's easy to just comment out your new line and uncomment the original one to go right back to the original Soldier.
<br><br>
<b>7-</b> Before we start up Torque we are going to make a small change to our spawn code so that we aren't in first person mode. Open the file <b>scripts/server/gameCore.cs</b> and search for the function:
<br>
<pre><code class="cs">
function GameCore::spawnPlayer(%game, %client, %spawnPoint, %noControl)
{
   ...
}
</code></pre>
<br>
Within this function find where it says:
<br>
<pre><code class="cs">
   // Give the client control of the player
   %client.player = %player;
</code></pre>
<br>
Right below that line add this line:<br>
<pre><code class="cs">
   %client.setFirstPerson(false);
</code></pre>
<br>
<b>8-</b> Since we are already in the gameCore.cs file, we should go ahead and remove the existing loadout for the Player so that we don't have our character equipping weapons they aren't ready to use yet. In the same <b>scripts/server/gameCore.cs</b> file find the function:<br>
<br>
<pre><code class="cs">
function GameCore::preparePlayer(%game, %client)
{
   ...
}
</code></pre>
<br>
Find the line <b>%game.loadOut(%client.player)</b> inside that function and comment it out. Remember, commenting is just adding <b>//</b> in front of that line:<br>
<br>
<pre><code class="cs">
   //%game.loadOut(%client.player);
</code></pre>
<br>
Now the Player won't try to equip weapons without the proper nodes in place.
<br><br>
<b>9-</b> Okay, double click your Torque3D.exe to start up the engine. Once on the title screen, click the Play button and choose the Empty Room level. You should notice your loading bar is processing the new .DAE file that you just pointed to in the Player datablock. When the level loads, you should have your new character model standing there with its arms out in a TPose. Sweet!
<br>
<p>
  <div align="center">
  <img src="/img/feet.png" height="384">
  </div>
</p><br>
The first problem you'll likely notice is we are looking at the new character's feet. lol. We'll fix  that right now in Step 2-B.
<br><br>
<h3>STEP 2-B: TSShapeConstructor in the Editor</h3>
Press F11 to open Torque's editor. <i>Depending on your keyboard and your computer's BIOS settings, certain keyboards with built in Function keys may require you to hold Fn and press F11. Chances are you won't encounter this but it's worth a mention.</i>
<br><br>
Click the icon in the top-middle-ish of your screen to launch the Shape Editor. It's the one that looks like a cube:
<br>
<p>
  <div align="center">
  <img src="/img/shapeEditor.PNG" height="374">
  </div>
</p><br>
Once the Shape Editor opens up look to the top right of the screen and click the <b>Library</b> tab. Right under that, click the dropdown box and navigate down to <b>art/shapes/actors/ybot</b> and click that.
<br><br>
<p>
  <div align="center">
  <img src="/img/shapeEdSelectNew.PNG" height="203">
  </div>
</p><br>
Once you do that, you'll see that you are now inside the new directory that we created earlier to keep our new T-Pose and animations:
<br>
<p>
  <div align="center">
  <img src="/img/shapeEdNew.PNG" height="203">
  </div>
</p><br>
Double click the ybot file there and the ShapeEditor will load up your model. With your mouse cursor hovering over the 3D viewport, you can hold <b>middle mouse button</b> and drag to reposition the view so you can see the model better:
<br>
<p>
  <div align="center">
  <img src="/img/shapeEdYbot.PNG" height="586">
  </div>
</p><br>
Alright, now let's turn our attention to the Properties section on the right side of the screen(right below the Shapes section where we just double clicked on our ybot file). Click the Node tab and then expand down the heirarchy of nodes down the Spine until it looks like the image below. Click the mixamorig_RightEye node and directly below in the Node Properties section rename the node to EYE and hit enter.
<br>
<p>
  <div align="center">
  <img src="/img/ShapeEdEYENew.PNG" height="411">
  </div>
</p><br>
Now click <b>mixamorig_HeadTop_End</b> and rename it to <b>CAM</b> and hit enter. After performing these changes click the file button(4 in image above) to save the changes. Go ahead and exit Torque now and restart it. Now when we load into the EmptyLevel mission our camera is hooked up to our new CAM node and we see things from a better perspective.
<br>
<p>
  <div align="center">
  <img src="/img/camNodeNew.png" height="498"> 
  </div>
</p><br>
Very cool! Now we are ready to plug in some animations!
<br><br>
<h3>STEP 3: TSShapeConstructor in Script</h3>
Now, let's get back to the TSShapeConstructor stuff. You might not realize it but just now when we changed the name of those nodes and clicked the save button, Torque automagically added a new ybot.cs file in the directory where your new model is located. Check in <b>art/shapes/actors/ybot/</b> and you should see a new <b>ybot.cs</b> file(or whatever your model is named). This is important for you to understand because right now we are going to use the power of that script. Rather than navigating through the Editor performing the same tasks over and over, we will just plug our animations into this script.
<br><br>
<b>1-</b> Go ahead and open the <b>art/shapes/actors/ybot.cs</b> file now. It should look like this:
<br><br>
<b>singleton TSShapeConstructor(YbotDAE)<br>
&#123;<br>
&emsp;baseShape = "./ybot.DAE";<br>
&#125;;<br>
<br>
function YbotDAE::onLoad(%this)<br>
&#123;<br>
&emsp;%this.renameNode("mixamorig_RightEye", "EYE");<br>
&emsp;%this.renameNode("mixamorig_HeadTop_End", "CAM");<br>
&#125;</b><br>
<br>
What's important to understand is that the <b>TSShapeConstructor</b> is being called from script in the first block to load the <b>./ybot.DAE</b> file. You'll notice that the baseShape has <b>./</b> in front of the ybot.DAE filename. All this means is that Torque is going to search in the same folder that the script is in to find this .DAE file.
<br><br>
<i>The second block of code will be executed any time this model is loaded up. So from here on out, even if you spawn in an AIPlayer using this same model, those nodes are going to be renamed by this script for you.</i>
<br><br>
<b>2-</b> Alright, let's get this animated already! <b>3, 2, 1, GO!</b> Here I'm going to provide the script for this to work, and then in Part II we'll cover how the TSShapeConstructor works using this script. Add the new lines from the script below so that it looks just like the example. You could even just copy this entire script and replace all of what's in yours:
<br><br>
<b>singleton TSShapeConstructor(YbotDAE)<br>
&#123;<br>
&emsp;baseShape = "./ybot.DAE";<br>
&#125;;<br>
<br>
function YbotDAE::onLoad(%this)<br>
&#123;<br>
&emsp;%this.renameNode("mixamorig_LeftEye", "EYE");<br>
&emsp;%this.renameNode("mixamorig_HeadTop_End", "CAM");<br>
&emsp;%this.addSequence("./anims/PlayerAnim_Root.dae", "Root", "0", "-1", "1", "0");<br>
&emsp;%this.addSequence("./anims/PlayerAnim_Run.dae", "Run", "0", "-1", "1", "0");<br>
&emsp;%this.addSequence("./anims/PlayerAnim_Back.dae", "Back", "0", "-1", "1", "0");<br>
&emsp;%this.addSequence("./anims/PlayerAnim_Sprint.dae", "Sprint_forward", "0", "-1", "1", "0");<br>
&emsp;%this.addSequence("./anims/PlayerAnim_Sprint_Back.dae", "Sprint_backward", "0", "-1", "1", "0");<br>
&emsp;%this.addSequence("./anims/PlayerAnim_Side.dae", "Side", "0", "-1", "1", "0");<br>
&emsp;%this.addSequence("./anims/PlayerAnim_Side_Right.dae", "Side_Right", "0", "-1", "1", "0");<br>
&#125;</b><br>
<br>
Save the script file and now when we launch Torque and start up a level we have an animated character playing its <b>Root</b> animation:
<p>
  <div align="center">
  <img src="/img/idle.png" height="384">
  </div>
</p><br>
You should be able to use the w,a,s,d keys to move around and also hold shift to sprint:
<br>
<p>
  <div align="center">
  <img src="/img/running.png" height="384">
  </div>
</p><br>
<br>
<h3>CONCLUSION</h3>
Let's recap. Now since this is a tutorial I went ahead and stepped you through all of the prerequisite knowledge required to get this initial movement state up and running. However, in the future you should now be able to take what you've learned here and do all of this super fast! Let's boil this tutorial down to its essence:
<br><br>
<b>1-</b> Download your T-Pose and animations.
<br><br>
<b>2-</b> Setup your datablock to point to your new character model.
<br><br>
<b>3-</b> Write a script that leverages the power of Torque's TSShapeConstructor to add your animations to the shape.
<br><br>

And there you have it! You are now free to integrate any mixamo characters or animations into your projects in 3 easy steps. All without ever having to touch a modeling app. In the following tutorial we will be breaking down those movement states a bit, looking closer at the <b>TSShapeConstructor</b> commands and how they work.
<br><br>
I hope you enjoyed this learning process as much as I did creating this tutorial. The main take away here is that you don't let Torque boggle your mind. Torque can be a bit daunting at first but once you learn the ropes it's as easy as <b>1, 2, 3</b>! 
<br><br>
Torque3D is Copyright &copy; 2012 GarageGames, LLC
