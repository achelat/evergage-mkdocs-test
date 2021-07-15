---
path: '/campaign-development/web-templates/web-template-handlebars'
title: 'Web Template Handlebars'
tags: ['template','handlebars']
---

With the [Handlebars Templates Gear](/campaign-development/web-templates#web-template-implementation-options) enabled, HTML received with a web campaign is rendered using [Handlebars](http://handlebarsjs.com/) templated HTML. 

---

**Note:** The above link to the external **Handlebars** site is provided for information purposes. Some of the code structures and functions of the Handlebars templating language are incorporated as part of the Web SDK when the Handlebars Templates Gear is installed and enabled. The Handlebars Templates Gear is installed with Interaction Studio and enabled by default, but an Interaction Studio administrator can disable or enable this Gear as required.

---

## Handlebars Templated HTML

Handlebars is a simple HTML templating language that uses a template and an input object to generate HTML. A handlebars expression is an opening double-brace `{{`, some contents, followed by a closing double-brace `}}`. Here is an example of a simple handlebars expression:

```<p>{{firstname}} {{lastname}}</p>```

In creating templates, developers enter Handlebars template code in the [Handlebars](/web-templates/web-template-global-templates) tab of the template editor. When a Handlebars template within an Interaction Studio web template is executed, each Handlebars expression is replaced with values from the specified input object. In Interaction Studio web templates, the template variables in the Handlebars expressions are populated by the [Web Template Context](/campaign-development/web-templates#web-template-context) provided by the [Client JavaScript](#client-javascript) objects specified in the **Clientside Code** tab.
