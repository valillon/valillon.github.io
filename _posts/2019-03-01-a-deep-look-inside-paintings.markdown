---
layout: single
title:  "A Deep Look Inside Paintings"
excerpt: "About deep learning enabling to reconstruct depth of paintings"
date: 2019-02-28 09:01:02
classes: wide
tags: 
    - deep learning
    - neural networks
    - openframeworks
    - megadepth
    - painting
    - art
---

Light and space is the origin of everything, the two gametes which engendered the Universe. Likewise, in the painting, a window to the Universe, the space becomes canvas and the light emerges in the form of colorful pigments. Somebody said once that the painting is just that, light and space. And it is still mysterious how a canvas and a bunch of pigments can condense the deepest corners of the human soul.

In particular, the light confined in the Meninas’ room can manifest itself the history of painting. Everything can be explained therein, any emotion can be found impregnated in those Velazquez's strokes. The atmosphere in that room has inspired painters, fascinated artists, and hypnotized audiences for centuries until our days. The mere idea of having the possibility to get not inside the canvas, but inside the room itself, is so attractive that triggers innumerable imaginary visions. By imaging yourself inside the painting next to Velazquez, walking between the Meninas, facing the Kings' mirror, or looking through that mysterious door at the end of the room, conforms a spatial-temporal transportation through the wrinkles of the Universe.

I don't remember when I started to be deeply fascinated by the painting. Recently, several months ago, I discovered that a neural network could estimate depth from a single image. I instantly dreamed about the idea of having a deep look inside that Meninas’ room.

## Neural Models

One of the evolutionary strategies to understand [depth in monocular vision](https://en.wikipedia.org/wiki/Depth_perception#Monocular_cues), which means looking through a single eye, has to do with the ability to (visually) remember that size decreases with distance. In other words, if a person in a picture has the same size as a building, the former is probably nearer than the latter. Actually, size-scaling was one of the first strategies which painters used to give [perspective](https://en.wikipedia.org/wiki/Perspective_(graphical)) to their paintings. And the other way around, many people use it to create disparate funny visual illusions like the well-known [Ames room](https://en.wikipedia.org/wiki/Ames_room).

Nowadays, deep neural networks for computer vision have been trained with so many objects, in so many perspectives, in so many locations and illuminations, that in the end they are able to learn what should be near and far in the scene. One of the first neural networks to estimate depth from a single image was [Monodepth](https://github.com/mrharicot/monodepth). However, this network was trained with street images and does not generalize properly with other types of scenes. That took me later to [Megadepth](https://github.com/lixx2938/MegaDepth), which was trained with thousands of images of city landscapes and impressively generalized better for indoor scenes and human bodies, delivering much cooler results. 

## Projection and lights

In order to project depth into space, focal projection is used instead of parallel projection. However most of the paintings do not present an evident optical perspective. Thus, for compromise, all the 3D models are projected with a similar and quite long focal distance. Regarding lights, no external spotlight illuminates the 3D canvases. The light contained inside the paintings is so marvelous that generates in itself such chiaroscuro atmospheres.

## The deep look

Looking carefully at the generated depths, specially when they are 3D projected, one can realize that buildings and elements like roofs, windows, or bridges are prettily reconstructed. Actually, Megadepth was specifically trained for that purpose. Furthermore, human silhouettes were also somehow considered during training and so people are gratefully reconstructed too. However, one can notice across all the processed paintings that several heads, and curiously heads of women, failed to be correctly estimated in their depth. On the contrary, the Millet's pitchfork for instance is nicely molded, even though the network probably never saw a tool like that before. Regardless the inaccuracies and visibly wrapped and twisted volumes, from an artistic point of view, new pictorial landscapes emerged so beautifully in the end. And that made me get so excited.

## The painters' paintings

- Diego Velázquez, *Las meninas* (1656).
- Francisco de Goya, *Los fusilamientos* (1814).
- Jean-François Millet, *L'Angélus* (1859).
- Edgar Degas, *Répétition de ballet* (1873).
- Georges Seurat, *Un dimanche après-midi à l'Île de la Grande Jatte* (1886).
- Pablo Picasso, *Mujer en azul* (1901).
- Ramon Pujolà, *Salamanca* (2018).

---
To render these 3D models check [DepthPainter](http://github.com/valillon/DepthPainter) out.	

---

{% include video id="320354403" provider="vimeo" %}


{% include figure image_path="assets/deeplook/velazquez_01.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_02.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_03.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_04.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_05.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_06.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_07.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_08.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_09.jpg" %}
{% include figure image_path="assets/deeplook/velazquez_10.jpg" %}
{% include figure image_path="assets/deeplook/millet_01.jpg" %}
{% include figure image_path="assets/deeplook/millet_02.jpg" %}
{% include figure image_path="assets/deeplook/millet_03.jpg" %}
{% include figure image_path="assets/deeplook/millet_04.jpg" %}
{% include figure image_path="assets/deeplook/millet_05.jpg" %}
{% include figure image_path="assets/deeplook/millet_06.jpg" %}
{% include figure image_path="assets/deeplook/millet_07.jpg" %}
{% include figure image_path="assets/deeplook/millet_08.jpg" %}
{% include figure image_path="assets/deeplook/goya_01.jpg" %}
{% include figure image_path="assets/deeplook/goya_02.jpg" %}
{% include figure image_path="assets/deeplook/goya_03.jpg" %}
{% include figure image_path="assets/deeplook/goya_04.jpg" %}
{% include figure image_path="assets/deeplook/goya_05.jpg" %}
{% include figure image_path="assets/deeplook/degas_01.jpg" %}
{% include figure image_path="assets/deeplook/degas_02.jpg" %}
{% include figure image_path="assets/deeplook/degas_03.jpg" %}
{% include figure image_path="assets/deeplook/degas_04.jpg" %}
{% include figure image_path="assets/deeplook/picasso_01.jpg" %}
{% include figure image_path="assets/deeplook/picasso_02.jpg" %}
{% include figure image_path="assets/deeplook/picasso_03.jpg" %}
{% include figure image_path="assets/deeplook/picasso_04.jpg" %}
{% include figure image_path="assets/deeplook/picasso_05.jpg" %}
{% include figure image_path="assets/deeplook/picasso_06.jpg" %}
{% include figure image_path="assets/deeplook/picasso_07.jpg" %}
{% include figure image_path="assets/deeplook/picasso_08.jpg" %}
{% include figure image_path="assets/deeplook/picasso_09.jpg" %}
{% include figure image_path="assets/deeplook/seurat_01.jpg" %}
{% include figure image_path="assets/deeplook/seurat_02.jpg" %}
{% include figure image_path="assets/deeplook/seurat_03.jpg" %}
{% include figure image_path="assets/deeplook/seurat_04.jpg" %}
{% include figure image_path="assets/deeplook/seurat_05.jpg" %}
{% include figure image_path="assets/deeplook/seurat_06.jpg" %}
{% include figure image_path="assets/deeplook/seurat_08.jpg" %}
{% include figure image_path="assets/deeplook/seurat_09.jpg" %}
{% include figure image_path="assets/deeplook/pujola_01.jpg" %}
{% include figure image_path="assets/deeplook/pujola_02.jpg" %}
{% include figure image_path="assets/deeplook/pujola_03.jpg" %}
{% include figure image_path="assets/deeplook/pujola_04.jpg" %}
{% include figure image_path="assets/deeplook/pujola_05.jpg" %}
{% include figure image_path="assets/deeplook/pujola_06.jpg" %}

The original assets used in this project.

<figure class="half">
<a href="/assets/deeplook/velazquez_meninas.jpg">		<img src="/assets/deeplook/velazquez_meninas.jpg"></a>
<a href="/assets/deeplook/velazquez_meninas_depth.jpg">	<img src="/assets/deeplook/velazquez_meninas_depth.jpg"></a>
<a href="/assets/deeplook/goya_mayo.jpg">				<img src="/assets/deeplook/goya_mayo.jpg"></a>
<a href="/assets/deeplook/goya_mayo_depth.jpg">			<img src="/assets/deeplook/goya_mayo_depth.jpg"></a>
<a href="/assets/deeplook/millet_angelus.jpg">			<img src="/assets/deeplook/millet_angelus.jpg"></a>
<a href="/assets/deeplook/millet_angelus_depth.jpg">	<img src="/assets/deeplook/millet_angelus_depth.jpg"></a>
<a href="/assets/deeplook/degas_dancers.jpg">			<img src="/assets/deeplook/degas_dancers.jpg"></a>
<a href="/assets/deeplook/degas_dancers_depth.jpg">		<img src="/assets/deeplook/degas_dancers_depth.jpg"></a>
<a href="/assets/deeplook/seurat.jpg">					<img src="/assets/deeplook/seurat.jpg"></a>
<a href="/assets/deeplook/seurat_depth.jpg">			<img src="/assets/deeplook/seurat_depth.jpg"></a>
<a href="/assets/deeplook/picasso_azul.jpg">			<img src="/assets/deeplook/picasso_azul.jpg"></a>
<a href="/assets/deeplook/picasso_azul_depth.jpg">		<img src="/assets/deeplook/picasso_azul_depth.jpg"></a>
<a href="/assets/deeplook/pujola.jpg">					<img src="/assets/deeplook/pujola.jpg"></a>
<a href="/assets/deeplook/pujola_depth.jpg">			<img src="/assets/deeplook/pujola_depth.jpg"></a>
</figure>



