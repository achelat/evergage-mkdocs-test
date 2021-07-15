---
path: '/campaign-development/email-campaigns-and-templates/render-non-western-characters'
title: 'Render Non-Western Characters in OTE Templates'
tags: ['template', 'email campaigns', 'ote template', 'ote']
---

Along with personalizing content and product recommendations sent via email campaigns at open time, Interaction Studio Open-time item templates enable you to build localized email campaigns for recipients in non-Western locales. By leveraging web fonts in item templates, you can render non-Western characters to display localized dynamic message content fields or general message content.

To embed a web font in your item template, link to the location serving your web font along with the appropriate CSS declarations in the HTML code of your item template. The following code sample depicts how you can use the [Noto Sans](https://fonts.google.com/specimen/Noto+Sans) web font and related CSS declarations in your item template HTML code to render the product label `彼らの機器や装置はすべて生命体だ。` in Japanese.

```html
<!--Import webfont-->
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Noto+Sans">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Noto+Sans+JP">

<!--CSS declarations-->
<style>
body {
	padding: 12px;
}

.product-label {
	font-family: 'Noto Sans', 'Noto Sans JP';
	font-size: 12px;
	margin: 10px;
}

.product-image {
	width: 160px;
	display: block;
	margin-left: auto;
	margin-right: auto;
}
</style>

<!--Item content block-->
<div><img src="${item.imageUrl}" class="product-image"> </div>
<div class="product-label"> 彼らの機器や装置はすべて生命体だ。</div>
```

<div class="alert-blue">

**Important:** Interaction Studio OTE templates do not support multiple locales. To leverage OTE item templates in email campaigns across multiple locales, you must create separate OTE item templates for each locale your campaigns target. 
</div>