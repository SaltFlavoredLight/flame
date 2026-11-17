# State Tree

like SwiftUI, everything is run on Background. buit differently, we use Object, notStruct (why? Swift have struct, JS don't)

and that's OK! Here let me show you what the concept is. you will also get more formal ideas on Documention.

## State

Like swiftUI, set persistant state should be simple.
just like:

```js
c.state.count = 1
```

Behind the sence, right before it start initallizing, this is a proxy() that make a intercept or not.

Wait?! so how to update?

you can update directly, but if you hate this magic, setSate is also a good choice!

```js
// how to update:
c.state.count ++;

// or this way:
c.setState((prevState) => ({
    count: prevState.count + 1
}));
```

kick a note: you always can read using `c.state` directly, but the perference is much depend on what you want to use.
and to prevent problems, always use this:

```js
c.vars.internalState = "message";
```

Of Course, you can still perform updates:

```js
c.vars.internalState = "new message";
```

but it do not re-render your componets.

> **Best Prectice:**
>
> When to use internal varibles? when you make sure that's temp, only exist in single event, like a step of calculation.
>
> When to use Vars? when you sure that you need persist storage, but don't want to let it update the whole infomation.
>
> When to use State? when you want that magic, or any reason that you want to update atuomatclly when you change.
>
> Hint: It is OK to use Vars, but I don't recommend.

## Events

You can make a hooks on everything that just happend to you. here is how to.

```js
onCreate(c) {
    // do what you need to do.
    c.onPropsChange({});
}

onPropsChange(c, prevProps) {
    // If there passed new props, what I need to do?
}

onStateChange(c, prevState) {
    // what need to be changes?
}

onParentChanage(c, prevParentC) {
    // what you need to do when parent update their state?
}

onWindowsChange(c, prevWindowsC) {
    // update when windows change.
}

onGlobalChange(c, prevWindowsC) {
    // update when windows change.
}

onDelete(c) {
    // clean up the effects.....
}
```

those event will be registered.

> Don't use `onPropsChange()` if you just store them in state, there already have self.props help you!
>
> Also that keep in mind that `onPropsChange()` will NOT run on initlization. use the example above to see how to.

## Render

we We discussed before, `render()` is the final thing that does render and create the DOM.
But, there does have a Catch....... remember there have Compose and base level?

### Compose Level

just stack elements, like this:

```js
render(c) {
    return Group(
        Text("Hello"), // a system level Compose Level element
        CustomizeGroup(), // a Customize Group here.
        FButton("Infomation Here", () => {
            c.state.count += 1;
        }).margin(30)
    )
}
```

### Base Level

that's little bit complicated - there have no `render()`. and you get c.element for the Element ready to communicating. when you create Base Element, quickly declare `type: "div";` (or other div) for the communication. remember, to assure data is not lost, Always use c.vars .

### Wait, what is `c`'s feature?

there have many, determined by those:

| Name | Infomation | Notes |
| ---- | -------- | ---- |
| `c.state.*` | The State - it will atuomatclly re-render your element | Compose Level Only or it get error |
| `c.vars.*` | you can store anything without worry that will get a re-render | |
| `c.props.*` | Props that passed to you, updated atuomatclly if it get updated | |
| `c.parentState.*` | Same as `c.state`, but you will acsess parent's state and update atuomatclly (not recommanded unless you know what the parent is or you asked it on props) | if you update that, both you and your parent updated! |
| `c.parentVars.*` | Same as `c.vars`, but it is parent's one. | |
| `c.windowState.*`, `c.windowVars.*` | Whole window's `c.state` and `c.vars` | |
| `c.globalState.*`, `c.globalVars.*` | Whole App's `c.state` and `c.vars` | Remember, it is NOT PERSIST!*= |

for parent and windows - 