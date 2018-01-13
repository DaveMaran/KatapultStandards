# Katapult Coding Standards

### This document has been produced as a guideline for all digital projects using HTML, CSS and JS.
This is a living document and should be updated to include more tools or information when needed.

For any queries please email pete@katapult.co.uk

---

# HTML Styling
Styling guidelines that should be followed for all HTML code produced for any Katapult digital project.
This is a living document and should be updated where and when necessary to keep it inline with standards required.

#### Formatting and whitespace

- Lowercase tag name
- Lowercase attribute name
- Use double-quotes for attribute values (not single-quotes)
```
<!-- Good -->
<img src="#">

<!-- Bad -->
<IMG SRC='#'>
```

### Attribute order

Use the order of attributes to improve the developer experience (the browser doesn’t care). Simple rules:
- Put class attribute first (developers read/write this most often).
- Put id attribute second (important if it’s defined).
- Everything else is alphabetical.
- Put data-* attributes last (alphabetical).
```
<div class="" id="" onclick="" data-abc="" data-xyz="">
<form class="" id="" action="" method="">
```

### Exceptions

When an attribute is critical to defining an element put it first to improve readability:
```
<!-- The "type" defines the element's behavior. -->
<button type="" class=""></button>
```
```
<!-- Put "src" before "alt". -->
<img class="" src="" alt="">
```
```
<!-- The "type" defines the element's rendering. -->
<input type="text" class="" id="" disabled name="" placeholder="" readonly value="">
```

### Attribute overload

When an element has too many attributes it’s hard to read. Break attributes onto lines:
```
<button
type="button"
class="button"
data-loading="Loading..."
data-x="123"
data-y="789"
>
    Send Coordinates
</button>
```

Some people probably hate that the attributes aren’t indented. I think it’s easier to read the DOM structure with them outdented and the closing ``` >```  on its own line.

### Indentation

Use 4 spaces (not tabs) for indentation.
```
<ul>
••••<li>Item<li>
<ul>
```

### Void Elements

Don’t use a trailing slash anymore. Yeya, HTML5!
```
<!-- Good -->
<br>
<hr>
<img>
<input>
<link>
<meta>

<!-- Bad -->
<br />
<hr />
<img />
<input />
<link />
<meta />
```

### Optional Tags
Optional tags should always be closed for readability — even if not strictly required by the HTML spec.
```
<!-- Good -->
<ul>
    <li>One</li>
    <li>Two</li>
</ul>

<!-- Bad -->
<ul>
    <li>One
    <li>Two
</ul>
```

### Comments

Don’t use HTML comments.
```
<!-- I don't need to be here -->
<p>Lorem ipsum dolor sit amet.</p>
```

##### Exception
Where HTML code is filtered out during the build process and is not visible on distribution versions. Most likely to be used in base HTML builds not incorporating a CMS.

Why not?
- With modern developer tools, the need to “View Source” is rare today.
- Developer comments should be secret (HTML comments are visible in the source).
- Don’t slow down users by making them download invisible comments.

##### Suggestion
Comments in HTML should always wrapped in backend/templating language, or stripped out during HTML minification. Example:
```
<?php
// GravDept:
// See: Issue 12345
?>
<p>Lorem ipsum dolor sit amet.</p>
```

### Nesting
Markup has a hierarchy, which creates parent/child relationships between elements. Maintaining an accurate hierarchy is essential for readability and avoiding validation issues.

#### Nest or not?

```
Write elements without attributes on one line:
<p>The quick brown fox jumps.</p>
```

Write elements with attributes nested:
```
<p class="lead">
    The quick brown fox jumps.
</p>
```

Write elements with multi-line content nested:
```
<p>
    The quick brown fox jumps.
    Do or do not — there is no try.
</p>
```

#### Single-child nesting
- Child elements may be most readable without whitespace. Example:
- Each line is under 80 characters.
- Each line is nested less than two levels deeper.
```
<ul>
    <li>One</li>
    <li>Two</li>
    <li>Three</li>
</ul>
```

A good example of multi-level HTML that still benefits from single-line nesting is anchors within lists:
```
<ul>
    <li><a href="#">One</a></li>
    <li><a href="#">Two</a></li>
    <li><a href="#">Three</a></li>
</ul>
```

#### Multi-child nesting
Use a single blank line to break up complex hierarchies. Separate child elements with a single blank line.
```
<ul>
    <li>
        <h2>Title</h2>
        <p>Lorem ipsum dolor.</p>
    </li>

    <li>
        <h2>Title</h2>
        <p>Lorem ipsum dolor.</p>
    </li>

    <li>
        <h2>Title</h2>
        <p>Lorem ipsum dolor.</p>
    </li>
</ul>
```

#### Hybrid nesting
In practice, single-line and multi-line syntaxes are both used. The goal is optimum readability for the given content. This is more important than strictly following a single syntax.
```
<ul>
    <li>
        <h2>Title</h2>
        <p>Lorem ipsum dolor sit amet.</p>
    </li>

    <li>
        <h2>Title</h2>

        <p class="lede">
            Lorem ipsum dolor sit amet.
        </p>

        <p>
            Nulla facilisi. Duis aliquet egestas purus in blandit.
            Curabitur vulputate, ligula lacinia scelerisque tempor,
            lacus lacus ornare ante, ac egestas est urna sit amet arcu.
        </p>
    </li>

    <li>
        <h2>Title</h2>
        <p>Lorem ipsum dolor sit amet.</p>
    </li>
</ul>
```

### Grouping elements
Complex markup can take hundreds of lines to describe relatively simple content. From an authoring perspective, this can be difficult to scan and read. Adding several line-breaks between large sets of markup improves readability for developers. Whitespace characters are collapsed in HTML so there is no visual output, and the additional bytes are negligible.

### Semantics
Above all, remember that HTML is a markup language. The elements we use to describe content infer hierarchical and descriptive meaning. These inferences should not be taken lightly because this metadata is read and understood by search engines. Using structured markup accurately and consistently helps man and machine accessing the site use it better.

Use HTML5 elements where appropriate:
```
<article>
<aside>
<figure>
<figcaption>
<header>
<footer>
<main>
<nav>
<section>
```

However, like all generic elements they should be qualified with a class/ID to define their role or context. Don’t use rely on the parent/child relationship in selector chains to style them.
```
<!-- Bad -->
<div class="page-header">
    ...
</div>

<!-- Good -->
<header class="page-header">
    ...
</header>
```

### Document outline
The document hierarchy follows the HTML5 outlining algorithm. The heading outline (```<h1> – <h6>```) does not need to be hierarchical globally. HTML5 sectioning elements are used to new outlining contexts.
Within each sectioning context, the appropriate heading hierarchy should be used for clarity (although this has no impact on the outline algorithm.

##### Traditional HTML
Traditional HTML authoring has a strict reliance on creating a universal document hierarchy. This is often difficult to maintain if content or modules are used in multiple contexts. This is not recommended today.
```
<div class="primary">
    <h1>Page Title</h1>

    <div class="article">
        <h2>Article Title</h2>
        <p>Llorem ipsum dolor sit amet.</p>

        <h3>Article Subtitle</h3>
        <p>Curabitur vulputate, ligula lacinia scelerisque tempor.</p>

        <h3>Article Subtitle</h3>
        <p>Curabitur vulputate, ligula lacinia scelerisque tempor.</p>
    </div>

    <div class="article">
        <h2>Article Title</h2>
        <p>Nulla facilisi. Duis aliquet egestas purus in blandit.</p>
    </div>
</div>

<div class="sidebar">
    <h2>Related Articles</h2>
    ...
</div>
```

### HTML5 with sectioning
In HTML5 documents, the heading hierarchy may be restarted within any element that creates a new sectioning context. The document outlining model in HTML5 combines the heading levels and sectioning hierarchy to assemble the document hierarchy.

Technically, you could use only ```<h1>``` elements if you wanted to, but this makes styling more difficult so it’s not recommended. If a section of content merits its own hierarchy, then use a sectioning element.
Never choose a heading level based on the global styling applied to that element though. There are conventions in CSS to swap the styling of another heading level consistently and in a maintainable way.
```
<div class="primary">
    <h1>Page Title</h1>

    <article>
        <h1>Article Title</h1>
        <p>Llorem ipsum dolor sit amet.</p>

        <h2>Article Subtitle</h2>
        <p>Curabitur vulputate, ligula lacinia scelerisque tempor.</p>

        <h2>Article Subtitle</h2>
        <p>Curabitur vulputate, ligula lacinia scelerisque tempor.</p>
    </article>

    <article>
        <h1>Article Title</h1>
        <p>Nulla facilisi. Duis aliquet egestas purus in blandit.</p>
    </article>
</div>

<aside>
    <h1>Related Articles</h1>
    ...
</aside>
```

### Conventions

#### Using classes
- Classes have no effect on semantics. They do affect extensibility.
- Classes can be stacked (and should). Take advantage of this modularity.
- Classes are case sensitive. Only use lowercase letters for consistency.
- Class-based CSS selectors are faster than element-based selectors.
```
<button class="button button--small">
    Add To Cart
</button>
```

- Using IDs
- IDs as #hash links
- Hash-linking is probably the only way IDs should be used today.
```
<a href="#content">
    Skip to content
</a>
```

#### IDs in CSS
Never use IDs for CSS styling because:
IDs must be unique per document. They’re not suitable for styling because they bind markup:CSS in a 1:1 relationship.
IDs add 100 to CSS specificity (classes add 10 specificity) so they blow away virtually all other styling.

#### Snippets

HTML Starting point
```
<!doctype html>
<html lang="en-us">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="x-ua-compatible" content="ie=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title></title>
        <link rel="stylesheet" href="css/app.css">
    </head>

    <body>
        <!--[if lte IE 9]>
            <div class="browser-warning">You’re using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience and security.</div>
        <![endif]-->

        <h1>Hello World</h1>
    </body>
</html>
```
---

# CSS
### CSS Principles

##### Syntax
- Use soft tabs with four spaces.
- When grouping selectors, keep individual selectors to a single line.
- Include one space before the opening brace of declaration blocks for legibility.
- Place closing braces of declaration blocks on a new line.
- Include one space after : for each declaration.
- Each declaration should appear on its own line for more accurate error reporting.
- End all declarations with a semi-colon. The last declaration's is optional, but your code is more error prone without it.
- Avoid specifying units for zero values, e.g., margin: 0; instead of margin: 0px;.
```
/* Bad CSS */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  box-shadow:0px 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}
```
```
/* Good CSS */
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin-bottom: 15px;
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```
##### Declaration order
Order and grouping of property declarations is not of paramount importance but, ideally, you would follow the order [specified in SMACSS](https://smacss.com/book/formatting#grouping):
- Box
- Border
- Background
- Text
- Other

##### Compile multiple files with SASS
Generally speaking you should compile your CSS into a single file with Sass. If there's a good reason to use multiple .css files then use multiple ```<link>``` elements rather than ```@import```. 

##### Media query placement
Place media queries near to their relevant rule sets. Don't place them in a separate stylesheet or at the end of the document. Example:
```
.element { ... }
.element-avatar { ... }
.element-selected { ... }

@media (min-width: 480px) {
  .element { ...}
  .element-avatar { ... }
  .element-selected { ... }
}
```

##### Shorthand notation
Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values. Common overused shorthand properties include:
- padding
- margin
- font
- background
- border
- border-radius

Often times we don't need to set all the values a shorthand property represents. For example, HTML headings only set top and bottom margin, so when necessary, only override those two values. Excessive use of shorthand properties often leads to sloppier code with unnecessary overrides and unintended side effects.
```
/* Bad example */
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

/* Good example */
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```
##### Nesting in Sass
Avoid unnecessary nesting. Just because you can nest, doesn't mean you always should. Consider nesting only if you must scope styles to a parent and if there are multiple elements to be nested.
```
// Without nesting
.table > thead > tr > th { … }
.table > thead > tr > td { … }

// With nesting
.table > thead > tr {
  > th { … }
  > td { … }
}
```

##### Comments
To help with maintaining and updating code please ensure your code is descriptive and well commented. Great code comments convey context or purpose. Be sure to write in complete sentences for larger comments and succinct phrases for general notes.
```
/* Bad example */
/* Modal header */
.modal-header {
  ...
}

/* Good example */
/* Wrapping element for .modal-title and .modal-close */
.modal-header {
  ...
}
```

##### Class names
- Keep classes lowercase, use dashes and underscores using either the SMACSS or BEM methodology. 
- Don't oversimplify. ```.btn``` is fine, but ```.b``` doesn't mean anything.
- Use meaningful names; use structural or purposeful names over presentational.
- Use ```.js-*``` classes to denote behavior (as opposed to style), but keep these classes out of your CSS.
```
/* Bad example */
.t { ... }
.red { ... }
.header { ... }

/* Good example */
.tweet { ... }
.important { ... }
.tweet-header { ... }
```

##### Organisation through 
We design in a modular way in which functional parts (we call them **components**) of the site are reused many times on different page templates. To help with this approach it is useful to create a separate CSS file for each component and combine these with SASS.    

##### BEM
BEM (Block, Element, Modifier) from Yandex allows developers to create a simple naming convention helping make your CSS more modular and portable.
```
.block{} // the ‘thing’ like .list
.block__element{} // a child of the block like .list__item 
.block--modifier{} // a variation of the ‘thing’ like .list-—vertical
Further Reading
```
##### BEM Explained 
[Find out about BEM](http://getbem.com/introduction/)
[How to use BEM with SASS](http://alwaystwisted.com/articles/2014-02-27-even-easier-bem-ing-with-sass-33)

##### CSS Tools

##### SASS

[Available from Sass-lang](http://sass-lang.com/)

- SASS to be used with extension .scss.
- All code should be split into components and placed inside the /scss/components folder.

[SASS guidelines can be read](https://sass-guidelin.es/)

##### Normalize
Should be used to make all browsers render elements more consistently and in line with modern standards.
[Find out about Normalize](https://necolas.github.io/normalize.css/)

##### CSS Frameworks
Framework of use is [Foundation 6](http://foundation.zurb.com/)
[Documentation available here](http://foundation.zurb.com/sites/docs/)
```Bower.json code :    "foundation-sites": "~6.3.0"```

---

# JavaScript

#### Frameworks
[Base Library - jQuery](https://jquery.com/)
[Animation Library - GreenSockJS](https://greensock.com/)

#### JavaScript Tools

Fall backs, polyfills or shims are to be confirmed.
A large array can be found [here](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills)

---

# General standards
### Media
##### Icons
SVG should be used where possible for all Icons, PNG fallback should be also used.
[Use Mondernizr](https://modernizr.com/)

Example code
```
.tomato {
    background-image: url('img/tomato.svg');
}
.no-svg .tomato {
    background-image: url('img/tomato.png');
}
```

##### Responsive Images
Use srcset to serve up a single image in 4 dimensions. 
- 400px
- 800px
- 1200px
- 1600px

Other sizes can be used, however these are the recommended ones.
Displays with a high Pixel Density will use whichever image is best for the size and density.
Make sure a src is still set as a fallback in case srcset is not supported. 

##### Performance
Measuring project speed
Used GTMetrix.com for the main website speed test.

##### Tooling
###### Task runners
Gulp or Grunt task runner can be used. All code and commands must be documented with simple installation and run commands. 

[Setup Gulp](https://github.com/gulpjs/gulp)
[Setup Grunt](https://gruntjs.com/)

###### Dependency Manager
All code libraries and frameworks for front-end code should be pull into the repository via a dependency manager. NPM is useable, Bower.io is the recommended manager.
[Setup Bower](https://bower.io/)

###### Linting
JS linting should make use of http://jslint.com/
Or the bower package [JsLint](https://github.com/sasidhar/bower-jslint)

CSS linting should make use of http://csslint.net/
Or the bower package [csslint](https://www.npmjs.com/package/csslint)

##### Specific frameworks

###### Wordpress
For all Wordpress websites, [JOINTSWP](http://jointswp.com/) should be used as a base. This includes all package management tools and dependencies.

Acceptable third-party Wordpress plugins to use are:
* [Advanced Custom Fields](https://www.advancedcustomfields.com/)
* [Formidable Forms](https://formidableforms.com/)
* [Hubspot Tracking Code](https://en-gb.wordpress.org/plugins/hubspot-tracking-code/)
* [Redirection](https://redirection.me/)
* [Smush Image Compression](https://wordpress.org/plugins/wp-smushit/)
* [Wordfence](https://www.wordfence.com/)

Any third-party plugin not on this list must be verified and added before use in a client website.

###### HTML
[FoundationTemplate](https://github.com/zurb/foundation-zurb-template/) should be used as a base for all systems other than Wordpress or sites not using a CMS.


##### Version control
Github will be used for all projects using version control.

Setting up Github correctly for the development process is important for both ease of deployment and management between multiple members of staff/ development team.

Github should be used for all project required code and not minified/ optimised assets. This should be completed within the build process for deployment. 

###### Master branch
This branch will be the location of all production ready code. All testing and staging environment must have been completed, before anything is placed into this branch.  A pull request must be created for any code merging from ‘staging’ to the Master branch.

###### Development branch
This branch is where all development work should be placed. All new branches produced should be merged into this area once completed. This allows the development team to work from one up-to-date code bank. 

Multiple branches can be used for development, if used they must be merged back to the development branch before being pulled into the staging/testing branch.

###### Staging/Testing branch
This branch is for testing and checking that all code runs in the staging environment correctly before being pushed to the master branch. Any code issues found within this testing phase must be placed as an issue in GitHub.

##### Deployment/Integration ([DeployBot](http://deploybot.com))
With the use of DeployBot, all code placed into the development branch will be sent to the development server via SSH automatically. This allows any code changes made in the development branch to be seen online by other members of staff.

An automatic deployment script shall be used to deploy the staging/testing branches to their environments as this would be dependant on the client and the environment they are currently using. This will be done either automatically or manual depending on the server details and access put in place. 

All deployments pushed to the live environment from the master branch will be done manually only. This should stop any errors or unrequired deployments to occur. 

---

### Produced by Pete Clark - Katapult (Last updated 13/01/2018)
