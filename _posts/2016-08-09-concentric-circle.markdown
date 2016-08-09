---
layout: post
title:  "カラフルな円をランダムに発生させる"
date:   2016-08-09 22:42:15 +0900
categories: Processing
image: /images/thumbnails/20160809.jpg
---

「ぼーっと眺めていても飽きないもの」というテーマで作ってみた。  
delay() が Processing.js では使えないみたいで、やや目まぐるしくなっている。

## デモ

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/processing.js/1.4.8/processing.min.js"></script>
<script type="text/processing" data-processing-target="processing-canvas">
float circleX, circleY;
int MaxCircleCount = 50;

int diameter = 0;
int circleCount = 0;

void setup() {
  size(640, 640);
  circleX = width / 2;
  circleY = height / 2;
  background(0);
  stroke(0);
  strokeWeight(0);
  frameRate(12);
}

void draw() {
  if (circleCount <= MaxCircleCount) {
    fill(random(255), random(255), random(255), 1000);
    ellipse(random(width), random(height), diameter, diameter);
    diameter += 5;
    circleCount++;
  } else {
    background(0);
    diameter = 0;
    circleCount = 0;
  }
}
</script>
<div>
  <canvas id="processing-canvas" class="canvas" width="200px" height="200px"></canvas>
</div>

## ソースコード

{% highlight java %}
float circleX, circleY;
int MaxCircleCount = 100;

int diameter = 0;
int circleCount = 0;

void setup() {
  size(640, 640);
  circleX = width / 2;
  circleY = height / 2;
  background(0);
  stroke(0);
  strokeWeight(0);
  frameRate(12);
}

void draw() {
  if (circleCount <= MaxCircleCount) {
    fill(random(255), random(255), random(255), 1000);
    ellipse(random(width), random(height), diameter, diameter);
    diameter += 3;
    circleCount++;
  } else {
    // 最大数を超えたら初期化する
    background(0);
    diameter = 0;
    circleCount = 0;
    // delay(500);
  }
}
{% endhighlight %}

## メモ
* [stroke()](https://processing.org/reference/stroke_.html)
  - Sets the color used to draw lines and borders around shapes.
    - 線(枠)の色を変える
* [strokeWeight()](https://processing.org/reference/strokeWeight_.html)
  - Sets the width of the stroke used for lines, points, and the border around shapes.
    - 線(枠)の太さを変える
* [fill()](https://processing.org/reference/fill_.html)
  - Sets the color used to fill shapes.
    - 塗りつぶしの色を変える
* [frameRate()](https://processing.org/reference/frameRate.html)
  - The system variable frameRate contains the approximate frame rate of a running sketch.
    - 毎秒表示するフレームの数を変更する
* [delay()](https://processing.org/reference/delay_.html)
  - The delay() function halts for a specified time.
    - msec で sleep させる
    - Processing.js では動かなかったが、Java だと動く
