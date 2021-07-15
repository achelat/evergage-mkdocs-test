---
path: '/campaign-development/web-templates/web-flicker-defender'
title: 'Flicker Defender'
tags: ['flicker', 'defender', 'flicker defender', 'template', 'campaign', 'gears', 'config']
---

The Flicker Defender gear can help prevent flicker caused by the rendering of personalized content (Templates/Campaigns) served by the Interaction Studio platform.

### Requirements

- A synchronously integrated Beacon in the `head` of the site is a strict requirement for Flicker Defender to work correctly.
- A working sitemap is necessary for Flicker Defender to work correctly and efficiently. Documentation for writing a sitemap can be found [here](https://developer.evergage.com/web-integration).
- At least one Content Zone with a `selector` attribute. _(Note that it is the developer's responsibility to ensure that the CSS selector used is valid.)_

---

### Overview

In order to help prevent flicker as described above, Flicker Defender does the following:
- Initially hides every content zone which has a `selector` by temporarily applying the `visibility: hidden !important` style to elements targeted by that `selector`
- Each hidden content zone is then redisplayed depending on:
    - If it is not targeted by a Template/Campaign for which the user has qualified
    - Upon resolution of the Template/Campaign targeting it
    - When the `redisplayTimeout` is hit
- If a page is not matched or if the Sitemap errors, all hidden content zones will reshow following the `pageMatchTimeout`

While there is no limit for either of the timeout values, it is advised to stay under 10000ms (10 seconds). Additionally, `pageMatchTimeout` should be less than or equal to the `redisplayTimeout`, since `redisplayTimeout` ultimately decides when to reshow all hidden content.

---

### Timeout Configuration

The below Flicker Defense timeout options are configurable from within Site-Wide JavaScript.

```ts
setPageMatchTimeout(pageMatchTimeout: number = 1000): void
```
`pageMatchTimeout` controls a maximum length of time should be waited for a page to be successfully matched before reshowing all hidden content

```js
// Example:
Evergage.FlickerDefender.setPageMatchTimeout(2500);
```

---
<br>

```ts
setRedisplayTimeout(redisplayTimeout: number = 2500): void
```
`redisplayTimeout` controls the maximum length of time that content should ever be hidden

```js
// Example:
Evergage.FlickerDefender.setRedisplayTimeout(5000);
```

---
<br>

Please note that calls to `setPageMatchTimeout` and/or `setRedisplayTimeout` must be done before the call to `Evergage.init()` in Site-Wide JavaScript:

```js
// Example:
if (typeof (Evergage.FlickerDefender || {}).setPageMatchTimeout === "function") {
    Evergage.FlickerDefender.setPageMatchTimeout(2500);
}

if (typeof (Evergage.FlickerDefender || {}).setRedisplayTimeout === "function") {
    Evergage.FlickerDefender.setRedisplayTimeout(5000);
}

Evergage.init().then(() => {

    // sitemap

});
```

---
<br>

## Please note:

- By design, Flicker Defender does not run in the Template or Campaign Editors.
- `Evergage.init()` should be called as soon as possible in Site-Wide JavaScript, since Flicker Defender relies on that call to determine when it should begin hiding content zones.
- When injecting or forcing the Web SDK URL using the Evergage Launcher browser extension, you are simulating an asynchronous implementation of the product. Therefore, at least a small amount of Flicker will always appear when injecting or forcing a Web SDK onto the page.
