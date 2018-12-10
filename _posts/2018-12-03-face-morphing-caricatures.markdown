---
layout: post
comments: true
title:  "Creating funny caricatures with automatic face morphing"
excerpt: "Some examples of facial expression transference between two faces with Dlib and OpenCV to automatically create facial caricatures."
date:   2018-12-03 12:00:00
author: "valillon"
header-img: "assets/caricatures/donald_trump-hillary_clinton-0001.jpg"
img: "assets/caricatures/donald_trump-hillary_clinton-0001.jpg"
tags: [machine learning, programming, python, neural networks, dlib, caricatures, art, cartoon]
mathjax: false
---

> Any political attribution to this post is purely coincidence.

## A sweet accident

This is my first post here and casually starts with an accident. A friend of mine told me he was making a video for promoting [Vei a Vei](https://deveiavei.org), a neighborhood association for helping people who temporally require food support and other basic necessities. He was using some semi-commercial software to mimic the morphing effect popularized by the marvelous [Black or White](https://www.youtube.com/watch?v=F2AitTPI5U0) video-clip. But because he had to pay some bills for unlocking some functional features, he was doing it quite manually in the end. I wanted to help him. So after a quick search in Internet, I ended up playing around with a very fancy [face morphing code](https://www.learnopencv.com/face-morph-using-opencv-cpp-python). I wanted to make it easier for my friend, almost plug-and-play or at least something to be run in the terminal prompt with a single execution instruction. The original post required to add manually some extra facial landmarks and just a single morphing frame was generated. So while adapting the code, I made the mistake of not clearing the landmarks coordinates for every frame iteration. That caused the landmarks for one of the faces to be completely transfered to the other. That first cartoon face I saw was so funny that I though it was a joke hidden in the original code. But it wasn't. It was my bug! A bit of uncertainty in digital arts are like salt and pepper for a good receipt. I heard once from the great [Sergi Jordà](https://www.dtic.upf.edu/~sergi) that some randomness is a good ingredient for extending interactive systems to more expressive terrains. That comes to my mind often, to let the emotions and other unexpected facts be, stay around, just flow in the space and time.

<div class="imgcap">
	<table><tbody>
	<tr><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton.jpg" width="109.5">
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/donald_trump-hillary_clinton-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/donald_trump_delaunay.jpg" width="109.5">
	</td></tr>
	</tbody></table>
	<div class="thecap">
		Hillary Clinton's caricature morphed with Donal Trump's facial expression.
	</div>
</div>

## The morphing dream

[Morphing](https://en.wikipedia.org/wiki/Morphing) is as old as the first human could image something to be transformed into another thing. It is the alchemist dreaming in transmuting organic matter into noble metals. It is the magic that converts the wand into a bouquet of flowers. It is the smooth transition in which two objects become one another. Two connected states of two different matters or simply the manifestation of a single matter having an untangled dual state.

Coming back to the Earth, digital morphing got popular in the 90's in multiple films and video clips. Techniques were based on outlining curvatures in the initial and target images, to match them and then apply some spatial transformation, like the [Beier Neely](https://en.wikipedia.org/wiki/Beier–Neely_morphing_algorithm) algorithm. But the procedure to create a single clip was really [tedious](https://www.youtube.com/watch?v=Q5Z1_n0S1uo). These days object recognition algorithms in computer vision have let morphing software to advance in order to automate the procedure quite a lot. Specially for human faces. For instance, [Dlib](http://dlib.net) is a great toolkit for detecting facial landmarks in an image, among other things. The high-end smart-phones line operate all kind of face-based magic tricks for shooting pictures. Or popular apps like *faceApp* created awesome makeup effects and surprised everyone making [Rijksmuseum's paintings](https://twitter.com/ollyog/status/863026581010354177/photo/1) amusingly smile.

<div class="imgcap">
	<img src="{{site.baseurl}}/assets/caricatures/antonio_de_la_gandara.jpg" height="350">
	<div class="thecap">
		Antonio de la Gándara (1861-1917) - The cabaret's gentlement and four artists of the <i>Chat Noir</i> (1984).
	</div>
</div>


## Let's morph it

The [FaceMorph](https://github.com/valillon/FaceMorph) code performs automatically these steps:		
1. Facial landmarks recognition in both faces ([Dlib](http://dlib.net)).	
2. Triangular [Delaunay](https://en.wikipedia.org/wiki/Delaunay_triangulation) segmentation.	
3. [Affine transformation](https://en.wikipedia.org/wiki/Affine_transformation) between the Delaunay triangles of both faces.
4. [Alpha blending](https://en.wikipedia.org/wiki/Alpha_compositing#Alpha_blending) on the paired triangles with a given transparency.

To create a caricature effect, the alpha blend must be set either to the maximum or minimum in order to make appear completely one of the faces and then the affine transformation must target the other face's landmarks set. 

A great choice for shaping funny caricatures is to pair faces with completely different facial dimensions between eyes, nose or mouth. Not only grins, but also disparate poses can generate crazy distortions too. 

<div class="imgcap">
	<table><tbody>
	<tr><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/donald_trump.jpg" width="109.5">
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton-donald_trump-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton_delaunay.jpg" width="109.5">
	</td></tr>
	</tbody></table>
	<div class="thecap">
		Donald Trump's caricature morphed with Hillary Clinton's facial expression.
	</div>
</div>

<div class="imgcap">
	<table><tbody>
	<tr><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton.jpg" width="119">
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez-hillary_clinton-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez_delaunay.jpg" width="119">
	</td></tr>
	</tbody></table>
	<div class="thecap">
		Hillary Clinton's caricature morphed with Velazquez's facial expression.
	</div>
</div>


In some cases however if the poses between the two faces are so distant, the morphing effect gets too much distorted or simply broken. That's the case when the two faces are looking at opposite directions or they are located at very far away relative locations in the image. For the first issue an horizontal flip can make the trick and for the second one a fair enough head alignment towards the center can work fine. 


<div class="imgcap">
	<table><tbody>
	<tr><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/van_gogh.jpg" width="122">
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/paul_goughin-van_gogh-0001.jpg" width="355">		
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/van_gogh-paul_goughin-0001.jpg" width="323">		
	</td><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/paul_goughin.jpg" width="122">
	</td></tr>
	<tr><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/paul_goughin_delaunay.jpg" width="122">
	</td><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/van_gogh_delaunay.jpg" width="122">
	</td></tr>	
	</tbody></table>
	<div class="thecap">
		Caricatures of Van Gogh and Paul Goughin morphed with each other's facial expression. Note the Paul Goughin's self-portrait was originally heading to the right side of the paining
	</div>
</div>

Now we rise the score with a creepy series of disturbing faces. Note that some of them are limit cases in which some facial parts get too distorted, but still worked pretty good.


<div class="imgcap">
	<table><tbody>
	<tr><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/galileo.jpg" width="119">
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton-galileo-0001.jpg" width="250">		
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez-galileo-0001.jpg" width="300">		
	</td><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/galileo.jpg" width="119">
	</td></tr>
	<tr><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton_delaunay.jpg" width="119">
	</td><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez_delaunay.jpg" width="119">
	</td></tr>	
	</tbody></table>
	<div class="thecap">
		Caricatures of Galileo Galilei morphed with Hillary Clinton's (left) and Velazquez's (right) facial expressions.
	</div>
</div>

<div class="imgcap">
	<table><tbody>
	<tr><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez.jpg" width="120">
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/jim_carrey-velazquez-0001.jpg" height="250">
	</td></tr><tr><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/jim_carrey_delaunay.jpg" width="120">
	</td></tr>
	</tbody></table>
	<div class="thecap">
		Velazquez's caricature morphed with Jim Carrey's facial expression.
	</div>
</div>

<div class="imgcap">
	<table><tbody>
	<tr><td valign="top">
		<img src="{{site.baseurl}}/assets/caricatures/batman.jpg" width="120">
	</td><td rowspan="2">
		<img src="{{site.baseurl}}/assets/caricatures/jocker-batman-0001.jpg" height="250">
	</td></tr><tr><td valign="bottom">
		<img src="{{site.baseurl}}/assets/caricatures/jocker_delaunay.jpg" width="120">
	</td></tr>
	</tbody></table>
	<div class="thecap">
		Batman's (Christian Bale) caricature with the Joker's (Heath Ledger) facial expression.
	</div>
</div>

That's all folks. Keep morphing and have fun.




