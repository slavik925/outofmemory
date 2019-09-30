---

title: Drawing in CSS - how and why?
date: "2019-09-30T16:00:00Z"

---

This is a first article in a short series inspired by CSS/JS Conf 2019 at Budapest.

The very first topic there was presented by Giulia Cardieri https://twitter.com/ggiulia - "CSS Games and Drawings in the real world: useful or just fun" which was actually quite inspiring.

The main idea is to bring more attention to CSS drawing capabilities, get some fun and experience while code in CSS not in a very usual manner. 

You can familiarize with one of her projects written just for fun called [Dinokiki](http://dinokiki.com) (be careful with the sound).

And look into this nice Peppa Pig created completely with CSS:

https://codepen.io/marcindotka/pen/GrrKYE

But the most inspiring for me was this game written completely in HTML/CSS with zero JS! More over the game contains a simple logic to handle clicks on the cards and tracking whether you won or not.

https://codepen.io/giuliacardieri/pen/XYOZom

Looks awesome right? 

But how do you manage clicks and respond to an events without JS? The code and technic is pretty clever - just to use styled check box. When it checked - show one state otherwise another.

### But why?
Here are few points
* to develop creativity
* to extend you knowledge of CSS
* to get some fun!

### But how?

If you are struggling how to start. Here are few advices.
First you need to select something that you want to draw with a CSS. If your CSS knowledge is limited I recommend you to start from something simple.

Try to reproduce a simple logo (cold be Chrome for example), flower (🌼, 🌻), car (🚓, 🚗), clock(⏱️, ⏰), etc. 

Then follow this steps:

1. Break the drawing into simple geometric forms
2. Use HTML to represent pieces of the drawing
3. Use CSS to style and position each piece
4. Repeat

In my example I have started to draw the logo of the company where I'm working at:
 
https://codepen.io/slavik925/pen/BaBEzPL

Looks not very bad but the problem whit this code that it's absolutely unresponsive. This means that the Collibra logo have fixed dimensions and can't be resized without changes the CCS code. So I decided to rewrite it using the CSS grids.

https://codepen.io/slavik925/pen/qBWzbwB

This is a way better I guess and to solidify my success :) I have to put this logo into this blog (you could find it on the main page at a top or at the bottom of this article). 
### What next?

I've definitely recommend you try to draw something or even go next level and try to create a game.

Here are resources that would help you to get/update your CSS knowledge.

* Official W3C site https://www.w3.org/Style/CSS/learning
* To lear more grid layout https://cssgridgarden.com, https://css-tricks.com/snippets/css/complete-guide-grid/
* To lean more flex box https://flexboxfroggy.com/, http://www.flexboxdefense.com/

And If you are already a CSS ninja go smoke the latest specs from here:
* https://github.com/w3c/csswg-drafts/blob/master/