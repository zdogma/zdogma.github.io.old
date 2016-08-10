---
layout: post
title:  "球体をゆっくり回してみた"
date:   2016-08-10 23:38:15 +0900
categories: Processing
image: /images/thumbnails/20160810.png
---

そろそろ3Dの世界に足を踏み入れることにした。

## デモ

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/processing.js/1.4.8/processing.min.js"></script>
<script type="text/processing" data-processing-target="processing-canvas">
void setup() {
  size(500, 300, P3D);
  sphereDetail(40);
}

void draw() {
  background(0);
  translate(width / 2, height / 2, 0);
  rotateY(frameCount * PI/360);
  sphere(80);
}
</script>
<div>
  <canvas id="processing-canvas" class="canvas" width="200px" height="200px"></canvas>
</div>

## ソースコード

{% highlight java %}
void setup() {
  size(500, 300, P3D);
  sphereDetail(40);
}

void draw() {
  background(0);
  translate(width / 2, height / 2, 0);
  rotateY(frameCount * PI/360);
  sphere(80);
}
{% endhighlight %}

## メモ
* [sphereDetail()](https://processing.org/reference/sphereDetail_.html)
  - Controls the detail used to render a sphere by adjusting the number of vertices of the sphere mesh.
    - 球体の面のきめ細かさ
* [sphere()](https://processing.org/reference/sphere_.html)
  - A sphere is a hollow ball made from tessellated triangles.
    - 球体を生成する
