---
layout: post
title:  "Processing のサンプルを表示できるようにする"
date:   2016-08-07 23:28:31 +0900
categories: Processing
image: /images/thumbnails/20160807.jpg
---

例として、 [本家サイト](https://processing.org/examples/sinewave.html) にある SignWave を表示してみる。  
取り急ぎベタ書きで書いているのでおいおいテンプレート化していきたい。

## デモ

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/processing.js/1.4.8/processing.min.js"></script>
<script type="text/processing" data-processing-target="processing-canvas">
int xspacing = 16;   // How far apart should each horizontal location be spaced
int w;              // Width of entire wave

float theta = 0.0;  // Start angle at 0
float amplitude = 75.0;  // Height of wave
float period = 500.0;  // How many pixels before the wave repeats
float dx;  // Value for incrementing X, a function of period and xspacing
float[] yvalues;  // Using an array to store height values for the wave

void setup() {
  size(640, 360);
  w = width+16;
  dx = (TWO_PI / period) * xspacing;
  yvalues = new float[w/xspacing];
}

void draw() {
  background(0);
  calcWave();
  renderWave();
}

void calcWave() {
  // Increment theta (try different values for 'angular velocity' here
  theta += 0.02;

  // For every x value, calculate a y value with sine function
  float x = theta;
  for (int i = 0; i < yvalues.length; i++) {
    yvalues[i] = sin(x)*amplitude;
    x+=dx;
  }
}

void renderWave() {
  noStroke();
  fill(255);
  // A simple way to draw the wave with an ellipse at each location
  for (int x = 0; x < yvalues.length; x++) {
    ellipse(x*xspacing, height/2+yvalues[x], 16, 16);
  }
}
</script>
<div>
  <canvas id="processing-canvas" class="canvas" width="200px" height="200px"></canvas>
</div>

## ソースコード

{% highlight java %}
int xspacing = 16;   // How far apart should each horizontal location be spaced
int w;              // Width of entire wave

float theta = 0.0;  // Start angle at 0
float amplitude = 75.0;  // Height of wave
float period = 500.0;  // How many pixels before the wave repeats
float dx;  // Value for incrementing X, a function of period and xspacing
float[] yvalues;  // Using an array to store height values for the wave

void setup() {
  size(640, 360);
  w = width+16;
  dx = (TWO_PI / period) * xspacing;
  yvalues = new float[w/xspacing];
}

void draw() {
  background(0);
  calcWave();
  renderWave();
}

void calcWave() {
  // Increment theta (try different values for 'angular velocity' here
  theta += 0.1;

  // For every x value, calculate a y value with sine function
  float x = theta;
  for (int i = 0; i < yvalues.length; i++) {
    yvalues[i] = sin(x)*amplitude;
    x += dx;
  }
}

void renderWave() {
  noStroke();
  fill(255);
  // A simple way to draw the wave with an ellipse at each location
  for (int x = 0; x < yvalues.length; x++) {
    ellipse(x * xspacing, height / 2 + yvalues[x], 16, 16);
  }
}
{% endhighlight %}

## メモ
* TWO_PI (6.28318...)
  - 2πのこと。詳細は [こちら](http://www.technotype.net/processing/reference/TWO_PI.html)。
* theta
  - θのこと。フレームごとの計算時での変化量を大きくすることで、単位時間当たりの振動数は大きくなる。
