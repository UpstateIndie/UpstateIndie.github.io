---
layout: post
section-type: post
title: Torque3D Player Animation II
category: tutorials
tags: [ 'tutorials' ]
---

Welcome back to the FUN side of Torque! In <a href="https://www.upstateindie.com/tutorials/2019/12/17/T3DPlayerAnim_I.html">Part I</a> we added the TPose(root/idle) and basic character movement to our Player. At the end of this tutorial, we'll have a mixamo character with more fluid input controls and additional movement states. Here we'll go over some fundamentals about Torque's input triggers and movement states,  as well as how these things affect animations seen in Torque3D games.<br><br>
<i>
Throughout this tutorial I refer to Torque3D MIT as Torque.
<br><br>
This tutorial is for character animations as viewed in third person camera mode.
<br><br>
</i>
<b>What You're Gonna Need:</b>
<br>
Torque3D MIT 3.10.1 BINARY: <a href="https://github.com/GarageGames/Torque3D/archive/v3.10.1.zip">Torque3D Binaries</a>
<br><br>
A Mixamo account at <a href="https://www.mixamo.com/#/" target="_blank">mixamo</a>. No I don't work for Mixamo, they just offer something amazing for us to leverage here.
<br><br>
A text editor. I personally use <a href="https://notepad-plus-plus.org/" target="_blank">Notepad++</a> but there's a strong case for using Torsion.
<br><br>
<h2>PART II</h2>
<h3>Input</h3>
Input can be from most standard input devices attached to the computer. By default, Torque uses the script file <b>scripts/client/default.bind.cs</b> to create a <b>moveMap</b> object. In this file, the moveMap object uses the <b>moveMap.bind()</b> function to assign the functions that will be called on user input. Consider the following script:<br>
<pre><code class="cs">
moveMap.bind( keyboard, a, moveleft );
moveMap.bind( keyboard, d, moveright );
moveMap.bind( keyboard, left, moveleft );
moveMap.bind( keyboard, right, moveright );

moveMap.bind( keyboard, w, moveforward );
moveMap.bind( keyboard, s, movebackward );
moveMap.bind( keyboard, up, moveforward );
moveMap.bind( keyboard, down, movebackward );
...
</code></pre>
 As seen above, Torque uses the <b>'w,a,s,d'</b> control scheme by default as 'up, left, down, right'. For example, moveMap.bind() is called to assign <b>w</b> on the <b>keyboard</b> to the function <b>moveforeward</b>. Let's look closer at these movement functions so we can get a little more insight into what is actually happening whenever the 'w,a,s,d' keys are pressed:<br>
<pre><code class="cs">
function moveleft(%val)
{
   $mvLeftAction = %val * $movementSpeed;
}

function moveright(%val)
{
   $mvRightAction = %val * $movementSpeed;
}

function moveforward(%val)
{
   $mvForwardAction = %val * $movementSpeed;
}

function movebackward(%val)
{
   $mvBackwardAction = %val * $movementSpeed;
}
</code></pre>
<br>
Important here are the variables $movementSpeed, $mvLeftAction, $mvRightAction, $mvForwardAction, and $mvBackwardAction. These variables are used internally by Torque to control the Player object.<br><br>
The <b>%val</b> variable is input as 1 when a key is pressed and 0 when a key is released, so '%val * $movementSpeed' above is always either 0(not moving) or 1 * $movementSpeed(moving at the speed set by $movementSpeed). The $movementSpeed variable is set in the same file just above movement functions <br><br>
Sweet, so that's fairly simple. However, this default control scheme does have a couple flaws that cause the Player to sort of glide across the ground while holding multiple keys. One example of this is that if you hold down the 'w' and 'd' keys down at the same time, the Player will play a movement animation but glide across the ground in an unsightly way. Basically the diagonal movement looks wierd, so let's go ahead and fix that now. Copy this script into the <b>scripts/client/default.bind.cs</b> file, replacing the existing movement functions:<br>
<b>Third Person Movement</b><br>
<pre><code class="cs">
function moveleft(%val)
{
   $leftKeyPressed = %val;

   if(%val) // on key press
   {
      if($mvForwardAction || $mvBackwardAction)
         return;
      else
         $mvLeftAction = %val * $movementSpeed;  // enable strafe
   }
   else // on release of key
   {
      $mvLeftAction = 0;      // disable strafe

      // The forward key is down and there isn't already a forward action
      if($forwardKeyPressed && !$mvForwardAction)
      {
         %move = $forwardKeyPressed * $movementSpeed;
         moveforward(%move);
      }
      // The backward key is down and there isn't already a backward action
      else if($backwardKeyPressed && !$mvBackwardAction)
      {
         %move = $backwardKeyPressed * $movementSpeed;
         movebackward(%move);
      }
   }
}

function moveright(%val)
{
   $rightKeyPressed = %val;

   if(%val) // on key press
   {
      if($mvForwardAction || $mvBackwardAction)
         return;
      else
         $mvRightAction = %val * $movementSpeed;  // enable strafe
   }
   else // on release of key
   {
      $mvRightAction = 0;     // disable strafe

      // The forward key is down and there isn't already a forward action
      if($forwardKeyPressed && !$mvForwardAction)
      {
         %move = $forwardKeyPressed * $movementSpeed;
         moveforward(%move);
      }
      // The backward key is down and there isn't already a backward action
      else if($backwardKeyPressed && !$mvBackwardAction)
      {
         %move = $backwardKeyPressed * $movementSpeed;
         movebackward(%move);
      }
   }
}

function moveforward(%val)
{
   $forwardKeyPressed = %val;

   if(%val) // on key pressed
   {
      if($mvLeftAction || $mvRightAction)
         return;
      else
         $mvForwardAction = %val * $movementSpeed;  // enable forward movement
   }
   else // on release of key
   {
      $mvForwardAction = 0;      // disable forward movement

      // The left key is down and there isn't already a left action
      if($leftKeyPressed && !$mvLeftAction)
      {
         %move = $leftKeyPressed * $movementSpeed;
         moveleft(%move);
      }
      // The right key is down and there isn't already a right action
      else if($rightKeyPressed && !$mvRightAction)
      {
         %move = $rightKeyPressed * $movementSpeed;
         moveright(%move);
      }
   }
}

function movebackward(%val)
{
   $backwardKeyPressed = %val;

   if(%val) // on key pressed
   {
      if($mvLeftAction || $mvRightAction)
         return;
      else
         $mvBackwardAction = %val * $movementSpeed;  // enable backward movement
   }
   else // on release of key
   {
      $mvBackwardAction = 0;      // disable backward movement

      // The left key is down and there isn't already a left action
      if($leftKeyPressed && !$mvLeftAction)
      {
         %move = $leftKeyPressed * $movementSpeed;
         moveleft(%move);
      }
      // The right key is down and there isn't already a right action
      else if($rightKeyPressed && !$mvRightAction)
      {
         %move = $rightKeyPressed * $movementSpeed;
         moveright(%move);
      }
   }
}</code></pre>
<br>
So there we've updated the functions so that they will check if certain other movement variables are set. This amounts to a smoother feel overall, but can be expanded upon or implemented in other ways. The main changes here are: 'if we're moving forward or backward, don't strafe' and 'if we're strafing, don't move forward or backward'. Also there is an additional check when releasing keys that helps transition from one movement direction to another. A fairly simple script for basic 3rd person view movement.<br><br>
<h3>Movement</h3>
<i>Torque calls movement states <b>poses</b>, so we will refer to the different modes of movement as poses from here on out.</i><br><br>
In Part I we added some basic movement animations, which included a couple sprint animations. If we hold down the left shift key while moving forward or backward, the Player will begin playing a sprinting animation(i.e. PlayerAnim_sprint_forward). Behind the scenes, Torque is using the movemap's binding for the 'lshift' key to call the function <b>doSprint()</b>:
<pre><code class="cs">
function doSprint(%val)
{
   $mvTriggerCount5++;
}

moveMap.bind(keyboard, lshift, doSprint);
</code></pre>
<br>
What's occurring here is the <b>$mvTriggerCount5</b> variable is being incremented in order to toggle 'on' the Player's sprint pose. In the <b>scripts/client/default.bind.cs</b> file, we can find where Torque binds the movement triggers 0, 1, 2, 3, and 5. Here is a list of the default bindings:<br><br>
<b>$mvTriggerCount0 :</b> mouseFire() - <i>left click</i><br>
<b>$mvTriggerCount1 :</b> altTrigger() - <i>right click</i><br>
<b>$mvTriggerCount2 :</b> jump() - <i>space</i><br>
<b>$mvTriggerCount3 :</b> doCrouch() - <i>left control</i><br>
<b>$mvTriggerCount5 :</b> doSprint() - <i>left shift</i><br>
<br>
You probably noticed right away that <b>$mvTriggerCount4</b> is missing. This movement trigger is used for the <b>prone<b> pose(i.e. lying on the ground). The trigger 0 doesn't actually change the Player's pose like the others, but just as importantly tells the Player object to 'fire. The trigger 1 signals to the Player to either 'jet', 'altfire', or 'fire a weapon in mount slot 1'. Using the default Soldier in Torque jetting is disabled, no weapons are setup to use an altfire, and no weapons are mounted to mount slot 1. This being the case, it can be a little unintuitive at first to use right click($mvTriggerCount1) for Player input. We'll get more into this later on when we explore weapons and firing them. For now, the main takeaway is that we understand these movement triggers are being turned on or off in order to tell the Player class it needs to do something. <br><br>
<h3>Animations</h3>
Okay, let's get back to poses and how they affect Player animations. Here is a list of the poses available to the default Player class:<br>
<b>Standing : </b>While in this pose, the Player plays the <b>Root, Run, Back, Side,</b> and <b>Side_Right</b> animations.<br>
<b>Sprinting : </b>While in this pose, the Player plays the <b>Sprint_Root, Sprint_Forward, Sprint_Backward, Sprint_Side,</b> and <b>Sprint_Right</b> animations.<br>
<b>Crouching: </b>While in this pose, the Player plays the <b>Crouch_Root, Crouch_Forward, Crouch_Backward, Crouch_Side,</b> and <b>Crouch_Right</b> animations.<br>
<b>Prone: </b>While in this pose, the Player plays the <b>Prone_Root, Prone_Forward,</b> and <b>Prone_Backward</b>animations.<br>
<b>Swimming: </b>While in this pose, the Player plays the <b>Swim_Root, Swim_Forward, Swim_Backward, Swim_Left,</b> and <b>Swim_Right</b> animations.<br>
In Part I we implemented all of the available animations for the Standing pose, but only 2 animations for moving forward or backward in the Sprinting pose. The Sprint_Root animation is useful if you want a 'root' animation to blend with the other animations in the Sprinting pose. For example, if the Player needs to hold a weapon differently while sprinting. We won't be using this animation presently but it is good to know that it is available. If the Sprint_Side/Sprint_Right animations are missing Torque will play the standard Side or Side_Right animations instead, just a bit faster. This usually ends up visually looking 'okay', but since we have a library of mixamo animations at our disposal we could download a couple new Sprint_Side and Sprint_Right animations. While we are there, we may as well also grab a set of animations for our Crouching pose.<br><br>
<b>1-</b> Go ahead and login to your mixamo account and grab the animations for the actions listed below. Be sure to download them in the .DAE format.<br><br>
(TODO: Add text from Part I about renaming files)<------------------------
<b>Sprint_Side:</b> Type Left Strafe in the search bar and pick an animation to download. Remember that mixamo calls this animation Left Strafe and Torque calls this animation <b>Sprint_Side</b>. Extract the animation file and rename it <b>PlayerAnim_Sprint_Side.DAE</b>.<br>
<b>Sprint_Right:</b> Type Right Strafe in the search bar and pick an animation to download. Remember that mixamo calls this animation Right Strafe and Torque calls this animation <b>Sprint_Right</b>. Extract the animation file and rename it <b>PlayerAnim_Sprint_Right.DAE</b>.<br>
<i>Mixamo offers a variety of similarly named strafe actions, so it is important that you distinguish between a standard strafe(i.e. PlayerAnim_Side) and a sprinting strafe(i.e. PlayerAnim_Sprint_Side). I ended up using mixamo's 'Left Strafe Walking/Right Strafe Walking' animations for regular strafing and similarly named 'Left Strafe/Right Strafe' animations for strafing in the Sprinting pose. Alternatively, there is a 'Strafe' animation that fits well for sprinting. Practice makes perfect, and as you become more familiar with mixamo's and Torque's naming conventions this process of choosing animations grows easier.</i><br>
<b>Crouch_Root:</b> Type Crouch Idle in the search bar and pick an animation to download. Remember that mixamo calls this animation Crouch Idle and Torque calls this animation <b>Crouch_Root</b>. Extract the animation file and rename it <b>PlayerAnim_Crouch_Root.DAE</b>.<br>
<b>Crouch_Forward:</b> Type Crouch Walk Forward  in the search bar and pick an animation to download. Remember that mixamo calls this animation Crouch Walk Forward  and Torque calls this animation <b>Crouch_Forward</b>. Extract the animation file and rename it <b>PlayerAnim_Crouch_Forward.DAE</b>.<br>
<b>Crouch_Backward:</b> Type Crouch Walk Back in the search bar and pick an animation to download. Remember that mixamo calls this animation Crouch Walk Back and Torque calls this animation <b>Crouch_Backward</b>. Extract the animation file and rename it <b>PlayerAnim_Crouch_Backward.DAE</b>.<br>
<b>Crouch_Side:</b> Type Crouch Walk Left in the search bar and pick an animation to download. Remember that mixamo calls this animation Crouch Walk Left and Torque calls this animation <b>Crouch_Side</b>. Extract the animation file and rename it <b>PlayerAnim_Crouch_Side.DAE</b>.<br>
<b>Crouch_Right:</b> Type Crouch Walk Right in the search bar and pick an animation to download. Remember that mixamo calls this animation Crouch Walk Right and Torque calls this animation <b>Crouch_Right</b>.<br><br>
<b>2-</b> Extract all of the .zip animation files into the <b>art/shapes/actors/ybot/Anims/</b> directory(replacing 'ybot' with your model's name if necessary).<br><br>
<b>2-</b> As before, we are going to add these new animations to our model using the <b>TSStaticShapeConstructor</b> in script. Open your <b>art/shapes/actors/ybot.cs</b> file and update it with the new animations, like so:<br>
<pre><code class="cs">
singleton TSShapeConstructor(YbotDAE)
{
   baseShape = "./ybot.DAE";
};

function YbotDAE::onLoad(%this)
{
   %this.renameNode("mixamorig_LeftEye", "EYE");
   %this.renameNode("mixamorig_HeadTop_End", "CAM");
   %this.addSequence("./anims/PlayerAnim_Root.dae", "Root", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Run.dae", "Run", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Back.dae", "Back", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Side.dae", "Side", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Side_Right.dae", "Side_Right", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Sprint.dae", "Sprint_forward", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Sprint_Back.dae", "Sprint_backward", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Sprint_Side.dae", "Sprint_Side", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Sprint_Right.dae", "Sprint_Right", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Crouch_Root.dae", "Crouch_Root", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Crouch_Forward.dae", "Crouch_Forward", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Crouch_Backward.dae", "Crouch_Backward", "0", "-1", "1", "0");
   %this.addSequence("./anims/PlayerAnim_Crouch_Side.dae", "Crouch_Side", "0", "-1", "1", "0");
   // If NOT adding a Crouch_Right animation, we automatically use Crouch_Side in reverse =)
}
</code></pre><br>
<i>Note that in this script I opted to not add a Crouch_Right animation. As I've pointed out in the comment above, excluding this animation will automatically play the Crouch_Side animation in reverse. If the Player is in Crouch pose and moving left, then right, or vice versa...using the same animation in reverse can appear smoother. This is all subjective and ultimately each project's needs can greatly differ. Try out different animations, experiment, get creative. Have fun with tinkering here!</i>
<br>
Save the script file and now when we launch a level our Player will use all of the new animations we've added. Remember that holding 'left shift' will activate the Sprinting pose, so if we move left or right while holding this key our new Sprint_Side or Sprint_Right animations will be played. Additionally, since we've added some animations for the Crouching pose we can now switch to that pose by holding down the 'left control' key. As long as this key is held down, the Player object will be in its Crouching pose and all of its Crouch animations can be played while moving.<br><br>
<h3>Conclusion</h3>
This concludes Part II of our Torque3D Player Animation series. Let's recap. Here we've formed a better understanding of how user input is passed on to the Player class. We've also expanded our knowledge of Torque's pose system, which is used to control a wide range of animations for Player objects. Here are some main points to remember:<br><br>
<b>1-</b> By use of <b>movement triggers</b> and a <b>moveMap</b>, we can create functions in script that will accept input and affect the Player object's pose.<br>
<b>2-</b> An important thing to remember is to be sure that each movemap binding is calling a function which toggles the correct movement trigger(i.e. $mvTriggerCount0, etc.).<br>
<b>3-</b> To implement additional poses for our Player, we can use the <b>TSStaticShapeConstructor</b> in script to hook in new animations.<br>
<b>4-</b> An important thing to remember about poses is to be sure the animation names use the correct prefixes, both in script and when naming the animation file. To add an animation that allows the Player to move forward while prone, for example,  we'd name our animation file <b>PlayerAnim_Prone_Forward</b>.<br><br>
<i>Recommended reading:</i> (TODO: additional links to docs) <--------------------------
Thanks for joining me on this journey of Torque learning! You might have noticed that there is no pose for jumping. In Part III we will be discussing and implementing Player jumping, as well as covering additional Player functionality in greater detail. Keep Torque-ing! <br><br>
Torque3D is Copyright &copy; 2012 GarageGames, LLC
