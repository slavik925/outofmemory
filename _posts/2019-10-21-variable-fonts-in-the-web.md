---
layout: post
title: Variable fonts in the Web
date: "2019-10-21T13:00:00Z"
tags: ["Fonts", "Web"]

---

> This article continues CSS/JS Conf Budapest 2019 series. Previous one was about [Drawing in CSS - how and why?](https://outofmemory.dev/drawing-in-css/)

## Into

The year is almost _2020_ but still the dominant method of getting the information is text. The text consists of symbols and letters (or hieroglyphs). To represent the text on the web we are using fonts (typefaces).

In the early ages of the web, there was impossible to change default fonts on the web page until Netscape introduces the **font** tag in _1995_.

## How does the font represent on the Web?

The font - is a file that contains all possible variations of letters, symbols, hieroglyphs, etc.

Here is an example of how Roboto typeface looks inside.

![Roboto Character Set](/assets/roboto_char_set.png "Roboto Character Set")

Every webpage has a default font depending on your web browser and operating system. This means if you don’t specify the exact font for your web application you don’t know for sure how it might look for the end-user.

Besides this, the default font is not always a good choice if you want to increase readability and achieve good aesthetic appeal.

## How to add custom font into the web page?

The simplest way to include font from https://fonts.google.com/. But there are a lot of downsides such as limited variation of fonts and depending on 3rd party CDN. So I’m not going to cover this approach.

We will be including the custom font face file. First what you need is to choose between multiple fonts face file formats:

* SVG (yes it could be used as a font)
* EOT - Embedded OpenType
* OTF - OpenType
* TTF - TrueType
* WOFF/WOFF2 - Web Open Font format

Currently, the web moves to WOFF/WOFF2 file types. They are developed especially for the web by Mozilla.

WOFF2 has full browser support (almost except IE).

![WOFF2 browser support](/assets/caniuse_woff2.png "WOFF2 browser support")

## You got your font - what next?

After you could include it with font-face property in CSS and you are ready to use it.

```css
@font-face {
  font-family: FontName;
  src: url('path/filename.woff2') format('woff2'),
    url('path/filename.woff') format('woff');
}
```

## Font variants and what this all about.

After you include the font you will start to style your text. Add bold heading, change the font weight, start to emphasize text with italic style.

Here is where the problems start. For every additional style or the font-weight, you actually need to include a separate typeface file!

Here, for example, all of the variations of Roboto font.

![Roboto font variations](/assets/roboto_variations.png "Roboto font variations")

### Variable fonts and how they solve our problems.

To optimize this we have a relatively new technology called - Variable Fonts. It includes all the type variations into a single file!

![Font file structure](/assets/vf_file_concept.png "Font file structure")

To use Variable Font first you need to find a typeface you want and then find it variable font version. Typically it has **VF** suffix in the font name (e.g., _AmstelvarAlpha-VF.woff2_). And that’s it.

You could see how the variable font could do the CSS really awesome:

{% codepen https://codepen.io/mandymichael/pen/vYYLByj %}

The good article explaining in deep how the variable font technology works - https://developers.google.com/web/fundamentals/design-and-ux/typography/variable-fonts.

The main idea are in axis and how the font properties distributed between them.

![Varaible font axis](/assets/variable_fonts_venn_weight_axis_width_axis.png "Font file structure")

This technology has support of all the modern browsers so it could be used freely.

![Variable font support](/assets/caniuse_woff2.png "Variable font support")

## Conclusion.

If your web application has rich typography use of the Variable Fonts is a good choice.

An additional extra would be the animation possibilities that you could apply to the variable font.

![Variable font animation](/assets/variable_font_animation.gif "Variable font animations")

Variable fonts playgrounds:

* https://www.axis-praxis.org
* https://v-fonts.com
