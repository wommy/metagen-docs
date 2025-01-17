---
title: Eleventy Plugin Metagen
date: 2021-09-06
npm_link: https://www.npmjs.com/package/eleventy-plugin-metagen
layout: main.njk
templateEngineOverride: njk, md
---

# {{ title }}

eleventy-plugin-metagen is a `<meta>` tag generator for sites built with [Eleventy](https://11ty.dev). Generate meta tags for [Open Graph](https://ogp.me/) and [Twitter](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/markup) along with some other useful tags. All of the tags necessary for a baseline social share functionality are included with this plugin along with some additional tags for more custom use cases.

<nav>
  <details>
    <summary>Table of Contents</summary>
      <ul class="toc">
        <li><a href="#installation">Installation</a></li>
        <li><a href="#what_does_it_do?">What does it do?</a></li>
        <li><a href="#custom_usage">Custom usage</a></li>
        <li><a href="#shortcode_default_tags">Shortcode Default Tags</a></li>
        <li><a href="#shorthand_nunjucks_usage">Shorthand Nunjucks Usage</a></li>
        <li><a href="#use_your_template_data">Use Your Template Data</a></li>
        <li><a href="#liquid_usage">Liquid usage</a></li>
        <li><a href="#shortcode_parameters">Shortcode Parameters</a></li>
      </ul>
  </details>
</nav>

## Installation
In your Eleventy project, [install the plugin](https://www.npmjs.com/package/eleventy-plugin-metagen) from npm:

```js
npm install eleventy-plugin-metagen
```

Then add it to your [Eleventy Config](https://www.11ty.dev/docs/config/) file:

{% raw %}
```js
const metagen = require('eleventy-plugin-metagen');

module.exports = (eleventyConfig) => {
    eleventyConfig.addPlugin(metagen);
};
```
{% endraw %}

## What does it do?
The plugin turns [11ty shortcodes](https://www.11ty.dev/docs/shortcodes/) like this:

{% raw %}

```liquid
{% metagen
    title="Eleventy Plugin Meta Generator",
    desc="An eleventy shortcode for generating meta tags.",
    url="https://tannerdolby.com",
    img="https://tannerdolby.com/images/arch-spiral-large.jpg",
    img_alt="Archimedean Spiral",
    twitter_card_type="summary_large_image",
    twitter_handle="tannerdolby",
    name="Tanner Dolby",
    comments=true
%}
```
{% endraw %}
into `<meta>` tags and other document metadata like this:

{% raw %}
```html
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Eleventy Plugin Meta Generator</title>
<meta name="author" content="Tanner Dolby">
<meta name="description" content="An eleventy shortcode for generating meta tags.">
<meta name="generator" content="eleventy">
<!-- Open Graph -->
<meta property="og:type" content="website">
<meta property="og:locale" content="en_US">
<meta property="og:title" content="Eleventy Plugin Meta Generator">
<meta property="og:description" content="An eleventy shortcode for generating meta tags.">
<meta property="og:url" content="https://tannerdolby.com">
<meta property="og:image" content="https://tannerdolby.com/images/arch-spiral-large.jpg">
<meta property="og:image:alt" content="Archimedean Spiral">
<!-- Twitter -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@tannerdolby">
<meta name="twitter:creator" content="@tannerdolby">
<meta name="twitter:url" content="https://tannerdolby.com">
<meta name="twitter:title" content="Eleventy Plugin Meta Generator">
<meta name="twitter:description" content="An eleventy shortcode for generating meta tags.">
<meta name="twitter:image" content="https://tannerdolby.com/images/arch-spiral-large.jpg">
<meta name="twitter:image:alt" content="Archimedean Spiral">
<link rel="canonical" href="https://tannerdolby.com">
```
{% endraw %}

## Custom Usage
For a baseline social share functionality, providing all of the comma separated arguments to `metagen` shown in the example usage above is recommended. If you want to add more meta tags that are not listed in the example, see [parameters](#shortcode_parameters) for supported tags. You might only need a few `<meta>` tags instead of the whole set, simply provide the arguments you need and the ones not included won't generate `<meta>` tags.

Besides the default generated `<meta>` tags, only the arguments you provide data for will be generated as `<meta>` tags. This allows you to include some of your own tags alongside `metagen` if you need. Template variables can be used in the Nunjucks and Liquid shortcode arguments without the curly braces or quotes like `title=page.url` (Nunjucks) or `title` (Liquid). More on template variable usage [below](#use_your_template_data)

## Shortcode Default Tags

If atleast one parameter is provided to `metagen`:

{% raw %}
```liquid
{% metagen 
  comments=true
%}
```
{% endraw %}

the default `<meta>` tags aside from the main Open Graph and Twitter card data are:

{% raw %}
```html
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<!-- Open Graph -->
<meta property="og:type" content="website">
<meta property="og:locale" content="en_US">
<!-- Twitter -->
<meta name="twitter:card" content="summary">
```
{% endraw %}

The `title` parameter provides data for `<title>`. If `title` is not defined within `metagen`, the `<title>` element will not be generated with the above default tags. The same rules apply for `name` and `desc`. All other arguments if omitted, will not have meta tags generated.

Using {% raw %}`{% metagen %}`{% endraw %} without any arguments will throw `Error: No data was added into the meta generator` and return an empty string.

## Shorthand Nunjucks Usage

To use the shortcode as a one-liner. You can define an object in frontmatter, global data or another data source with the required key/value pairs and supply the shortcode with a single object parameter. 

{% raw %}
```liquid
---
data: 
    title: Eleventy Plugin Add Meta Tags
    desc: An eleventy shortcode for generating meta tags.
    url: https://tannerdolby.com
    img: https://tannerdolby.com/images/arch-spiral-large.jpg
    img_alt: Archimedean Spiral
    twitter_card_type: summary_large_image
    twitter_handle: tannerdolby
    name: Tanner Dolby
---
<head>
  {% metagen data %}
</head>
```
{% endraw %}

## Use Your Template Data
To make your metadata dynamic, you can use template data as arguments to the shortcode, without quotes or braces:

{% raw %}
```liquid
---
title: Some title
desc: Some description
metadata:
  title: Some other title
  desc: Some other description
url: https://tannerdolby.com
image: https://tannerdolby.com/images/arch-spiral-large.jpg
alt: Archimedean spiral
type: summary_large_image 
twitter: tannerdolby
name: Tanner Dolby
---
{% metagen
    title=title or metadata.title,
    desc=desc or metadata.desc,
    url=url + page.url,
    img=image,
    img_alt=alt,
    twitter_card_type=type,
    twitter_handle=twitter,
    name=name
%}
```
{% endraw %}

As a general rule, don't forget your in a templating engine context. Use your variables in the shortcode as you would inside {% raw %}`{% var %}`{% endraw %} tags or {% raw %}`{{ var }}`{% endraw %} without the curly braces like `title=var`.

### Liquid Usage

If you would like to use Liquid (which is the default templating engine in Eleventy). Then utilize the following syntax, to turn shortcodes like this:

{% raw %}
```liquid
---
data: 
    title: Eleventy Plugin Add Meta Tags
    desc: An eleventy shortcode for generating meta tags.
    url: https://tannerdolby.com
    img: https://tannerdolby.com/images/arch-spiral-large.jpg
    img_alt: Archimedean Spiral
    twitter_card_type: summary_large_image
    twitter_handle: tannerdolby
    name: Tanner Dolby
---
<head>
  {% metagen data %}
</head>
```
{% endraw %}

The Liquid usage is a bit different as the shortcode expects a single parameter representing an object with key/value pairs. You can define the object in frontmatter like shown above or within global data files.

## Shortcode Parameters
A list containing all of the parameters this plugin accepts. If you can't find a parameter that suits your all of your needs, feel free to [open an issue](https://github.com/tannerdolby/eleventy-plugin-metagen/issues) so we can get the parameter and/or corresponding meta tag added to the plugin.

### title
The content for `<title>`, `<meta name="title">`, `og:title` and `twitter:title`. If `og_title` and `twitter_title` are defined they will override the fields.

### name
The content for `<meta name="author">`.

### desc
The content for `<meta name="description">`, `og:description`, and  `twitter:description` if `og_desc` or `twitter_desc` isn't defined.

### generator
The content for `<meta name="generator">`. The value must be a free-form string that identifies one of the software packages used to generate the document. This value must not be used on pages whose markup is not generated by software, e.g. pages whose markup was written by a user in a text editor.

### comments
Display default comments for the Open Graph and Twitter tags.  
default: `false`

### og_comment
Display a custom comment for the Open Graph set of tags.  
requires: `comments: true`

### type
The content for `og:type`.  
options: `article`, `product`  
default: `website`

### url
The content for `og:url` and `twitter:url`.

### site_name
The content for `og:site_name`.

### locale
The locale these tags are marked up in.  
default: `en_US` 

### og_title
The content for `og:title`.  
default: `title`

### og_desc
The content for `og:description`.  
default: `desc`

### img
The content for `og:image` and `twitter:image`.

### img_alt
The content for `og:image:alt` and `twitter:image:alt`.

### img_width
The content for `og:image:width`.

### img_height
The content for `og:image:height`.

### twitter_comment
Display a custom comment for the Twitter set of tags.  
requires: `comments: true`

### attr_name
Define attribute name for the Twitter set of meta tags.  
options: `property`  
default: `name`

### twitter_card_type
The content for `twitter:card`.  
options: `summary_large_image`  
default: `summary`

### twitter_handle
The content for `twitter:site` or `twitter:creator` if `twitter_card_type: summary_large_image`.

### creator_handle
The content for `twitter:creator`. This tag is used if `twitter_card_type: summary_large_image`.

### twitter_title
The content for `twitter:title`.  
default: `title`

### twitter_desc
The content for `twitter:description`.  
default: `desc`