---
layout: post
title: Understand the Basics (SASS)
description: "When working on the web, don't forget the basics!"
image: "understand_the_basics.png"
categories:
  - html
  - css
  - javascript
  - js
  - scss
  - sass
---

![](/public/images/scss_css.svg)

The other day I was watching a web-cast from Hubspot. The web-cast centered around a simple question "Can I use SASS on your blog theming platform." The answer to me seemed obvious. Yes you can use SASS or LESS or any language to style your blog so long as it compiles down to css. But I was surprised when the Hubspot team answered "No."

I understand the answer. NO you cannot upload SASS to Hubspot's servers and have it work like a CSS file. This doesn't have to stop you from being able to use SASS to style HTML anywhere, even on a platform like Hubspot.

Take a look at this snippet that defines a class named a `section` with a few modifier classes `--sm`, `--med`, `--lg`, `--xl`.

~~~scss
$sizes: [sm, med, lg, xl];
$color--black: #FFF;
$base-spacing: 1em;

.section {
  background-color: $color--black;

  @for $i from 1 through length($sizes) {
    $size: nth($sizes, $i);

    &--#{$size} {
      padding: $spacing * $i;
    }
  }
}
~~~

In this tiny snippet we're doing several things that you can't get done in vanilla css (<a href="https://developer.mozilla.org/en-US/docs/Web/CSS/calc" target="_blank">not</a> 100% <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/var" target="_blank">true</a>). We're looping over a list containing the sizes we want `[sm, med, lg, xl]` and doing some math where we multiply a `$base-spacing` by a factor of the index of the size in the previously stated list.

You can turn this SASS into CSS using [the SASS cli tool](https://sass-lang.com/install).

~~~bash
sass file.scss file.css
~~~

This gives you the following CSS.

~~~css
.section { color: #000; }
.section--red { background-color: #F00; }
.section--sm { padding: 1em; }
.section--med { padding: 2em; }
.section--lg { padding: 3em; }
.section--xl { padding: 4em; }
~~~

Try uploading this to Hubspot's blog theming platform. Now realize that you are now theming your Hubspot blog using SCSS. Easy! You could even further automate this with a [`--watch`](http://sassbreak.com/watch-your-sass/) and perhapse a `npm deploy` command that FTPs your generate css file(s) up to Hubspot!
