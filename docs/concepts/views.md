# State Tree

like SwiftUI, everything is run on Background. buit differently, we use Object, notStruct (why? Swift have struct, JS don't)

and that's OK! Here let me show you what the concept is. you will also get more formal ideas on Documention.

## State

Like swiftUI, set persistant state should be simple.
just like:

```js
this.state.count = 1
```

Behind the sence, right before it start initallizing, this is a proxy() that make a intercept or not.

Wait?! so how to update?

you can update directly, but if you hate this magic, setSate is also a good choice!

```js
// how to update:
this.state.count ++;

// or this way:
this.updateMethod = "setState"; // you will see the form that you know how to update.
this.setState({
    count: this.state.count + 1
});

// or this way:
this.updateMethod = "setFunction";
this.setState((prevState) => ({
    count: prevState.count + 1
}));
```

kick a note: you always can read using `this.state` directly, but the perference is much depend on what you want to use.
and to prevent problems, always use this:

```js
this.vars.internalState = "message";
```

Of Course, you can still perform updates:

```js
this.vars.internalState = "new message";
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
onCreate() {
    // do what you need to do.
    this.onPropsChange({});
}

onPropsChange(prevProps) {
    // If there passed new props, what I need to do?
}

onStateChange(prevState) {
    // what need to be changes?
}

onDelete() {
    // clean up the effects.....
}
```

those event will be registered.

> Don't use `onPropsChange()` if you just store them in state, there already have self.props help you!
>
> Also that keep in mind that `onPropsChange()` will NOT run on initlization. use the example above to see how to.

## Render

we We discussed before, `render()` is the final thing that does render and create the DOM.
But, there does have a Catch....... remember there have high and base level?

### High Level

just stack elements, like this:

```js
render() {
    return Group(
        Text("Hello"), // a system level High Level element
        CustomizeGroup(), // a Customize Group here.
        FButton("Infomation Here", () => {
            this.state.count += 1;
        }).margin(30)
    )
}
```

### Base Level

that's little bit complicated - there have no `render()`. in fact, it using a 

```js
```


### Then, how to create element?