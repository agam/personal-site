{:title "fun with processing spinning ellipses"
:layout :post
 :tags ["old-post"]}



[Processing](http://www.processing.org/) is intended to be a "creative coding" tool for non-programmers. It runs on top of Java, which is good enough to get started. The performance-anxious folks might eventually prefer something like [Cinder](http://libcinder.org/), which has C++ libraries instead.



I tweaked a simple snippet from a tutorial as a first step:



``` java

void setup() {

  size(600, 600);

  background(255);

  smooth();

  translate(300, 300);

  for (float i = 0; i < 360; i+= 0.3) {

    pushMatrix();

      rotate(radians(i));

      translate(0, 300);

      rotate(radians(i * 10));

      scale(map(sin(radians(i * 6)), -1, 1, .5, 1),

        map(sin(radians(i * 3)), -1, 1, .5, 1));

      drawEllipse();

    popMatrix();

  }

}



void drawEllipse() {

  noFill();

  stroke(255, 0, 0, 128);

  ellipse(0, 0, 600, 100);

}

```



and got some funky looking images.



{% img center http://farm4.staticflickr.com/3776/12162399443_14fd4eea37_z_d.jpg %}



{% img center http://farm8.staticflickr.com/7423/12162160705_5bc891cfa6_z_d.jpg %}



{% img center http://farm8.staticflickr.com/7341/12162577004_ee1689e71f_z_d.jpg %}



{% img center http://farm4.staticflickr.com/3742/12162819596_176dc1a9d3_z_d.jpg %}



{% img center http://farm6.staticflickr.com/5513/12162574864_10b7b887f3_z_d.jpg %}



Before you say _meh_, none of this is supposed to make you Michaelangelo. But it provides a kind of instant-gratification, since you can tweak your _sketch_, run it, tweak it again, run it again, and so on ... which is rather hard to come by.
