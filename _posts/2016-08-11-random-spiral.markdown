---
layout: post
title:  "乱数を与えた螺旋"
date:   2016-08-11 13:11:15 +0900
categories: Processing
image: /images/thumbnails/20160811-2.png
---

乱数を使ってより自然なカオスを生み出してみる。  
今回は螺旋の半径に乱数を与えたものを連続出力している。

## デモ

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/processing.js/1.4.8/processing.min.js"></script>
<script type="text/processing" data-processing-target="processing-canvas">
ffloat radius = 150;
int sizeX = 500;
int sizeY = 500;
int centX = sizeX/2;
int centY = sizeY/2;

float x, y;
float lastX = -999, lastY = -999;

void setup() {
  size(500, 500);
  frameRate(12);
}

void draw() {
  background(255);
  drawDefaultCircle(centX, centY, 100);
  drawSpiral(centX, centY);
}

void drawDefaultCircle(int centX, int centY, int radius) {
  strokeWeight(5);
  stroke(0, 30);
  noFill();
  ellipse(centX, centY, radius*2, radius*2);
}

void drawSpiral(int centX, int centY) {
  stroke(20, 50, 70);

  float radius = 10;
  float radiusNoise = random(10);

  for (float angle = 0; angle <= 1440; angle += 5) {
    radius += 0.5;
    radiusNoise += 0.005;
    float thisRadius = radius + (noise(radiusNoise) * 200) - 100;
    float radian = radians(angle);
    x = centX + (thisRadius * cos(radian));
    y = centY + (thisRadius * sin(radian));

    if (lastX > 0) {
      line(x, y, lastX, lastY);
    }

    lastX = x;
    lastY = y;
  }
}
</script>
<div>
  <canvas id="processing-canvas" class="canvas" width="200px" height="200px"></canvas>
</div>

## ソースコード

{% highlight java %}
float radius = 150;
int sizeX = 500;
int sizeY = 500;
int centX = sizeX/2;
int centY = sizeY/2;

float x, y;
float lastX = -999, lastY = -999;

void setup() {
  size(500, 500);
  frameRate(12);
}

void draw() {
  background(255);
  drawDefaultCircle(centX, centY, 100);
  drawSpiral(centX, centY);
}

void drawDefaultCircle(int centX, int centY, int radius) {
  strokeWeight(5);
  stroke(0, 30);
  noFill();
  ellipse(centX, centY, radius*2, radius*2);
}

void drawSpiral(int centX, int centY) {
  stroke(20, 50, 70);

  float radius = 10;
  float radiusNoise = random(10);

  for (float angle = 0; angle <= 1440; angle += 5) {
    radius += 0.5;
    radiusNoise += 0.005;
    float thisRadius = radius + (noise(radiusNoise) * 200) - 100;
    float radian = radians(angle);
    x = centX + (thisRadius * cos(radian));
    y = centY + (thisRadius * sin(radian));

    if (lastX > 0) {
      line(x, y, lastX, lastY);
    }

    lastX = x;
    lastY = y;
  }
}


{% endhighlight %}

## メモ
* [noise()](https://processing.org/reference/noise_.html)
  - Returns the Perlin noise value at specified coordinates. Perlin noise is a random sequence generator producing a more natural, harmonic succession of numbers than that of the standard random() function.
    - パーリンノイズを用いた乱数生成
      - 詳細は [こちら](http://postd.cc/understanding-perlin-noise/)
