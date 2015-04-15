---
layout: post
title: "Using inline block for columns"
description: "Father please forgive me since I've sinned&amp;used the [table] tag"
category: tech
tags: [DBC, CSS, HTML, column, table]
---
{% include JB/setup %} <link rel="stylesheet" href="{{ ASSET_PATH }}xcss/t2-sprite.css" type="text/css" media="screen" charset="utf-8">
###Father please forgive me since I've sinned&amp;used the [table] tag<img src="/assets/imgs/t2.jpg"  alt="BabyFacingKariFocused" width="20%"/>

<div id="article">
  <p>Lots of texts we read are presented in columns, thus it's naturally web developers want to split paragraph into columns for better design and visual effects. Also if we have several things on the webpages and we'd like to put it together <strong>side by side</strong>, column arrangements are also handy. In the past the [table][tb][tr] HTML tags were pretty efficient, but now the trend has turned to use CSS for some reasons. Here are some CSS table tricks:</p>

  <p>It's easy to make two blocks with defined width float and it will make columns, simply add <code> width: whatever%; float:left;</code> into their CSS, and you will see tableless columns.</p>
  <img id="f1" src="/assets/imgs/t2_empty.gif">
  <p>However, if there's footers followed by, one will find the footer will also float with the previous column.  Many people add <code>clear: both;</code> in footers to make the footer go to where it should be, and it seems works fine.  However, if you need to wrap both columns for placing somewhere, you will find the outer wrap ended with no height!  Don't blame CSS, its properties got cleared but the block is not properly closed, a very short wrapper is the consequence.</p>
  <img id="f2" src="/assets/imgs/t2_empty.gif">
  <p>To perfectly resolve such problem, <a href="http://www.sitepoint.com/simple-clearing-of-floats/">A. Walker proposed several ways</a> and setting the <code><em><strong>overflow: auto;</strong></em></code> in outer wrapper is my favorite. We don't need to clear both again, every block is still closed perfectly and looked beautifully with the footer.
   </p>
  <img id="f3" src="/assets/imgs/t2_empty.gif">
  <p>However, with <code>float</code> method sometimes we still need to deal with annoying inversed text-flow direction especially when <code>float:right;</code> is desired. How are we resolve this? <a href="http://www.highchairdesign.com/studio/inline-block">As pointed out by J.Hogue, there's another way to make CSS columns without <code>float</code></a>.  By assigning <code>display: inline-block;</code>, we can make elements act like text and they will sit side by side naturally. Apparently all text are aligned against the baseline, so <code>vertical-align:top;</code> will make the columns we always want.  Here I included a <a href="testib.htm">working HTML</a> or the <a href="http://jsfiddle.net/xjlin0/qx2qekqw/8/">JSfiddle</a> if you like to try. At last, this non-floating CSS column works since IE5.5 and here are the screen shots and <a href="http://www.browserstack.com/screenshots/9731943d2a22b27f513ed0364547307e076240b3">working proof from BrowserStack.com</a>. Let's enjoy the magic CSS code together!</p>
  <p><code>
    <em>CSS to make columns</em><br>
    display: inline-block;<br>
    *display:      inline;  /* IE5&amp;6 hack*/<br>
    *zoom:              1;  /* IE5&amp;6 hack*/<br>
    vertical-align:   top;<br>
    width:            49%;<br>
  </code></p>
  <img src="/assets/imgs/test.jpg" alt="BrowserStack.com testing" width="95%" />
</div>