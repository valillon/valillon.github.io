---
layout: single
title:  "Creating funny caricatures with automatic face morphing"
excerpt: "Some examples of facial expression transference between two faces with Dlib and OpenCV to automatically create facial caricatures."
date: 2018-12-03 12:00:00
classes: wide
tags: 
    - machine learning
    - programming
    - python
    - neural networks
    - dlib
    - caricatures
    - art
    - cartoon
---

Any political attribution to this post is purely coincidence.
{: .notice}

## A sweet accident

This is my first post here and by coincidence it starts with an accident. A friend of mine told me he was making a video for promoting [Vei a Vei](https://deveiavei.org), a neighborhood association for helping people who temporarily require food support and other basic necessities. He was using some semi-commercial software to mimic the morphing effect popularized by the marvelous [Black or White](https://www.youtube.com/watch?v=F2AitTPI5U0) video-clip. However, because he had to pay some bills for unlocking some functional features, he was doing it quite manually in the end. I wanted to help him, so after a quick search on the Internet, I ended up playing around with a very fancy [face morphing code](https://www.learnopencv.com/face-morph-using-opencv-cpp-python). I wanted to make it easier for my friend, a plug-and-play thing, or at least something to be run in the terminal prompt with a single execution instruction. The original post required manually adding some extra facial landmarks and just a single morphing frame was generated. So, while adapting the code, I made the mistake of not clearing the landmarks coordinates for every frame iteration. That caused the landmarks for one of the faces to be completely transferred to the other. That first cartoon face I saw was so funny that I thought it was a joke hidden in the original code. But it wasn't. It was my bug! A bit of uncertainty in digital arts is like salt and pepper for a good recipe. I heard once from the great [Sergi Jordà](https://www.dtic.upf.edu/~sergi) that some randomness is a good ingredient for extending interactive systems to more expressive terrains. That comes to my mind often, to let the emotions and other unexpected facts be, stay around, just flow in the space and time.

<div class="align-center" style="width: 50%">
	<table>
	<tr style="border:0px;"><td valign="top" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton.jpg" width="143">
	</td><td rowspan="2" align="left" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/donald_trump-hillary_clinton-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom" align="right" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/donald_trump_delaunay.jpg" width="143">
	</td></tr></table>
	<figcaption align="center">A Hillary Clinton's caricature morphed with the Donal Trump's facial expression.</figcaption>
</div>

## The morphing dream

[Morphing](https://en.wikipedia.org/wiki/Morphing) is as old as the first human imaging to transform a thing into another. It is the alchemist dreaming in transmuting organic matter into noble metals. It is the magic that converts the wand into a bouquet of flowers. It is the smooth transition in which two objects become the other. Two connected states of two different matters, or simply the manifestation of a single matter having an untangled dual state.

Back on Earth, digital morphing got popular due to multiple films and video clips during the 90's. By then, techniques outlined curvatures in both the origin and target images, to later make them match by applying some sort of spatial transformation, like the [Beier Neely](https://en.wikipedia.org/wiki/Beier–Neely_morphing_algorithm) algorithm. But the procedure to create a single clip was really [tedious](https://www.youtube.com/watch?v=Q5Z1_n0S1uo). Nowadays, the object recognition algorithms based on computer vision have let to drastically automate the procedure, especially for human faces. For instance, [Dlib](http://dlib.net) is a great toolkit for automatically detecting face landmarks in a way that it was hardly to believe just ten years ago, high-end smart phones operate all kind of face-based magic tricks, or the fact that some popular apps like *faceApp* creates awesome makeup effects, surprising everyone by making the [Rijksmuseum's paintings](https://twitter.com/ollyog/status/863026581010354177/photo/1) amusingly smile.

{% capture fig_img %}
![Foo]({{ '/assets/caricatures/antonio_de_la_gandara.jpg' | relative_url }}){: .width-half .align-center}
{% endcapture %}
<figure class="align-center">
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption align="center">Antonio de la Gándara (1861-1917) - The cabaret's gentlemen and four artists of the <i>Chat Noir</i> (1984).</figcaption>
</figure>

## Let's morph it

The [FaceMorph](https://github.com/valillon/FaceMorph) code performs automatically the following steps:		
1. Facial landmarks recognition on both the origin and target faces ([Dlib](http://dlib.net)).	
2. Triangular [Delaunay](https://en.wikipedia.org/wiki/Delaunay_triangulation) segmentation.	
3. [Affine transformation](https://en.wikipedia.org/wiki/Affine_transformation) between the Delaunay triangles of both faces.
4. [Alpha blending](https://en.wikipedia.org/wiki/Alpha_compositing#Alpha_blending) on the paired triangles with a given transparency.

To create a caricature effect, the alpha blend on it must be set to either the maximum or the minimum values in order to make completely appear one of the faces, and then the affine transformation must target the landmarks of the other face. 

A great choice for shaping funny caricatures is to pair faces with completely different facial dimensions between eyes, nose or mouth. Not only grins, but also disparate poses can generate crazy distortions too. 

<div class="align-center" style="width: 50%">
	<table>
	<tr style="border:0px;"><td valign="top" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/donald_trump.jpg" width="143">
	</td><td rowspan="2" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton-donald_trump-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom" align="right" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton_delaunay.jpg" width="143">
	</td></tr></table>
	<figcaption align="center">A Donald Trump's caricature morphed with the Hillary Clinton's facial expression.</figcaption>
</div>

<div class="align-center" style="width: 50%">
	<table>
	<tr style="border:0px;"><td valign="top" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton.jpg" width="130">
	</td><td rowspan="2" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez-hillary_clinton-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom" align="right" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez_delaunay.jpg" width="130">
	</td></tr></table>
	<figcaption align="center">A Hillary Clinton's caricature morphed with the Velazquez's facial expression.</figcaption>
</div>

In some cases however if the poses between the two faces are so distant, the morphing effect gets too much distorted or simply broken. That's the case when the two faces are looking at opposite directions or they are located at very far away relative locations in the image. For the first issue, an horizontal flip can make the trick, while for the second one, a fair enough head alignment towards the center can work fine. 


<div class="align-center" style="width: 100%">
	<table>
	<tr style="border:0px;"><td valign="top" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/van_gogh.jpg" width="160" align="right">
	</td><td rowspan="2" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/paul_goughin-van_gogh-0001.jpg" width="375">		
	</td><td rowspan="2" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/van_gogh-paul_goughin-0001.jpg" width="345">		
	</td><td valign="top" align="left" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/paul_goughin.jpg" width="160">
	</td></tr>
	<tr><td valign="bottom" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/paul_goughin_delaunay.jpg" width="160">
	</td><td valign="bottom" align="left" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/van_gogh_delaunay.jpg" width="160">
	</td></tr></table>
	<figcaption align="center">
		Caricatures of Van Gogh and Paul Goughin morphed with each other's facial expression. Note the Paul Goughin's self-portrait was originally heading to the right side of the paining.
	</figcaption>
</div>

Now we rise the score with a creepy series of disturbing faces. Note that some of them are limit cases in which some facial parts get too distorted, but still worked pretty good.

<div class="align-center" style="width: 100%">
	<table>
	<tr style="border:0px;"><td valign="top" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/galileo.jpg" width="130" align="right">
	</td><td rowspan="2" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton-galileo-0001.jpg" width="250">		
	</td><td rowspan="2" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez-galileo-0001.jpg" width="300">		
	</td><td valign="top" align="left" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/galileo.jpg" width="130">
	</td></tr>
	<tr><td valign="bottom" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/hillary_clinton_delaunay.jpg" width="130">
	</td><td valign="bottom" align="left" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez_delaunay.jpg" width="130">
	</td></tr></table>
	<figcaption align="center">
		A couple of caricatures of Galileo Galilei morphed with the Hillary Clinton's (left) and Velazquez's (right) facial expressions.
	</figcaption>
</div>


<div class="align-center" style="width: 50%">
	<table>
	<tr style="border:0px;"><td valign="top" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/velazquez.jpg" width="125">
	</td><td rowspan="2" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/jim_carrey-velazquez-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom" align="right" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/jim_carrey_delaunay.jpg" width="125">
	</td></tr></table>
	<figcaption align="center">A Velazquez's caricature morphed with the Jim Carrey's facial expression.</figcaption>
</div>

<div class="align-center" style="width: 50%">
	<table>
	<tr style="border:0px;"><td valign="top" align="right" style="border:0px">
		<img src="{{site.baseurl}}/assets/caricatures/batman.jpg" width="132">
	</td><td rowspan="2" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/jocker-batman-0001.jpg" height="300">		
	</td></tr><tr><td valign="bottom" align="right" style="border:0px;">
		<img src="{{site.baseurl}}/assets/caricatures/jocker_delaunay.jpg" width="132">
	</td></tr></table>
	<figcaption align="center">A Batman's (Christian Bale) caricature with the Joker's (Heath Ledger) facial expression.</figcaption>
</div>

That's all folks. Keep morphing and have fun.







