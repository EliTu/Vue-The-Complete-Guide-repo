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

Now we need to pass the `computed` property in the `<div>` itself for it to work, and we will simply replace the object we set before with `divClasses`. Now our template code looks much leaner and all the logic is within our Vue instance.

```js
 <div class="demo" @click="attachRed = !attachRed" :class="divClasses">1</div>
```

## Dynamically Styling with CSS Classes - Using Names (Section 2, lectrue 29)

Sometimes we won't want to decide straight away which class we would like to attach to a certain element, but we want to have the flexibility to calculate or dynamically set which class we want to be attached.

For this example, we will create a text `<input>` element and set `v-model` and bind it to a `data` property named `color`, which we will pass with the value of `royalblue`, refering to one of our CSS classes. Next, in the last `<div>` we will bind `:class` with the property of `color`, and now this `<div>` will hold the property that is being stored in `color`, which is in a 2-way binding with `<input>`.

```js
    <div class="demo" :class="color">3</div>
        <br />
        <input type="text" v-model="color">
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                attachRed: false,
                attachGreen: false,
                color: 'royalblue'
            },
            computed: {
                divClasses() {
                    return {
                        orangered: this.attachRed,
                        springgreen: !this.attachRed
                    }
                }
            }
        });
    </script>
```

We will see `royalblue` in the input field upon page reload, if we were to delete the value that is inside the input field, the class attachment will dissapear, if we were to pass a different class name, like `springgreen`, the `<div>` will dynamically receive the `springgreen` class, since the value of `color` changed dynamically to whatever we pass in the input field.

We're not determining weather or not a class should be attached or not with a boolean, like we done in the last few examples, but we're directly passing on a class that has a name that happens to be stored in the CSS classes, and it could pe passed to the `color` property and thus be set up in our `<div>`.

### Attaching multiple class names with the [] syntax

We could also attach multiple class names by using the array syntax in the binded `:class`, by passing [] and inside appending multiple properties, methods, objects etc. We add these elements as arguments, where the first one will take precedent, then the 2nd one and so forth.

```js
    <div class="demo" :class="[color, {orangered: attachRed}]">3</div>
    <br />
    <input type="text" v-model="color">
```

What we do here is basically first passing the `color` property as our first argument, which is whatever is passed within the `<input>`, and a conditional object as a second argument. If we were to delete the class name in the input field and set `attachRed` to true by clicking on the first `<div>`, the 3rd `<div>` will also attach `orangered` as its class, and display that color.

Vue basically analyzes this array of elements will pass, and calculates what should we put the class as based on what that list resolves to. In our case it resolves into an object with a key-value pair, where the key is the class name and the value is a boolean that determines if we should attach it or not.

## Setting Styles Dynamically (Without CSS Classes)(Section 2, lecture 30)

First, for this lecture we get the basic setting of 3 `<div>` elements with `demo` class and an `<input>` filed that has `v-model` and is doing a 2-way binding with a property named `color`, which is by default set to 'gray'.

By now we saw how to set up styling for our template by attaching and detaching classes, with booleans or key-value pairs and by dynamically setting a class name. Now we would like to see how to set styles by interacting with the `style` attribute directly, and not necessarily have to attach classes to elements.

It is simple, as basically we could bind `style` and simply pass an object that contains the CSS property we're targeting, lets say for example `background-color`, and either pass `'background-color'` (Between quotations, as it contains a dash that is not a valid JavaScript name character), or camelCase it and pass `backgroundColor`, then we simply pass `color` as its value, and now we can pass different HTML reserved color names or properties that contain a color, and the `<div>` will get its background color accordingly, because of the 2-way binding.

```js
<div id="app">
        <div class="demo" :style="{backgroundColor: color}"></div>
        <div class="demo" :style="myStyle"></div>
        <div class="demo"></div>
        <input v-model="color" type="text" />
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                color: 'gray',
            },

        });
```

### Setting a style object in the `computed` properties

Just like we've seen before, we could also have a `computed` property that returns an object that holds various properties, and these properties could be CSS style properties, which we then can simply pass into our template with the binding.

Let's say we would also would like to be able to control the `width` value of a certain `<div>`, and so we will create another `<input>` which is binded to a new `data` property named `width` which is set to 100 initially. In our `computed` properties we will set a function named `myStyle` which will return an object that contains 2 properties: `backgroundColor` and `width`, both corresponding to the `data` properties, only we would also like to specify the `px` when we pass the width value, and so we will pass `width: ${this.width}px`. Now we will pass the new `computed` property to the 2nd `<div>`.

```js
<div id="app">
        <div class="demo" :style="{backgroundColor: color}"></div>
        <div class="demo" :style="myStyle"></div>
        <div class="demo"></div>
        <input v-model="color" type="text" />
        <input v-model="width" type="text" />
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                color: 'gray',
                width: 100
            },
            computed: {
                myStyle() {
                    return {
                        backgroundColor: this.color,
                        width: `${this.width}px`
                    }
                }
            }

        });
```

Now by adjusting the values in both of the input fields, we can dynamically change the styles, the background color and width, of the `<div>` that are binded.
