---
path: '/campaign-development/web-templates/web-display-utilities'
title: 'Web Template Display Utilities'
tags: ['gears', 'template', 'sitemap', 'es5', 'SPA Handling']
---

This gear is a beacon extension that provide some page behavior API included page exit intention, page inactivity, page scroll, and visible elements, and loaded elements watcher. It is built with SPA handling in mind. Even though it is written in es5, not all function is usable for IE.


## Usage

All methods
```ts
type UnbindFunction = () => any

interface Bindings {
    [id: string]: UnbindFunction
}

interface BaseMethods {
    pageElementVisible(selector: string, percentage: number): Promise<IntersectionObserverEntry>
    pageExit(delay?: number): Promise<Event>
    pageInactive(ms: number): Promise<Event>
    pageScroll(percentage: number): Promise<Event>
    pageElementLoaded(targetSelector: string, observerSelector?: string): Promise<Element>
}

interface BindingMethods {
    bind(id: string): BaseMethods
    unbind(id): void
    getBindings(): Bindings
    clearBindings(): void
}
```

___
```ts
bind(id: string): BaseMethods
```
- Bind with an id so that later one can unbind listeners/observers with the same id whenever they want.
    - re-binding with same id will unbind previous binding and set the new unbind method to the id.
    - if use bind() alone, it will generate id based on function name and selector
    - if there are no bind() method used, it will generate a random id
    
    ```js
    // Examples:
    // binded id is "id"
    Evergage.DisplayUtils.bind("id").pageElementVisible("selector").then(function(entry) {
        console.log("At least 1px of target in view.");
    });
    // binded id is "<pageElementVisible>selector"
    Evergage.DisplayUtils.bind().pageElementVisible("selector").then(function(entry) {
        console.log("At least 1px of target in view.");
    });
    // binded id is "nmelsbd6pq"(random)
    Evergage.DisplayUtils.pageElementVisible("selector").then(function(entry) {
        console.log("At least 1px of target in view.");
    });
    ```

---
```ts
unbind(id): void
```
- unbind with an id to cancel listeners/observers
    
    ```js
    // Examples:
    Evergage.DisplayUtils.unbind("id");
    ```

---
```ts
getBindings(): Bindings
```
- getBindings will get all ids with their unbinding methods
    
    ```js
    // Examples:
    Evergage.DisplayUtils.getBindings();
    ```

---
```ts
clearBindings(): void
```
- clear will go through each known bindings and unbind them all
    
    ```js
    // Examples:
    Evergage.DisplayUtils.clear();
    ```

---
```ts
pageElementVisible(selector: String, percentage: Number): Promise
```
- Triggers resolve function when the provided percentage of a target node is in the viewport
    - Defaults to firing when at least 1px of target is visible.
    - Only trigger once.
    - No IE support because it requires Intersection Observer API.
    - `entry` is the data when element is visible
    - Error "[pageElementVisible] Invalid arguments":
        - selector is not a string
        - Typeof percentage is not “number”
        - Percentage less than zero
        - Percentage greater than one

    ```js
    // Examples:
    Evergage.DisplayUtils.pageElementVisible("selector").then(function(entry) {
        console.log("At least 1px of target in view.");
    });
    Evergage.DisplayUtils.pageElementVisible("selector", 0.5).then(function(entry) {
        console.log("At least 50% of target in view.");
    });
    ```

---
```ts
pageExit(delay?: Number): Promise
```
<div class="alert-blue">

**Important:** While Interaction Studio web templates are based on responsive HTML that supports displaying web campaigns on mobile devices, the `pageExit` utility does not detect exit intent on mobile devices since it depends on listening for mouse move events on a page. 
</div>

- Triggers resolve function when the cursor leaves the bounds of the body from the top of the page with/without a delay. If more mouse movement is detected in the body within the delay period after leaving the window, the exit intent detection delay is cleared, but the event listener is not removed.
    - Default to 0ms delays which also means no delay.
    - Listening for the mousemove event.
    - Only trigger once.
    - `event` is the mousemove event when mouse leaves the page
    - Error "[pageExit] Invalid arguments":
        - Typeof ms is not a number
        - ms is less than 0

    ```js
    // Examples:
    Evergage.DisplayUtils.pageExit(2000).then(function(event) {
        console.log("Exit Intent Detected - 2 second delay");
    });

    Evergage.DisplayUtils.pageExit().then(function(event) {
        console.log("Exit Intent Detected - no delay");
    });
    ```

---
```ts
pageInactive(ms: Number): Promise
```
- Triggers resolve function when no user activity within the document after milliseconds.
    - Any mousemove, click, scroll, keyup and keydown events in the document will reset the inactivity timer.
    - Using `.then` only trigger once
    - Using `.subscribe` will continue to trigger on inactivity.
        - To discontinue, please call `event.disconnect`
    - `event` is the last event that triggers inactive.
        - Event types: mousemove, click, scroll, keyup, keydown
    - Error "[pageInactive] Invalid arguments":
        - Typeof ms is not a number
        - ms is less than or equal to 0
    
    ```js
    // Examples:
    var count = 0;
    Evergage.DisplayUtils.pageInactive(1000).subscribe(function(event) {
        if (count >= 3) {
            return event.disconnect();
        }
        console.log("inactive for 1 second");
        count++;
    });

    Evergage.DisplayUtils.pageInactive(2000).then(function(event) {
        console.log("inactive for 2 seconds");
    });
    ```

---
```ts
pageScroll(percentage: Number): Promise
```
- Triggers resolve function when user scroll pass provided page percentage.
    - Percentage is expected to be in decimal form.
    - Only trigger once.
    - Becareful for dynamic height pages.
    - There is no detection on page scrolling direction so it will trigger either from top to bottom or bottom to top.
    - `event` is the scroll event when scroll passed percentage.
    - Error "[pageScroll] Invalid arguments":
        - Typeof percentage is not a number
        - percentage is less than 0
        - percentage is greater than 1
    
    ```js
    // Examples:
    Evergage.DisplayUtils.pageScroll(0.5).then(function(event) {
        console.log("PageScroll has passed 50%");
    });
    ```

---
```ts
pageElementLoaded(targetSelector: string, observerSelector?: string): Promise
```
- Triggers when element is loaded on page.
    - Choose observerSelector wisely for performance issue.
    - Use Mutation Observer to watch added nodes.
    - Only trigger once.
    - `element` is the target element that loaded.
    - Error "[pageElementLoaded] Invalid arguments":
        - targetSelector must be an non-empty string
        - observerSelector must be an non-empty string
    
    ```js
    // Examples:
    setTimeout(function() {
        Evergage.cashDom("body div").append("<div class='evg-element'></div>")
    }, 1000);

    Evergage.DisplayUtils.pageElementLoaded(".evg-element").then(function(element) {
        console.log("Element is loaded | then");
    });

    Evergage.DisplayUtils.pageElementLoaded(".evg-element", "body div").then(function(element) {
        console.log("Element is loaded | delegate");
    });
    ```

---