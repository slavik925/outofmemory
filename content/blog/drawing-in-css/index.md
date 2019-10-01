---

title: Drawing in CSS - how and why?
date: "2019-09-30T16:00:00Z"

---

This is a first article in a short series inspired by CSS/JS Conf 2019 at Budapest.

The very first topic there was presented by Giulia Cardieri https://twitter.com/ggiulia - "CSS Games and Drawings in the real world: useful or just fun" which was actually quite inspiring.

The main idea is to bring more attention to CSS drawing capabilities, get some fun and experience while code in CSS not in a very usual manner. 

You can familiarize with one of her projects written just for fun called [Dinokiki](http://dinokiki.com) (be careful with the sound).

And just look into this nice Peppa Pig created only with the CSS:

https://codepen.io/marcindotka/pen/GrrKYE

But the most inspiring for me was this Planets game written only using HTML/CSS features with no Java Script at all! More over the game handle clicks on the cards and even has a simple logic to track whether you win or not.

https://codepen.io/giuliacardieri/pen/XYOZom

Looks awesome right? 

But how do you manage clicks and respond to an events without JS? The code and technic is pretty clever - just to use styled check box. When it checked - show one state otherwise another.

### But why?
Here are few points:
* is to develop creativity
* to extend you knowledge of CSS
* to get some fun!

### But how?

If you are struggling on how to start, first you need to select something that you want to draw.
If your CSS knowledge is limited I recommend you to start from something simple.

Try to draw a simple logo (Chrome e.g), flower (🌼, 🌻), car (🚓, 🚗), clock(⏱️, ⏰), etc. 

Then follow this steps:

1. Break the drawing into simple geometric forms.
2. Use HTML to represent pieces of the drawing.
3. Use CSS to style and position each piece.
4. Repeat from step one if needed.

In my example I have started to draw the logo of the company where I'm working at:
 
https://codepen.io/slavik925/pen/BaBEzPL

Looks not very bad but the problem whit this code that it's absolutely unresponsive. This means that the Collibra logo have fixed dimensions and can't be resized without changes in the CCS code. So I decided to rewrite it using the CSS grids.

https://codepen.io/slavik925/pen/qBWzbwB

This is a way better I guess and to solidify my success :) I have put this logo into this blog (you could find it on the main page at a top or at the bottom of this article). 

### What next?

I've definitely recommend you try to draw something or even go next level and try to create game using only CSS.

In the end I want to leave a few resources that would help you to improve your CSS knowledge. 

* Official W3C site 
  * https://www.w3.org/Style/CSS/learning
* To lear more about grid layout 
  * https://cssgridgarden.com
  * https://css-tricks.com/snippets/css/complete-guide-grid/
* To lean more about flex box 
  * https://flexboxfroggy.com/
  * http://www.flexboxdefense.com/

As a good inspiration resource I recommend you [codepen.io](https://codepen.io) where you could find a lot of good examples or even subscribe on their weekly newsletter https://codepen.io/spark/. It will bring you a lot of fresh and interesting works every week.

If you are already a CSS ninja you could go and check the latest CSS specs:
* https://github.com/w3c/csswg-drafts