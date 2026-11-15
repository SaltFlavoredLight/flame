# Lavels Explained

There use **hybrid mental model**— you can use wording in web destription (DOM-friendly, URL-driven routing) and native desktop destription (window management, scenes, compositional views). we will use a "/" to indicate for rest of them. and in the concept, we will perfer using Web destription.

## How App Level works

there have 3 levels here: App, Window/Scene, and Element/View.

### App

this is Global state, entry point, root lifecycle. your whole app is based on this.
Although seems like important (indeed), but it's Optional if you don't want SPA acsess.

### Window/Scene

That's a Single page the system have, AKA, a window here.

### Element/View

that's a Element that on showcase, it can be one of the 3 nodes that I will show later.

## Levels of Elements/VIew

there have 4 levels of Levels:

1. App
2. Window/Scene
3. High Level Elements/View
4. Node Elements/View
5. Construction

### App Level

That App will be used as a mode that contain all Element here, that include a Router to all WIndows, Manage Stack and SPA.
Out of your gasp, technically this is Optional, and you should not use it esp. when work with Astro.

### Window/Scene Level

You can use this Level to manage the elements, they can insert either Hieh Level Element/View or Node Level Ones.

### High Level Elements/View Level

You can create or use High Level Element/View, that element will depend on Low Level Element.
When you design a custom Element, you just stack up the Node Element/View
It works by just Embed Node Level Completely.

### Node Elements/View Level

That is an element that High Leve Element will use, but they are the only one (in theroy) that allowed for acsess DOM.
There will have custom Element/View to self-manage the life-cycle of the Element/View, Connect with DOM or native element, And Determine the function that this will be created.

**Note:**

1. if you are using Web Standard, you can find that you don't need so much because all Element for the web is avliable for you!
2. However, if you want to embed vailia JS like [prosemirror](https://www.prosemirror.net/), which need native web event, you still can just build a wrap, and it's easy, even AI who have the AI Agent Guide can do it for you with just less than 150 lines of code.
3. Keep in mind that this framework only support ESM - make sure you are using Import to import JS if you want to embed JS container like [prosemirror](https://www.prosemirror.net/).
4. Also, it is Cross platform, if you say you support both Web (by default) and NodeGUI, you technically just write 2 version of code in same Componets and make it cross platform.

### Construction

that's something that should be enter as a varible editor, like Color.
It should import final as a CSS or HTML varibles.

## What the code should be

it pretty like SwiftUI (well, it will be more perfect if you use Python, I hate that Js 'new'......).

here is a designed Usage:

```js
'use strict';

// techincally, you can call it with Any Names.
import flameworkUI from "flame";

// You can write your usgae here.
const Text = flameworkUI.elements.base.Text;
const e = flameworkUI.createElement;
const Color = flameworkUI.Color;

// usage on Bottom
Text("infomation here").style(Style.color("#235265"));

// Or this way also work
e(Text, {style: Style.color("#235265")}, "Infomation here");
```

at least you see, this use a modefier and a class, or react style put it to another function.
no matter what, they will produce exact same result - a base level elementTree.

### Wait, why it's Reverse Order?

the `e()` in this function successfully showed that it's like react's [createElement](https://react.dev/reference/react/createElement).
but the main reacton it use Text is becuase that sometimes, like the example below, use mutiple slots.
(of couse that this is a high level button)

```js
const FButton = flameworkUI.elements.high.Button;

// I will tell you how to use that and update state later.
function IncreaseByOne() {}

// it use up to 2 inputs at same time. 
FButton("Click Me", IncreaseByOne).style(Style.color("#235265"))

// that's same as
e(FButton, {style: Style.color("#235265")}, "Click Me", IncreaseByOne);
```

if you are not comfortable with it, use other one, but you can see trade-offs (at least, you can mix them, Nice)

```js
const ec = flameworkUI.createElementClassic;

// worked.
ec(FButton, {"Click Me", IncreaseByOne}, {style: Style.color("#235265").padding(4)});

// trade off?
ec(Text, {"Infomation here"}, {style: Style.color("#235265").padding(4)});
```

that will be worse if your element use a object `{}` on the original styling, you need make a `{{}}`, which no one knows what you are doing...
But Even though I still love it.

### So how should I read this?

all 5 level inside have a `render()` function, so I can chain it until the end. I will talk more on next document