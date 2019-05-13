# Section 2

## Dynamic Styling with CSS Classes - Basics (Section 2, lecture 27)

We will look on styling our templates using Vue functionality.
For our first example, we will see how can we attach CSS classes using event listeners and Vue properties. We have 3 classes in our example, all with the class of `demo`, and already having some CSS rules so we could see them in the UI.

```js
 <div id="app">
        <div class="demo"></div>
        <div class="demo"></div>
        <div class="demo"></div>
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
            }
        });
    </script>

    <style>
        #app {
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: center;
        }

        .demo {
            margin: 1rem;
            width: 11rem;
            height: 11rem;
            border: 2px solid #6666;
        }
    </style>
```

Right now we don't have any Vue code involved at all, but we will create 3 new classes, each taking a `background-color` of a different color, and we would like to dynamically attach each to each of our `<div>`, using Vue.

```css
.orangered {
	background-color: orangered;
}

.springgreen {
	background-color: springgreen;
}

.royalblue {
	background-color: royalblue;
}
```

### Attaching classes using Vue code

Let's say we would like to attach the `orangered` class to the first `<div>` element only upon a click, and detach it upon a second click. First we would like to set a property with a boolean value in our `data` object, which will be used as a flag, initially it'll be set to `false`. Using `@click` on the relevant `<div>` element, we would like to specify that upon a click we would set the value of `attachRed` to be opposite of its current state, by specifying `attachRed = !attachRed`, basically moving the flag between `false` and `true` on each click.

```js
<div id="app">
        <div class="demo" @click="attachRed = !attachRed"></div>
        <div class="demo"></div>
        <div class="demo"></div>
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                attachRed: false
            }
        });
    </script>
```

That alone doesn't change the color as of now, we would also need to attach a CSS class conditionally (Only when our value is `true`). To do that, we need to attach a bind to the `class` attribute, and we could do this even though we already have a `class` attribute set to "demo", with `:class` we're actually using the Vue syntax, and not reusing the HTML class syntax - we're binding data to it, and behind the scenes Vue knows how to avoid conflicts with a possible existing HTML attribute. Now, Vue will expect us to pass a JavaScript object as an argument to the bind, with curly braces. This object should include: First, which CSS class I attach (as a key), Second, after ':', a boolean which will indicate if we should attach or not. If out class includes a special character, like a dash for example, we will need to inclose it within a quotation mark, but in our case we won't need now since our class name is `orangered`.

```js
  <div id="app">
        <div class="demo" @click="attachRed = !attachRed" :class="{orangered: attachRed}"></div>
        <div class="demo"></div>
        <div class="demo"></div>
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                attachRed: false,
            },
            computed: {

            }
        });
```

Now upon a click on the first `<div>`, it attaches the `orangered` class and set tge `<div>` background color accordingly. Upon a 2nd click, it returns to default state.
If we will copy this code to another `<div>`, both of them will be switched on and off by clicking on just one of the `<div>` elements.

## Dynamic Styling with CSS - Using Objects (Section 2, lecture 28)

The way we attached the CSS class `orangered` to our template, by passing it as an inline object, was rather simple, but what if we will have to pass on some more classes or conditionals? For example if we will also add the `springgreen` class to the same `<div>`, and set it to be at the opposite condition of `attachRed` property state, meaning that when `attachRed` is `false`, the `springgreen` class will be attached. Our code will look like this:

```js
  <div id="app">
        <div class="demo" @click="attachRed = !attachRed" :class="{orangered: attachRed, springgreen: !attachRed}"></div>
        <div class="demo"></div>
        <div class="demo"></div>
    </div>
```

This will achieve the purpose of toggling between the 2 colors, but now our template code looks bloated, and if we will continue to add more logic to it, it will get unmanageable fast.

### Storing and passing an object from a `computed` object property

We will add the `divClasses` property in the `computed` object, and we would like to `return` an object, which will be exactly like the object we set in the first `<div>`, it will set `orangered` when `this.attachRed` is `false`, and `springgreen` when it was changed to `true` by a click.

```js
new Vue({
	el: '#app',
	data: {
		attachRed: false,
	},
	computed: {
		divClasses() {
			return {
				orangered: this.attachRed,
				springgreen: !this.attachRed,
			};
		},
	},
});
```

Now we need to pass the `computed` property in the `<div>` itself for it to work, and we will simply replace the object we set before with `divClasses`.

```js
 <div class="demo" @click="attachRed = !attachRed" :class="divClasses">1</div>
```
