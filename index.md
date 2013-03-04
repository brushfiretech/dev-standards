---
layout: default
date: 03/03/2013
---

# Introduction

### Foreword

Whenever two or more people work on anything, a centralized, consistent set of language and methods is necessary to increase productivity and decrease confusion. This is especially true of software development. The practices outlined in this document are an attempt to create a consistent set of standards based upon years of experience and commonly agreed-upon best practices.

>The best way to learn good practices is to run into the problems that spawned them in the first place and internalize the solution. Code that implement patterns from a checklist and not experience is often worse than code that doesn't use patterns in the first place. – [Phil](https://twitter.com/haacked/status/20227608300) [Haack](https://twitter.com/haacked/status/20227672330)

### General Document Guidelines

* All indentation white space should be made with [__tabs and not spaces__](http://lea.verou.me/2012/01/why-tabs-are-clearly-superior/).
* Tab length can be set to any number of spaces in your personal editor that you prefer. Two or four spaces are encouraged.
* All trailing spaces at the end of lines should be removed.
* File names should contain no spaces.

# Front End (Client Side)

## Browsers

It is not important (or even possible) for our websites to appear _identically_ in every browser. Our purpose is to serve a functional site to all users of supported browsers by delivering content that is valuable to them. To that end, the site should function _similarly_ in all supported browsers and not appear broken.

### We currently support:

Internet Explorer 7+, and the latest versions of Firefox, Chrome, Safari, and Opera. Testing for all these browsers __must__ be done prior to code going into production. Install the browsers yourself or fire up a Virtual Machine.

## HTML

### Semantics

>Semantics (from Greek: sēmantikós) is the study of meaning. It focuses on the relation between signifiers, like words, phrases, signs, and symbols, and what they stand for. – [Wikipedia](http://en.wikipedia.org/wiki/Semantics)

HTML gives meaning to content so that browsers and devices can then give that meaning to a user. Therefore use the correct element(s) to define the appropriate type of information you are presenting (e.g, if it's a heading, use a `h1`, `h2`, `h3`, etc. If it is a list, use a `ul` or `ol`). This is also a key component to creating an accessible site.

<aside>
Tables are the most common offender of semantic web designs. Tables should __only__ be used for tabular data where rows and columns have clearly and traditionally defined roles and representation. Tables should __never__ be used to lay out content. Never.
</aside>

### Formatting

* All tags should have a matching closing tag or be self closing.
* All tags should be lower case
* All attributes should be enclosed in double quotes.

### DOCTYPE

A doctype is necessary to force browsers to render in standards mode and prevent quirks mode headaches in IE.  The "HTML5 doctype" should be used in __all__ documents.

```html
<!doctype html>
```

### Naming conventions

Id and Class names should be lower case and hyphenated and not limit or specify use within the document structure because that structure may change. Therefore:

* Id's should __define what the object is and does__ rather than where it appears on the page or in the DOM hierarchy. (e.g. `nav1` rather than `top-nav`)
* A class name should __describe the behavior that it defines__ rather than where it appears on the page or in the DOM hierarchy. (eg. `news-block` rather than `bottom-info`)

Id’s should be assigned on a “general to specific” basis. e.g., `billing-name-first` shows that this is a field in the domain of the billing address, and then that this is part of the name element in that address, and of that name, this is the first name portion (to differentiate from `shipping-name-first` or `billing-name-last`).

## CSS

CSS 3.0 can and should be used as long as all functionality degrades gracefully to all targeted browsers so that there may be a degradation in the _quality_ of the experience but all required functionality will still exist.

Inline styles should be avoided in favor of class declarations except when it is not possible or pragmatic to do so.  Class declarations should be made in external files.

### Naming conventions

Id and Class names should be lower case and hyphenated and not limit or specify use within the document structure because that structure may change. Therefore:

* Id's should __define what the object is and does__ rather than where it appears on the page or in the DOM hierarchy. (e.g. `nav1` rather than `top-nav`)
* A class name should __describe the behavior that it defines__ rather than where it appears on the page or in the DOM hierarchy. (eg. `news-block` rather than `bottom-info`)

Style declarations should always be made at the lowest level of specificity required for consistent use. That is, a generic style should never be applied to solve a specific problem. (e.g., avoid using `div#searchbox input {font-size:larger;}` to adjust the style of a specific input control. Instead, reference that control by ID or assign a class to that control specifically. (e.g., `.search-field {font-size:larger;}`)

Classes are encouraged over Id's. Due to the nature of a large-scale application, it is often difficult to know if an object may be re-used, but it should be crafted such that it can be.

### Syntax

Don't use `!important`. It's lazy. All CSS is important and well-written CSS cascades so it works as desired.

All style attribute declarations should be followed by a semi-colon.

All colors should be specified in HEX, RGB, or HSL. Fixed color names such as `white` or `black` should be avoided.

Shorthand notation should be used whenever possible:

```css
.class {margin: 10px 0; color:#333;}

/* instead of */

.class {margin-top:10px; margin-right:0px; margin-bottom:10px; margin-left:0px; color:#333333; }

```

#### Sizing and Units

`em` and `%` are preferred over `px` and `pt` when feasible.

Units are not necessary for "zero". Zero is zero in any unit.

Units should always be provided for non-zero units due to issues with downlevel browsers (e.g. `margin: 10px 0px auto 10px;` instead of `margin: 10 0 auto 0;`)

Fixed dimensions should __not__ be specified unless absolutely necessary. Use padding/margin values that ensure that the affected elements will still render as preferred regardless of the nature of dynamic content or device size.

Box-sizing should __always__ be set to `border-box`.

#### Viewport

For all responsive designs, the following viewport should be set:

```html
<meta name="viewport" content="initial-scale=1.0, width=device-width">
```

### Browser-Specific Considerations

#### Prevent IE from switching to unhelpful modes

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
```

Rather than using specific style sheets for IE version-specific situations (e.g., `style-ie.css`) assign conditional classes to the root html classes:

```html
<!--[if lt IE 7 ]> <html class="ie6"> <![endif]-->
<!--[if IE 7 ]>    <html class="ie7"> <![endif]-->
<!--[if IE 8 ]>    <html class="ie8"> <![endif]-->
<!--[if (gte IE 9)|!(IE)]><!--> <html> <!--<![endif]-->
```

then a specific version can be targeted with a class like

```css
.ie6 #myElement { float: left; width: 100% }
```

Giving an element "Layout" will fix 99% of IE rendering bugs. The other 1% will most likely be related to position: relative; or floats. Use "zoom: 1" as a trigger for whatever IE versions need it.

```css
.ie6 #myElement, .ie7 #myElement { zoom: 1 }
```

Use all vendor prefixes in alphabetical order with the prefixless version last.

```css
#slides img:hover {
  -moz-transform: scale(1.6);
  -ms-transform: scale(1.6);
  -o-transform: scale(1.6);
  -webkit-transform: scale(1.6);
  transform: scale(1.6);
}
```

## JavaScript

The goal of JavaScript should be to progressively enhance the user experience. Ideally the site will still function without JavaScript, but in large-scale applications that is becoming increasingly difficult to accomplish efficiently.  In fact, more and more applications are being entirely written in JavaScript and all modern browsers default to having scripting turned on.  Therefore, in all applications we can assume that the user has JavaScript enabled, but we should gracefully degrade with at least a message informing the user that they must have JavaScript enabled to use the site if they do not have it turned on.

However, we _cannot_ assume a browser has a specific set of functionalities or capabilities. All capabilities _must_ be tested for using libraries such as [Modernizr](http://modernizr.com). If we require functionality that a certain browser does not have, a cross-browser shim should be put in place.

### Syntax

Avoid inline event handlers. The clutter up markup and are the scripting equivalent of inline CSS styles. Only use them when it is absolutely necessary.



## Accessibility

Since there is no way to fully determine what every user of our software will see or experience due to handicaps or other circumstances, it should be our goal to make the experience as accessible as possible for the common tools to help those. As such, we should always, at least:

* use semantic HTML
* use alt text on all images
* use title attributes on all links
* test our sites using a screen reader

# Back End (Server Side)

## C-Sharp

Private member declarations should be declared with a leading underscore and camel-case.

```c#
private string _firstName;

private void _logMessage(string message) { ... }
```

Public member declarations should be declared with Pascal-Case.

```c#
public string FirstName {get; set;}

public void LogMessage(string message) { ... }
```

Local variables (function-scope) should be declared with camel-case.

```c#
void LogMessage(string message)
{
	DateTime todaysDate = DateTime.Today;
	string messageType = "debug";
	...
}
```

Unless the entire function/control statement body can fit on one line, opening and closing curly bracess should appear on their own line at the same indent as the parent function/control statement body. The enclosed body should be indented one tab.

```c#
// OK
string _smallFunction(string simpleParameter) { return simpleParameter; }

// also OK
string _longerFunction(string complexParameter)
{
	var variable1 = "";
	var variable2 = true;

	if (variable2)
	{
		variable1 = "variable 2 is true";
	}
}
```

## SQL

Entity names (both tables and fields) should be Pascal-cased.

Table names should be pluralized and avoid reserved words whenever possible.

Field names should be singular and avoid reserved words whenever possible.

All tables should contain at least one primary key field. That field should be the singularized form of the table name followed by "Id" (e.g.for the table `Orders` the primary key would be `OrderId`.)

Many-to-many tables should be named with the two table names concatenated together (e.g. `Prices` and `Sections` would be joined on `PricesSections`) unless it is commonly understood to more closely correlate to a parent-detail relationship (e.g. `OrderDetails`).

## 3rd Party Tools

Use of 3rd party tools is encouraged when it is not feasible or smart to develop the functionality ourselves. 3rd party tools generally fall into two categories:

Frameworks are not evil, they save time, fix cross browser compatibility issues and make us think in new ways. But, they do add overhead so before we jump in bed with them:

>If you want to use a library you must have read it, understood it, agree with it, and not be able to write a better one on your best day of coding. - @sentience

* Commercial - These tools are provided for a cost from some 3rd party. As long as there are avenues available to contact support quickly they're good.

* Open Source - These tools must be in active development and support and well documented.

All production applications at Brushfire will automatically bundle and minify static resources using server-side processes. Because of this, it is recommended to never touch 3rd party CSS or JS files if at all possible. For CSS simply, include a set of overrides in a secondary file that will be bundled and served after the original file. For javascript, if a plugin architecture exists use it, or prototype functionality on to the object prototype.

If it is not feasible or wise to do one of the above, then the following methods are suggested:

```javascript

... existing code ...

// BRUSHFIRE START - {YOUR INITIALS} - {TODAY'S DATE}

... modified code ...

// BRUSHFIRE END

... existing code ...

```

and

```css

... existing code ...

/* BRUSHFIRE START - {YOUR INITIALS} - {TODAY'S DATE} */

... modified code ...

/* BRUSHFIRE END */

... existing code ...

```

This is so that when an updated version of the file is released from the 3rd party, you can replace the entire file and not overwrite your code.  All code should be searched for the above comment syntax before it is replaced with a newer version.

# Resources

* [Can I Use](http://caniuse.com)
* [Twitter Bootstrap](http://twitter.github.com/bootstrap)
* [Modernizr](http://modernizr.com)
* [Scalable and Modular CSS](http://smacss.com)