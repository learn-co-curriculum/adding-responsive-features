# Responsive Features Code-Along

## Objectives

* Practice what we have learned about CSS media queries, viewport settings, and
relative values in CSS by applying them to a nearly finished website

## Overview

In this exercise, we will build a responsive layout using CSS media queries and
the responsive design strategies we've been learning. All the files you need to
follow along are provided.  We will be practicing _desktop-down_ design here,
working with a website built for a larger screen, and making it look good on
a variety of smaller screen sizes.

Run `httpserver` in your terminal or open index.html to see how things look
currently. From a laptop screen, this site is looking pretty good! Let's see
how it looks on a smaller screen.

### Using Chrome Developer Tools to Test Our Site

We'll be using the Chrome developer tools throughout this lesson, so go ahead
and open them up.  It may be helpful to dock the developer tools to the bottom
of the page since we will be looking at screen widths.  You can do this be first
clicking the 'Customize and control DevTools' button, just to the left of the
'Close' button:

![Customize DevTools button](https://s3.amazonaws.com/ironboard-learn/chrome_developer_dock_button.png)

Then, at the top of the menu that appears, choose the Dock side that moves the
developer tools to the bottom of the browser.  

![Customize DevTools button](https://s3.amazonaws.com/ironboard-learn/chrome_developer_dock_menu.png)

While we're in the developer tools, toggle the device toolbar. The button to
toggle is in the upper left corner, just beside the select element button

![Customize DevTools button](https://s3.amazonaws.com/ironboard-learn/chrome_developer_device_toolbar_button.png)

With this toolbar, draggable bars will appear on the bottom and right side of
your HTML page. You can use these to grow and shrink this size of your page.
Once we set up some media queries, we will be able to see the CSS change when
the queries are triggered.

![Customize DevTools button](https://s3.amazonaws.com/ironboard-learn/chrome_developer_device_toolbar.png)

On the device toolbar, you can also choose to mimic the screen size of mobile
phones using the dropdown menu on the far left.  We'll leave this set to
_Responsive_, its default setting, for now.

Grab the draggable bar on the right side of your webpage and shrink the page.
Content on the page remains the same size, causing it to extend past the page
width. Typically having to scroll in two directions to view content isn't great
for user experience.  

If you switch from _Responsive_ to one of the mobile phone views, we can see the
whole page, but it is shrunk down to the point where text is unreadable.

We're going to build a solution to both of these issues.

### Viewport

We first want to tell devices how to handle our website and prevent them from
automatically zooming out.  In `index.html`, add the following meta tag inside
the `<head>` section of the html:

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

We can see the resulting effect of this by checking the website out while in a
mobile phone view.  Instead of zooming out to fit the entire web page, our
page content stays the same size.  We can now get to work on building media
queries without the concern that devices will automatically scale, and break,
 our styling.

### Adding `css/responsive.css`

We've currently got one linked CSS file in the `<head>` section, `style.css`,
which has all the existing styling.  There is actually quite a lot of CSS
provided! We won't go into detail about all of it, but definitely take a look
through it if you're interested in seeing how intricate styling can get. We want
to keep using `style.css`, but also need to override some of the properties
defined there when we build media queries.  Since we're working with _cascading_
style sheets, if we add a second CSS file directly after the first, anything
defined in the second file will take precedence over the first file.

Create a new file in the css folder of this lesson called `responsive.css`. To
do this on the terminal, type `cd css` from inside the add-responsive-features
lesson folder, then type `touch responsive.css`. Make sure to `cd ..` back out
before running `httpserver`, as it won't find an HTML file to run if you're
inside the CSS folder.

In the `<head>` section of `index.html`, add a new `<link>` tag:

```
<link rel="stylesheet" type="text/css" href="css/responsive.css">
```

Now, we've got a blank CSS file to work with, so let's start writing some media
queries!

### Setting Up Device Focused Media Queries

We can use as many media queries as we like, and if we really wanted to, could
have custom settings for each screen size to provide the _best_, most
_consistent_ experience any website has ever provided. The problem is that there
are [hundreds](http://viewportsizes.com/) of mobile and tablet screen sizes, so
trying to design for all of them would not be feasible or maintainable.  

Scaling that back a bit, we could still design a set of media queries, each for
a slightly different range of screen sizes.  For example, add the following set
of media queries your new CSS file:

```CSS
body {
  font-size: 100%;
  color: black;
}
h1 { font-size: 2.5em; }
p { font-size: 1em; }

  /* CSS for large desktop screens */

@media (min-width: 1025px) and (max-width: 1281px) {
  body {
    font-size: 105%;  
    color: red;
  }
  /* CSS for smaller laptops/desktops screens and even some large tablets */
}
@media (min-width: 768px) and (max-width: 1024px) {
  body {
    font-size: 110%;  
    color: blue;
  }
  /* CSS for small netbooks, most tablets in landscape mode */
}
@media (min-width: 481px) and (max-width: 767px) {
  body {
    font-size: 130%;  
    color: purple;
  }
  /* CSS for tablets in portrait, mobile phones in landscape orientation */
}
@media (max-width: 480px) {
  body {
    font-size: 140%;  
    color: green;
  }
  /* CSS for most mobile phones in portrait orientation */
}
```

You may need to restart `httpserver`, so go ahead and do that now, then check
out the site with Chrome developer tools open.  Switch back to Responsive on the
device toolbar, and try dragging the page to different widths.  On a laptop or
desktop, the font on the page will likely be black or red to start, but if you
shrink the page, the font will change color and gradually get bigger.

This approach generalizes the sizes of devices into a few categories, and is a
common way to design, especially as new CSS functionality has been introduced
such as CSS grid and flexbox. This approach can be handy if you are designing
distinctly different experiences between mobile and desktop, as it lets you
create delineated 'categories' for devices.

One additional concept the point out here: each of these queries is independent.
The media query that triggers between 481px and 767px will not be affected by
the queries above and below it.  If we removed all of the `min-width`
conditionals, these queries would cascade. A screen 400px wide would have _all_
of these queries apply.  

In some cases, this can actually be helpful, as it keeps our CSS clear of
duplicate code.  In the next section, we'll see examples of this.

## Setting up Content Focused Media Queries

Another approach to designing responsiveness worth considering is to  build
media queries that trigger based on _content_.  Instead of being concerned with
actual device screen sizes, we can test to see the exact screen widths when our
page starts to look weird, and create queries at those points.  Building this
way allows us to easily make a minimal amount of styling changes; just what is
necessary to make the site look good.

### Our First Media Query - Repositioning and Making Images Responsive

On our webpage, we've got some fixed, floating links on the right
side of the page for social media accounts.  Once the screen size shrinks too
much, these links will start to overlap onto our image and text content.  Even
before that, if the screen is less than around 1108px wide, they start to squish
up against the side of the content.  In addition to this, once the screen gets
just a little smaller, around 960px, our page content starts to go past the edge
of the screen, which will cause users to have to scroll horizontally, something
we want to avoid.

We can create one media query to address both of these. Remove the
previous media queries from `responsive.css`, and add the following in their
place:

```CSS
@media (max-width: 1108px) {
  /* All of our images and text are wrapped in the .wrapper class */
  /* Setting this will change the width property to a relative size,
  which will shrink to fit */
  .wrapper {
    width: 90%;
  }

  /* Our social media links are wrapped in a #social id */
  /* We're moving these links so they stay in one place, at the top right of the page */
  #social {
    position: absolute;
    top: 82px;
    right: 4%;
    width: auto;
  }

  /* We also tell each a child of #social to change their display property */
  #social a {
    display: inline-block;
  }
}
```

Now, take a look at our webpage, growing and shrinking the page. Whoa! On a
smaller screen, the social media links jump up to the header section of our page
and stay there even if you scroll. Meanwhile, our images shrink nicely while
maintaining a good size on the page.

### Media Query Two - Adjusting Type and our Logo

If you shrink the width of the page too far, though, the text in our navigation
bar starts to bunch and overlap.  Text further down the page also starts to
squish together and become difficult to read.  We can create another query to
fix this.  In this case, we probably want to set it to trigger _before_ the
text starts getting weird, so 900px will be a good breakpoint.  

Add the following query to `responsive.css` below our existing CSS:

```CSS

@media (max-width: 900px) {
  /* this will affect all text on the page */
  body {
    font-size: 85%;
  }

  #navbar nav a {
    font-size: 90%;
  }

  /* This is the round image on our logo */
  #logo h1 {
    width: 35px;
    height: 35px;
  }

  /* The text, Exceptional Realty, is inside of an h2, inside of #logo */
  #logo h2 {
    font-size: 2.2em;
  }

  #social {
    top: 72px;
  }

  .col-3 p {
    column-count: 2;
  }
}
```

Now, when our page shrinks below 900px, our fonts will shrink a bit, give us
more space and look a bit more in proportion with the content. Meanwhile,
our logo will resize - the image shrinks, the `h2` shrinks, and our social
links move up a little. Notice that when we set #social, we are inheriting our
previous property settings from the _last_ media query, so #social will still
have _absolute_ positioning.

The last block of CSS will affect the paragraphs that are currently in three
column sections (you can see this just below the first big image).
Setting column-count to 2 will keep our text from squishing together.

### Media Query Three - More Column Adjustments

There are still some issues though.  Further down the page, beside the video,
the Company News section is not in three columns, so it hasn't been affected.
Plus, if we continue to shrink our page, even the two column solution we've
written won't work.

Lets add another media query:

```CSS
@media (max-width: 750px) {
  /* here we are overriding three classes with the same properties.  Our images and paragraphs are all organized within these classes. This query will cause them to 'stack' on each other */
  .col-1, .col-2, .col-3 {
    width: 100%;
    margin-left: 0;
    float: none;
  }

  .col-3 p {
    column-count: 1;
  }
}
```

Save `responsive.css` and checkout the website again. Now, if our screen shrinks
below 750px, all of our images and paragraphs stack on top of each other, making
sections like Company News much nicer looking.  We also set our three column
section of paragraphs to become just one.

### Unexpected Results

The changes we've made have had some unexpected effects.  Our footer section now
breaks down when we shrink down past 750px wide; the text starts spilling over
and a rogue line appears on the right side.  We can take care of these by adding
a few more blocks to the last media query:

```CSS
  /* Removes the rogue border line on the right side of our footer */
  .border-right {
    border: 0;
  }

  /* Setting height to auto here will let our footer divs stretch to fit the size of
  the content inside of them.  Adding padding-bottom, will give them a little
  space in between when stacked */
  #details div {
    height: auto;
    padding-bottom: 20px;
  }
```

Great! At 750px, our footer content now stacks nicely.  Not only that, our site
content looks pretty good at all sizes! There are very few mobile devices that
have a smaller width than 320px, so it is okay for us to ignore what things look
like when we shrink our width any smaller.

We've still got one glaring issue left: our navigation bar is still squishing
together on smaller widths.

### Reorganizing Navigation

For a small screen, our navigation bar currently has too many things to fit in an
aesthetic way.  We need some way to _collapse_ our links without making them
inaccessible.  Often, web designers will choose to replace text with icons, but
in our case, our nav links are easy to convert to image references.  Another
common option is to use the _hamburger button_. You've likely seen the hamburger
button before on other sites:

![hamburger icon](https://s3.amazonaws.com/ironboard-learn/hamburger_icon.png)

When navigation becomes too small, the navigation links are removed from
immediate view, replaced with one button that will display our links when
clicked.

We've already got some HTML provided to get us started.  In the navigation section of our HTML,
just after the H.U.D. link, we have:

```html
<a class="menu-icon">&#9776;</a>
```

This is the ASCII character code for the hamburger button.  So, it is already
sitting in our HTML, but _we don't see it on the page currently_. This is
because in `style.css`, we have the following:

```css
#navbar nav a.menu-icon {
  display: none;
}
```

Our little hamburger is automatically invisible, while all of our other
navigation links are set to `display: inline-block`.  We want to write a media
query that _flips_ these, hiding all the currently visible links, and displaying
just the one hamburger button.

We'll make one more media query to handle this:

```CSS
@media (max-width: 700px) {
  #navbar nav a {
    display: none;
  }

  #navbar nav a.menu-icon {
    display: block;
    float: right;
    font-size: 1.5em;
    padding: 10px 20px;
    width: auto;
  }

}
```

Save this and check out the site.  _Now_, just before our navigation links start
wrapping and causing display issues, the entire set of links is replaced by one
hamburger.

This link doesn't work at the moment, and won't work just yet - in order to get
this button working as we expect, we're going to need to incorporate some
JavaScript, listen for when the button is clicked and have a new nav menu appear
on the scren, which is beyond the scope of this code along.  

This is great progress though! We've turned a desktop only website into one that
will look good regardless of screen size. We _could_ go even further, and if you
are inclined, you could continue to work on additional details, such as fixing
the #social links so they won't overlap our logo on small screens. A lot has
been accomplished though, so let's quickly summarize some of thinsg we've
encountered.

## Summary

* Using media queries, we can alter web page content to look great on any screen
size
* We have to choose how granular we want to go in customizing our queries. Often,
having a few is the easiest in terms of maintenance
* Each query can build off the last, inheriting properties, so later queries
can focus on just what needs to be adjusted and now repeat a bunch of code
* CSS can become very complex, and it is possible that a change meant to fix
one thing may cause unexpected issues elsewhere.  

Remember: Always thoroughly test your web pages when building for responsiveness!

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/adding-responsive-features' title='Responsive Features Code-Along'>Responsive Features Code-Along</a> on Learn.co and start learning to code for free.</p>
