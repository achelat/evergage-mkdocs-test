---
path: '/interface-config/properties/color'
title: 'Color Property'
tags: ['config', 'gears', 'template']
---
#### Color Picker
A basic color picker input.
```ts
colorProperty: Color;
```
![Color Picker](Color.png) 
The value returned from the selection contains both the hex value and the rgb value. 

The object structure is as follows:
```ts
{
    hex: string,
    rbg: {
        r: number,
        b: number,
        g: number,
        a: number
    }
}
```