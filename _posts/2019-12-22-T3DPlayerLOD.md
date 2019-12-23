---
layout: post
section-type: post
title: How to LOD Mixamo Characters
category: tutorials
tags: [ 'tutorials' ]
---

<h3>DISCLAIMER</h3>
<i>Throughout this tutorial I will refer to Torque3D MIT as Torque.
<br><br>
If you followed my Torque3D Player Animation I tutorial prior to this, nice, you understand how to get your characters into Torque. However, here we will be starting from scratch with a new character. This workflow will cause any existing animations you downloaded before to be incompatible with the new model that includes LODs.
<br><br>
A basic working knowledge of Blender usage and Torque3D's LOD system are both prerequisites to this, but I'll go over important specifics here. I will not be going over why you need to LOD or how the LODs work in Torque, as all of that is beyond the scope of this tutorial.
<br><br>
I am not responsible for anything you mess up with your project as a result of following this.
</i><br><br>

<h3 text-align="center">INTRO</h3>
Hello out there Torque people! Before I get too far into my beginner's series, I wanted to take a step back to talk about LOD(level of detail) for Mixamo characters. More specifically, how to do this using Blender. 
<b>What You're Gonna Need:</b>
<br>
Torque3D MIT 3.10.1 BINARY: <a href="https://github.com/GarageGames/Torque3D/archive/v3.10.1.zip">Torque3D Binaries</a>
<br><br>
A Mixamo account at <a href="https://www.mixamo.com/#/" target="_blank">mixamo</a>. No I don't work for Mixamo, they just offer something amazing for us to leverage here.
<br><br>
<a href="https://www.blender.org/" target="_blank">Blender 2.81a</a> - At the time of this writing this is the latest version, although anything newer should be similar.
<br><br>

<h2>Step 1: Mixamo Download</h2>
Alright, so one thing to go over about Mixamo that's important to remember if you follow this workflow. We want to download our characters in the <b>.fbx</b> format, NOT the usual <b>.DAE</b>. We can still ultimately end up with <b>.DAE</b> models to use in Torque, but for this LOD workflow it is important that we download the characters from Mixamo in the <b>.fbx</b> format initially. Go ahead and download a character from <a href="https://www.mixamo.com/#/" target="_blank">mixamo</a>:<br>
<p>
  <div align="center">
  <img src="/img/tposeDownloadFBX.PNG" height="210">
  </div>
</p><br>

<h2>Step 2: Blender Specifics</h2>
Extract the .fbx someplace and import it into Blender. Here, we are basically going to perform a few steps to prep this model for re-rigging. Don't get worried, we won't actually have to rig it ourselves and this process is fairly quick.
<br><br>

<b>1-</b> In the top right of the Blender window, expand the collection that holds your model. The heirarchy should look like this currently:<br>
<p>
  <div align="center">
  <img src="/img/blendLODorig.PNG" height="164">
  </div>
</p>
<br><br>
Now click the Armature, then hover your mouse over the 3D viewport and press Delete. The heirarchy should now look like this:<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep2-1.PNG" height="90">
  <br><i>This is using Mixamo's ybot model.</i>
  </div>
</p><br>
You'll notice that the 3D viewport is filled with a hugely scaled version of the original model. Zoom out with your mouse so you can see what's going on.
<p>
  <div align="center">
  <img src="/img/ybotOnGround.PNG" height="306">
  </div>
</p><br>

<b>2-</b> Let's go ahead and fix the model's rotation now. Click on a mesh(1), be sure you are in the Object Properties tab(2), and click the <b>Rotation X</b> field(3) under the <b>Transform</b> section. Change that to <b>90</b> and hit enter:<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep2-2.PNG" height="397">
  </div>
</p><br>
Repeat this step for any meshes that might be in your model. In my case, I'm using the ybot model so I had to rotate both the <b>Alpha_Joints</b> mesh and the <b>Alpha_Surface</b> meshes. Now the model is standing upright:<br>
<p>
  <div align="center">
  <img src="/img/ybotStanding.PNG" height="778">
  </div>
</p><br>
<br>

<b>3-</b> The model is still super huge by Torque standards so we'll scale it down. Click on a mesh, be sure you are in the Object Properties tab, and click the <b>Scale X</b> field under the <b>Transform</b> section. Type <b>0.01</b> for each axis(X, Y, Z). Your rotation and scale properties should now look like this:<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep2-3.PNG" height="251">
  </div>
</p><br>
Repeat this step for any meshes in your model.<br>
<br>
Since the model is so tiny now, hover your mouse over the 3D viewport and press <b>.</b> on the numpad to automatically zoom in(a mesh has to be selected).
<br><br>

<b>4-</b> Since we've adjusted the rotation and scale for each mesh we should apply those values to each object's data. With a mesh selected, click <b>Object</b> - 
<b>Apply</b> - <b>Rotation & Scale</b>(in the top left), like so:<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep2-4.PNG" height="259">
  </div>
</p><br>
Now when you look back at the Object Properties tab in the Transform section, you'll notice the rotation and scale values we adjusted have been zeroed out. Perfect. Repeat this step for all the meshes in your model.
<br><br>

<b>5-</b> Now let's clean up some stuff. Since we removed the original Armature, we won't need the old Armature modifier or the existing vertex groups. If you look at the top right and expand the heirarchy for each mesh, you'll see a visual indicator for the Modifiers and Vertex Groups:<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep2-5.PNG" height="204">
  </div>
</p><br>
<b>(A)</b>Click on a mesh, be sure you are in the Modifier Properties tab(1), and click the <b>X</b>(2) in the top right of the modifier block to delete it.
<p>
  <div align="center">
  <img src="/img/blendLODstep2-5A.PNG" height="280">
  </div>
</p><br>
Repeat this step for all the meshes in your model.<br>
<br>
<b>(B)</b>Click on a mesh and click the Object Data Properties tab(1). At the top of that tab you'll see the <b>Vertex Groups</b> section. Just repeatedly click on the minus symbol to the right of that list(2) to remove all of the existing vertex groups.
<p>
  <div align="center">
  <img src="/img/blendLODstep2-5B.PNG" height="389">
  </div>
</p><br>
Repeat this step for all the meshes in your model. When you're done, your heirarchy should look like this:<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep2-5C.PNG" height="132">
  </div>
</p><br>
<br>

<h2>Step 3: Blender LODs</h2>
Now we're ready for the good stuff. LODs. How you go about creating the LODs for your models is completely subjective, and there are several ways to do this. I'll introduce one of the simplest methods I use and leave alternative methods as an exercise for the reader.
<br><br>
<b>1-</b> The first order of business is to duplicate your meshes(shift+D with an object selected) and rename them with the <b>_LOD[num]</b> suffix. Using the ybot, for example, I duplicated Alpha_Joints first. I renamed the original high-poly mesh to Alpha_Joints_LOD500 and the copy to Alpha_Joints_LOD300. Then I did the same thing for the Alpha_Surface mesh, ending up with Alpha_Surface_LOD500 and Alpha_Surface_LOD300. I'll usually just rename the Object and the ObjectData(Mesh) using the same name, like so:
<br><br>
<p>
  <div align="center">
  <img src="/img/blendLODstep3-1.PNG" height="214">
  </div>
</p><br>
Repeat this step for as many LODs you want your model to have.
<br><br>
<i>The <b>_LOD[num]</b> suffix is highly subjective and will vary greatly depending on your project's needs. You are just going to have to practice these awhile to get a good set of values that work for what you are doing. A lot of times I use _LOD500, _LOD300, _LOD200, _LOD100, and _LOD2. What's important is as each number decreases, so should the number of polygons.</i>
<br><br>

<b>2-</b> Leave the original high-poly objects as they are(in my case _LOD500 objects) and click one of the objects with the next lowest LOD suffix(in my case _LOD300). 
With the object you want to LOD selected(1), click the Modifier tab(2), and then click <b>Add Modifier</b>(3). Choose <b>Decimate</b> from the dropdown.<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep3-2.PNG" height="568">
  </div>
</p><br>

<b>3-</b> The Decimate modifier has a few different ways that it decreases polygons in a mesh. The two I use regularly are <b>Collapse</b> and <b>Un-Subdivide</b>. In the case of Collapse(1), use the <b>Ratio</b>(2) slider to decrease triangles. For Un-Subdivide(1), increase the number of <b>Iterations</b>(2) and Blender reduces the geometry down in steps. Just pay attention to the number of faces shown in the modifier interface and decrease the polygons for each LOD mesh you have created, then click <b>Apply</b>(3)<br>.
<p>
  <div align="center">
  <img src="/img/blendLODstep3-3.PNG" height="218">
  </div>
</p><br>
You can find more info about this modifier in the <a href="https://docs.blender.org/manual/en/latest/modeling/modifiers/generate/decimate.html" target="_blank">Blender 
docs</a>.
<br><br>

<b>4-</b> With all of the duplicate versions of your model stacked up, in the viewport you'll probably see 'squiggly' lines all across the meshes where they overlap.<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep3-4.PNG" height="450">
  </div>
</p><br>
This is perfectly normal, and you can click the eye icon to show/hide any LOD meshes while you are working with them. Keep in mind that when all of this gets to Torque only one of these meshes will display at a time, based on how far away the model is from the screen.
<br><br>
If you've created all of the duplicate versions of your model and applied the Decimate modifier to each one, you're ready to export it all. Just leave all the meshes stacked up and don't add any extras such as nodes or Bounds shapes. Go ahead and export the collection of LODs from Blender in the <b>.fbx</b> format. In Blender 3.8, I just export these using default export settings.
<br><br>

<h3>Step 4: Mixamo Specifics</h3>
The reason why it is important that we work with the <b>.fbx</b> format(even if we ultimately want to use .DAE) is we are about to upload this collection of LODs back to Mixamo for rigging. The mixamo rigger only accepts a few formats, and .DAE is not one of them.
<br><br>
<b>1-</b> Login to Mixamo and look to the top right of the screen. Under the Download button click the button <b>Upload Character</b>. A popup will appear where you can drag and drop your exported .fbx file. Go ahead and do that now and let Mixamo process it. Mixamo's Auto-Rigger interface will appear, and if all as well your stack of LODs should be displayed.

<b>2-</b> Click Next to begin the auto-rigging process, it's pretty simple to use. You'll just drag the circles onto the correct joints as instructed, but to clear any confusion there is an image on the right side of the Auto-Rigger interface that shows what that should look like when complete. Mine looked like this:<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep4-2.PNG" height="454">
  </div>
</p><br>

<b>3-</b> Click Next when done, and allow the Auto-Rigger to do its magic. Within a couple minutes, you should be presented with an example idle animation hooked into your 
model.<br>
<p>
  <div align="center">
  <img src="/img/blendLODstep4-3.PNG" height="317">
  </div>
</p><br>

<b>4-</b> Click Next and you'll see your uploaded model in the standard TPose. Download that file in the format you are working with and you are good to go! You are now free to import this model into Torque and add any additions such as nodes, bounding boxes, and so on using Torque's TSShapeConstructor. By following this process all of the LODs are rigged to the Mixamo bones, and will leverage Torque's LOD system.

<h3>Additional Notes</h3>
When using this workflow there are a couple important things to consider. One is that once you upload your character to Mixamo for rigging, Mixamo will store that character in your account. This way, when you log out and come back you'll be greeted with the same model you uploaded so you can come back and grab extra aimations. However, if you were to upload an additional character, you would remove the existing one and thus lose the rigged character you had. To combat this, I always backup the original model file that holds its collection of LODs(i.e. ybot_preMix.fbx).
<br><br>
Another thing to remember is that Torque also uses nodes with its LOD system, so you'll need to create the proper LOD nodes using Torque's TSShapeConstructor.
<br><br>
I started using this workflow due to the process of exporting from Blender into Torque being a huge can of worms and usually resulting in deformed skins on the model. There are ways around this, and ultimately all of what I've introduced here can also be achieved using Blender. However, as Blender versions change these things can break again and this allows us to just let Mixamo handle rigging and export so we don't have to. I hope this helps anyone having issues and needing LODs for character models. Happy blending and mixing!







