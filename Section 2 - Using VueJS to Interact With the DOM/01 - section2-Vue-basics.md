# Section 2 - Using VueJS To Interact with the DOM

## Understanding VueJS Templates (Section 2, lecture 9)

Looking at our starting code, we have a very simple Vue app, with one `<p>` displaying our `title`. We pass the `title` in our HTML using the `{{}}` syntax notation, also known as 'Interpolation' or 'String Interpolation'. This displays 'Hello World!' on the page.

```js
<body>
    <div id="app">
        <p>{{ title }}</p>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                title: 'Hello World!'
            }
        });
    </script>
</body>
```

### The connection between the Vue instance and the HTML

Our Vue instance, represented by the keyword of `new Vue()`, is the connected to our HTML markup, basically having the data connected to the DOM. We need to understand, that whenever we make an instance of Vue, we create this connection, and this allows Vue to take our HTML and create a template based on it. Vue at runtime does not take our HTML code and use the commands we pass in the HTML code. We can see this if we will open our dev tools and inspect the DOM tree, we see the HTML is purely HTML, and does not have any curly braces attached to it.

Vue creates a template based on the HTML markup, stores it internally and then uses this template to create and output the real HTML code that will be rendered to the webpage.

## How the VueJS Template Syntax and Instance Work Together (Section 2, lecture 10)

We can output the `title` property of our `data` object in our Vue instance into our HTML simply by passing the property name, we don't need to add any `this` keyword inside our `{{}}` to access it or anything, thus outputing data is very easy.

### The `methods` object

Not only the `data` object can pass properties into our HTML, but we can also pass in whole functions, and we can do this with the `methods` object, which will allow us to store methods inside the Vue instance. We'll create a simple method that returns a string 'Hello!'.

```js
<body>
    <div id="app">
        <p>{{ title }}</p>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                title: 'Hi Vue!',
                bool: true,
            },
            methods: {
                sayHello: function () {
                    return 'Hello!';
                }
            }
        });
    </script>
</body>
```

We can go to the `<p>` and inside the `{{}}` simply pass `sayHello()` to call the function and execute it in the DOM, whenever Vue renders our HTML. Now the `<p>` tag value will be 'Hello!' and that will also be displayed in the DOM.

```js
<body>
    <div id="app">
        <p>{{ sayHello() }}</p>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                title: 'Hi Vue!',
                bool: true,
            },
            methods: {
                sayHello: function () {
                    return 'Hello!';
                }
            }
        });
    </script>
</body>
```

We do need to note, that if we pass a method in our HTML with Vue, what should be returned and eventually displayed is a type of `string`, which is something that the DOM can present. Another important thing to note, is that we access the `sayHello` function straightforwardly, without any mentions that it belongs to the `methods` object or anything, just like we did with `title`.

## Accessing Data in the Vue Instance (Section 2, lecture 11)

In our Vue instance, if we wanted to output `title` in our `sayHello` function, so that the function instead of returning `Hello!` would just return whatever was pass in the `title` in the `data` object. If we would just `return` the `title`, this won't work, because unlike the HTML template, where we do have access to all the objects and properties thanks to the proxying the Vue does for us, in the Vue instacne, the JavaScript code, it is not so.

But we still have a way, and a relatively simple one, to access out `data` properties inside our methods. The `title` property sits inside the `data` object, and normally we won't be able to access it by calling `this.title`, because `this` will not refer to the `data` object. But Vue provides us some "magic" by proxying these properties in a way, that if we call `this` anywhere inside the Vue instance object, we will get an access to the properties or methods we need. Behind the scenes, Vue creates a rather easy access to these properties (We will discuss this in detail later when we'll talk about the Vue instance in detail).

Now we can get 'Hi Vue!' displayed on the page, but this time by getting the function to access the `title` property and return it.

```js
<script>
        new Vue({
            el: '#app',
            data: {
                title: 'Hi Vue!',
                bool: true,
            },
            methods: {
                sayHello: function () {
                    return this.title;
                }
            }
        });
</script>
```

## Binding to Attributes (Section 2, lecture 12)

Let's say we have a link property inside `data`, and it is a link to google.com. We would like to append it to our template, and naturally we would do that with an `<a>` tag. Inside the `href` attribute, we might think we should pass the link within interpolation string `{{}}`, but this will not work.

```js
    <div id="app">
            <p>{{ sayHello() }} - <a href={{ link }}>Google</a></p>
    </div>
```

By clicking on the link, we will be directed to a site that has '{{}}` in its domain, basically parsing the curly braces in the URL, which is clearly wrong. The reason is that **the Vue string interpolation cannot be used within any HTML element attribute**, only in places where we would normally show text, not HTML elements.

### Bind Directives with `v-bind`

But we still want to bind this link dynamically with out Vue instance, so how will we do that? we will use Directives to bind the link dynamically with `v-bind`. This directive tells Vue to not use t he normal attribute like a normal HTML attribute, but it should bind it. We do this by adding `v-bind`, followed by a ':' which assigns an argument to the `v-bind`, and then passing the argument, which is the attribute we want to bind, in our case the `href`. What we'll pass in the value of the `v-bind:href` attribute is simply `link`, between quotation marks (""), not interpolation ({{}}).

```js
    <div id="app">
        <p>{{ sayHello() }} - <a v-bind:href="link">Google</a></p>
    </div>
```

In turn, the `href` attribute is now bound to the Vue instance and can access the `link` property dynamically, thanks to the directive.

## Understanding and Using Directives (Section 2, lecture 13)

What is a directive? it's basically an 'instruction' we put in our code. Vue is basically shipped with a few built in directives, like the `v-bind`, and later we will see that we can also set custom directives by ourselves.

In the example we just saw, the `v-bind` is an instruction that tells Vue to bind something to our data in the Vue instance, and is used in cases where we can't really use the string interpolation to access the data in the instance.

Each directive takes an argument, which is passed using a colon, and the value of what should be passed inside of quotation marks: `v-(directiveName):(argument)="(value)"`. This allows us to pass dynamic data to HTML elements, something we otherwise have no option to do.

## Disable Re-rendering with `v-once`

Lets say we have an `<h1>` title, and inside we will pass our `title` property.

```js
<h1>{{ title }}</h1>
```

Simultaneously, we will go back to the `sayHello` method and mutate `title` by assigning `this.title` to something else.

```js
  methods: {
                sayHello() {
                    this.title = 'Hello';
                    return this.title;
                }
            }
```

If we wil now run the code, we will see 'Hello' being printed twice, once in the `<h1>` element, and once in the `<p>` element, where we pass `{{ title }}` in both of these tags, and that is because we're setting the value of `title` in the `sayHello` method.

But what if we really wanted to stick to the original title and output it in the `<h1>` tag, while also mutating the value of `title` to 'Hello' like we did? We can do just that with the `v-once` directive, that comes by default with Vue. We pass it into an HTML element that holds an interpolation, like we have with `<h1>`. By adding this to an element, all the content inside of it will only be rendered once, and won't changed again when the `sayHello` method is being called and mutate the value of `title`.

```js
<h1 v-once>{{ title }}</h1>
```

## How to output Raw HTML - `v-html` (Section 2, lecture 15)

We'll look at another cool Vue feature. Lets create a property named `finishedLink`, and unlike the `link`, this will take the full HTML markup as the value.

```js
data: {
    title: 'Hi Vue!!',
    bool: true,
    link: 'http://google.com',
    finishedLink: '<a href="http://google.com">Google</a>'
    },
```

Now we will create another `<p>` tag, and we would like to pass in the `finishedLink` property in it, if we will try to do this with string interpolation, we will just output the value as raw text to the UI, and not a parsed HTML element.

```js
<p>{{ finishedLink }}</p> // -> '<a href="http://google.com">Google</a>'
```

Why does the link is not being displayed like it did with the `link` property? This is the default behavior of Vue and it is done to prevent cross-site script injection attacks by a third party. **By default, Vue escapes HTML and does not render it, but renders text**, which is what we would want most of the time.

But if we do want to output an HTML element and we know it is from a safe source and cannot host a malicious script, we could render HTML code with the `v-html` directive. We will get rid of the interpolation, and inside the attribute pass `v-html`, without an argument, and the value will be the property that holds the raw HTML.

```js
<p v-html="finishedLink" /> // -> Google (With the link)
```

We should still use it very carefully, as it does expose our site to cross-site script attack, if we will let our users to input the content for example.

## Optional: Assignemt Starting Code (Section 2, lecture 16)

<!-- See the Exercise1 folder -->

## Listening to Evenets (Section 2, lecture 17)

Another thing that we could do with the directives is to directly listen for events on the HTML tags, with the syntax of `v-on:(eventName)="(callbackFunction)"`. There is also a shorthand for this with the `@` notation, it'll look like `@(eventName)="(callbackFunction)". Basically it is the opposite of the`v-bind` directive: instead of passing someone to the attribute, we want to "receive" something from it, which is an event.

We can use the `v-on` or `@` and pass many kind of events we would like to listen to: clicks, mouseon, mouseoff, scroll, load etc.

We would create a `<button>` and another `<p>` tag, which inside of it will hold the property of `counter`, initially set to 0. We would like to listen to a click on the button, and upon a click increment the counter by one, by calling a callback function.

```js
    <button type="button" @click="increase">Click</button>
    <p>{{ counter }}</p>

     new Vue({
            el: '#app',
            data: {
                counter: 0
            },
            methods: {
                increase() {
                    this.counter++;
                }
            }
        });
```

Now upon a click, the counter will be increased by 1. This is a great and fast replacement for writing JavaScript `addEventListener` to create a faster way to listen to events.

## Getting Event Data from the Event Object (Section 2, lecture 18)

Another thing that is important to remember with events in Vue is that vue `v-on` directives set us the `event` object that is created by the JavaScript runtime automatically, and we can access it through the callback function arguments. Whenever a `v-on` directive is passed, we could access that event object and all its properties and data that it stores, giving us a greater ability to manipulate event data.

For this example, we will create another `<p>` tag, and we would like to listen to a `mouseover` event on it, and pass on the X and Y coordinates whenever we our mouse is passing over it, updating `x` and `y` properties we pass in the `data` object. For that, we will create a callback function that takes the event object as an argument, named `e`, and then sets `x` and `y` as the `e.clientX` and `e.clientY`, which hold the data of the X and Y coordinates of the mouse.

```js
<p @mousemove="getCoordinates">Coordinates: {{ x }} | {{ y }}</p>

  new Vue({
            el: '#app',
            data: {
                x: 0,
                y: 0
            },
            methods: {
                getCoordinates(e) {
                    this.x = e.clientX;
                    this.y = e.clientY;
                }
            }
        });
```

Whenever we move the mouse over the `<p>` tag that holds the `@mouseover` directive, the `x` and `y` values will dynamically change and will also be dynamically dipslayed on the UI, since we pass them in the interpolation.

## Passing your own Arguments with Events (Section 2, lecture 19)

What if we wanted to pass our own argument? Meaning, pass on an argument to the callback function which we then can use in the callback function. Lets take the `counter` example - what if we wanted to pass on ana argument that will be a step that we can use in the `increase` method to increase the counter? We can do that easily with Vue.

We simply pass it in parantheses at the callback function in the HTML tag, inside inputing the the argument, which then we can pass on at the callback function and use.

```js
   <button type="button" @click="increase(2)">Click</button>
    <p>{{ counter }}</p>

       new Vue({
            el: '#app',
            data: {
                counter: 0,
            },
            methods: {
                increase(step) {
                    this.counter += step;
                }
            }
        });
```

We pass on `step` as our parameter at the `increase` callback function, set it as the increase amount, and pass 2 as our argument in the directive function call itself. Now every time we click on the button, the counter will be increased by 2.

### Passing the `event` object as an argument

We could also set to pass the `event` object as an argument of the callback function. We could do that, but we need to pay attention to the name we pass - Vue fetches and stores this argument for us, and we indicate it with `$event` at the arguments we pass in the event listener directive callback function. Back in the function itself, we could simply pass it as `e` like we done before.

`$event` is a protected name in Vue, and it is necessary to be passed as such to get access to that event as an argument of the callback function.

```js
   <button type="button" @click="increase(2, $event)">Click</button>
    <p>{{ counter }}</p>

       new Vue({
            el: '#app',
            data: {
                counter: 0,
            },
            methods: {
                increase(step, e) {
                    console.log(e);
                    this.counter += step;
                }
            }
        });
```

## Modifying Events - with Event Modifiers (Section 2, lecture 20)

Sometimes, events gives us few cases that we will need to solve, and Vue makes it easy for us to modify events according to our needs.

Lets go back to the `<p>` tag that displays the `x` and `y` coordinates, and add a `<span>` to it which we would like to act as a "dead-zone", meaning whenever we will hover our mouse on to him we will not receive the coordinate values of `x` and `y`.

```js
 <p @mousemove="getCoordinates">Coordinates: {{ x }} | {{ y }}
    <span> DEAD ZONE </span>
 </p>
```

If we will just put it as is, we will still get the coordinates because it is part of the `<p>` tag that listens to the `mousemove` event. And so, we would like to create another event listener for the `<span>` which will also listen to the `mousemove` event, but this time we will modify it.

To modify the `<span>` tag directive to stop listening to the coordinates, we could approach this in couple of ways:

-   **Return empty value** - Simply pass on an empty value.

```js
 <p @mousemove="getCoordinates">Coordinates: {{ x }} | {{ y }}
            <span @mousemove=""> (( DEAD ZONE ))</span>
        </p>
```

-   **callback function with `stopPropagation()`** - Exectue a callback function that uses the `event` object, and executes `e.stopPropagation()` method, which basically stops the event from being propagated to the element that holds it.

```js
 <p @mousemove="getCoordinates">Coordinates: {{ x }} | {{ y }}
    <span @mousemove=""> (( DEAD ZONE ))</span>
 </p>

 new Vue({
            el: '#app',
            data: {
                x: 0,
                y: 0
            },
            methods: {
                getCoordinates(e) {
                    this.x = e.clientX;
                    this.y = e.clientY;
                },
                preventMouse(e) {
                    e.stopPropagation();
                }
            }
        });
```

-   **Vue event modifiers** - Instead of calling a callback function on that element, we can leave the value empty and use event modifiers that allows us to modify the behavior of the selected event. There are several event modifiers that comes with Vue (Full list at the lecture resources), but in our case we can use the `stop` modifier to achieve our goal, by simply passing it after the event with the dot notation, like this: `@mouseover.stop=""`.

```js
  <p @mousemove="getCoordinates">Coordinates: {{ x }} | {{ y }}
        <span @mousemove.stop=""> (( DEAD ZONE ))</span>
  </p>
```

This negates the need to write an extra callback function, and is a better practice than just leaving the value empty by itself. the `.stop` is basically like an "intermidtate" Vue function that does the job for us, it stops the propagation.

Another important modifier that we can encounter a lot is `.prevent`, which runs `preventDefault()` functions to stop the default behavior, usually paired together with submit buttons to prevent page reload upon a click.

Another thing that we can do is to chain modifiers, for example

```js
  <p @mousemove="getCoordinates">Coordinates: {{ x }} | {{ y }}
        <span @mousemove.stop.prevent=""> (( DEAD ZONE ))</span>
  </p>
```

## Listening to Keyboard Events (Section 2, lecture 21)

Like we specific modifiers for various events, we also have modifiers for keyboard keys, meaning that we can modify an event listener to listen to specific keys being pressed, like enter, space, delete, key-up etc...

The way that it is done is by first to pass on an event that listens to key strokes, and then we could modify it by passing the specific key we're interested in.

For example, we have a text input field, and whenever a key is being pressed, a callback function is called that will alert a message.

```js
 <input type="text" @keyup="alertMe">

  new Vue({
            el: '#app',
            data: {

            },
            methods: {
                alertMe() {
                    alert('Alert');
                }
            }
        });
```

The problem now is that we will have an alert popup every time any key is pressed, and that is too much. In this case, we could use modifiers to indicate a specific key to be pressed in order to create the alert, like the enter key for example. We could indicate it easily and fast using the event modifiers, by simply passing `.enter` to the directive.

```js
<input type="text" @keyup.enter="alertMe">
```

Now the alert will popup only if we will press the enter key, and will ignore it on any other key press.

Another thing that we could do with the modifiers, as we know, is to chain up key modifiers, that way we could easily specify multiple keys to be listened to.

```js
<input type="text" @keyup.enter.space.delete="alertMe">
```

Now the alert will pop up upon pressing enter, space and the delete button.

## Exercise 2

<!-- See Exercise2 folder -->

## Writing JavaScript Code in Templates (Section 2, lecture 22)

When passing JavaScript expressions and properties in the template, in the interpolation or the Vue attribute values, we could also pass whole JavaScript expressions right inside the template, within the interpolation or within the quotation marks, and don't need to output a single value or a method.

Basically in every place where we access the Vue instance, we could write any valid JavaScript code, as long as it only has one expression, and doesn't contain any complex expression like `if` statement, `for` loop etc. So statements like a simple arrow function, ternary operator, simple arithmetic calculations etc can be passed within the interpolation of the Vue instacne in the template, and very simple arithmetic calculations could be passed withim the attribute value of the directive.

For example we would duplicate the click button and pass few JavaScript expressions right within the Template, instead of passing properties and methods.

```js
<button type="button" @click="increase(2, $event)">Click</button>

<button type="button" @click="counter++">Click</button>
<p>{{ `${counter} ${counter * 2 > 10 ? 'Greater than 10' : 'Smaller than 10'}` }}</p>
```

Earlier we called a method to increment the counter, but we can also do that by passing `counter++`, which will increment the value of `counter` by one upon each click. In the interpolation we could pass expressions like template strings, and within pass ternary operators, and arithmetic operators (that would eventually override the increment expression we passed in the event listener).

## Using Two-Way-Binding - `v-model` (Section 2, lecture 23)

Another feature available for us to manipulate data within the template is the `v-model` directive, which basically allows us the set and get data at the same time. We saw how we could output data by passing the properties of the `data` object in the string interpolation, and saw how we could listen and set data with the `v-on` directive, but with `v-model` we could do both in the same time, which is the two-way data binding concept.

In our example, we have an input field and an empty `data` property named `name`, we could populate the input field with value, and at the same time set that value to whatever we pass in the input field. We do this by passing `v-model` in the `<input>`, and as a value passing whatever property we want to bind the input field to.

```js
 <input type="text" v-model="name">
<p>{{ name }}</p>

 new Vue({
            el: '#app',
            data: {
                name: ''
            },
        });
```

Now whatever we pass in the input field will be set as the value of `name`, and we could confirm this by looking at the Vue dev tools. Of course, it will also work the other way around, as if we manually set the value of `name` to some string, it will be present within the text field upon the page load.

Basically it is a syntactic sugar for the following code:

```js
<input
   v-bind:value="something"
   v-on:input="something = $event.target.value">
```

`v-model` directive simplifies our code and make it easy to listen and set values for properties.

## Reacting to Changes With Computed Properties (Section 2, lecture 24)

We'll go back to a previous example we've seen with the counter, we have a button and whenever we click on it we fire the callback function `increase` that simply increases the `counter1` by one, and outputs a result that says if the value of `counter1` is greater or smaller than 5, depends on the current amount of `counter1`

```js
    <div id="app">
        <button @click="increase">Increase counter1</button>
        <p>Counter: {{ counter1 }}</p>
        <p>Result: {{ result }}</p>
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                counter1: 0
                result: ''
            },
            methods: {
                increase() {
                    this.counter1++;
                    this.result = this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
            }
        });
```

That is a nice and very easy to maintain code, but it is very simple and in bigger applications we would have a much more verbose and complicated code that may not be so simple as this. If we will use a counter variable in many other places in the app and other properties may depend on it, so greater portions of the app would have to rely on the value of that property, it could quickly get very problematic to manage all of them with the `increase` method.

It would already get harder to maintain if we for example add a decrease button that would simply do the reverse operation, we would have to create another method named `decrease` that would handle the reversed operation.

```js
    <div id="app">
        <button @click="increase">Increase counter1</button>
        <button @click="decrease">Decrease counter1</button>
        <p>Counter: {{ counter1 }}</p>
        <p>Result: {{ result }}</p>
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                counter1: 0
                result: ''
            },
            methods: {
                increase() {
                    this.counter1++;
                    this.result = this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
            },
                decrease() {
                    this.counter1--;
                    this.result = this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
            }
        });
```

### Fixing the code

This works, but we have the same time of code in 2 different places, and if we will have some more things we need to do with `counter1`, it will quickly get very unmaintainable. Luckily for us, Vue makes it much easier to modal such cross-property code and dependencies. We'll get rid of both functions, and simply pass the increment and decrement operation for `counter1` in the event listener itself.

We would also like to update our result like we had before. We can't set the logic for that in the `data` object by simply passing the ternary operator in the `result` property, `data` is not reactive in Vue.

One way to do that is we could do that with a simple method that returns the result of `counter`. Then we call the `result` method in the template, get rid of the `result` property and now we have the same code execution like we had before.

```js
    <div id="app">
        <button @click="counter1++">Increase counter1</button>
        <button @click="counter1--">Decrease counter1</button>
        <p>Counter: {{ counter1 }}</p>
        <p>Result: {{ result() }}</p>
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                counter1: 0
            },
            methods: {
                result() {
                    return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
                }
            }
        });
```

### The problem

If we will add a third button, which will have `counter2` that is being incremented, the `result` method will still be executed upon each update of the page, as Vue does that automatically whenever one of the `data` properties changes, and doesn't know if the `result` function we call is using one of the properties we changed, basically doesn't know if the increment of `counter2` would influcence the `result` method, and so it re-call this method at any way.

```js
    <div id="app">
        <button @click="counter1++">Increase counter1</button>
        <button @click="counter1--">Decrease counter1</button>
        <button @click="counter2++">Decrease counter2</button>
        <p>Counter: {{ counter1 }}</p>
        <p>Result: {{ result() }}</p>
    </div>

    <script src="./vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                counter1: 0,
                counter2: 0
            },
            methods: {
                result() {
                    return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
                }
            }
        });
```

This is not a problem at this kind of simple app, but can get very CPU heavy for bigger apps that run a vast function that have many different operations, we won't want to execute it unnecessarily, but by default, the engine would do it anyway.

### The solution: the `computed` object

For this case we have a new object for the Vue instance objects, and that is the `computed` object. The `computed` object allows us to store properties, which will mostly be functions. For example, we would store a property named `output`, which will be a method, where we return the same thing the `result` method returns.

```js
new Vue({
	el: '#app',
	data: {
		counter1: 0,
		counter2: 0,
	},
	computed: {
		output() {
			return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
		},
	},
	methods: {
		result() {
			return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
		},
	},
});
```

It looks the same as the `result` method, but has a different name., **But it is being passed in a different way: we pass it like we would pass a property, basically without the parantheses**, we don't call it as a function, but use it like a property stored in the `data` object.

```js
<div id="app">
        <button @click="counter1++">Increase counter1</button>
        <button @click="counter1--">Decrease counter1</button>
        <button @click="counter2++">Decrease counter2</button>
        <p>Counter: {{ counter1 }}</p>
        <p>Result: {{ result() }} | {{ output }}</p>
</div>

<script src="./vue.js"></script>
<script>

new Vue({
	el: '#app',
	data: {
		counter1: 0,
		counter2: 0,
	},
	computed: {
		output() {
			return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
		},
	},
	methods: {
		result() {
			return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
		},
	},
});
```

Upon reloading of the page, we would see the string that being returned from `result` and `output` is side by side and it updates dynamically upon clicking on increment or decrement of `counter1`. The difference behind the scenes now is, the `result` function is being called every time, upon changing `counter1` or `counter2`, because it doesn't take into account which properties are being passed within the function and which should change, but a `computed` property, `output` method, does analyze the code and does take into account what should be updated, and thus won't run if we will hit the increment button om `counter2`.

We can prove this if we will pass some `console.log` into both of the methods, to see what is being printed when we press on the buttons. We will also add `counter2` to the template to see its result.

```js
<div id="app">
        <button @click="counter1++">Increase counter1</button>
        <button @click="counter1--">Decrease counter1</button>
        <button @click="counter2++">Decrease counter2</button>
        <p>Counter: {{ counter1 }} | {{ counter2 }}</p>
        <p>Result: {{ result() }} | {{ output }}</p>
</div>

<script src="./vue.js"></script>
<script>

new Vue({
	el: '#app',
	data: {
		counter1: 0,
		counter2: 0,
	},
	computed: {
		output() {
			console.log('Computed!');
			return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
		},
	},
	methods: {
		result() {
			console.log('Method!');
			return this.counter1 > 5 ? 'Greater than 5' : 'Smaller than 5';
		},
	},
});
```

When we press on the increase button of `counter1`, we see that both `Method` and `Computed` are being printed to the console, meaning both are being executed, and this makes sense because the `counter1` changes and it affects both of the functions. If we will incrase `counter2`, a value that is not being passed in the `output` method, we only see `Method!` being loged, meaning that only `result` was executed and `computed` didn't. This makes sense because `output` function, as a `computed` method, does take into account what values and properties exist inside of it, and then will only be executed if there is a change in these properties.

The conclusion is that we should use `computed` properties if we have such dependencies, and we need to cache the result, not unnecessarily recalculate all the properties, and so we should take more advantage of the `computed` concept.

## Reacting to Changes with Computed Properties - using `watch` object (Section 2, lecture 25)

Vue has another Vue instance object that we should know well, and that is the `watch` object, that joins to the group of Vue objects we know by now. Basically, `watch` executes code when a certain property that it watches has changes made to him.

With `computed` methods, we first set the property in `data` and then pass on the logic inside a method in the `computed` object, telling how the property should be computed. In `watch` it works kind of the other way around: as the property key, we set a name of an already included `data` property that we would like to watch - so if we want to watch `counter1`, we will pass it as the name of the key. We will then pass the `value` paramther, which is automatically passed into the function, and it cathes the current value of the property we're watching.

```js
new Vue({
	el: '#app',
	data: {
		counter1: 0,
		counter2: 0,
	},
	computed: {
		output() {
			return this.counter1 > 5 ? 'Greater than 5' : 'smaller than 5';
		},
	},
	watch: {
		counter1(value) {},
	},
	methods: {
		result() {
			return this.counter1 > 5 ? 'Greater than 5' : 'smaller than 5';
		},
	},
});
```

### When to use `computed` and when to use `watch`

At this point, we could set it to act as the `computed` method we set before, but it is the best practice to use `computed` methods to perform this kind of synchronous tasks whenever we can, since its more optimal to work this way. **Best use `watch` methods to work with asynchronous code, and that is because `computed` properties are only being ran synchronously,**, and so if we will need to get responses from the server, HTTP requests, use AJAX etc we should achieve this with a `watch` method.

For this example we will use the `setTimeOut` function to reset the `counter1` value after 2 seconds.

```js
new Vue({
	el: '#app',
	data: {
		counter1: 0,
		counter2: 0,
	},
	computed: {
		output() {
			return this.counter1 > 5 ? 'Greater than 5' : 'smaller than 5';
		},
	},
	watch: {
		counter1(value) {
			// let vm = this; // In case not an arrow function
			console.log(value); // Returns the N clicks we made om the increment/decrement button.
			setTimeout(() => {
				this.counter1 = 0;
			}, 2000);
		},
	},
	methods: {
		result() {
			return this.counter1 > 5 ? 'Greater than 5' : 'smaller than 5';
		},
	},
});
```

If we are running ES5 code, meaning we pass a normal anonymous function, we will have to set `this` to a variable so we will be able to use it, but since we're using ES6 arrow function, the value of `this` is being lexically passed, and so it will always refer to the Vue instance.

Now we can hit the button to increment/decrement the counter value, and after 2 seconds it will be round back to 0. This happens because the `watch` method watches the `counter1` property, and upon a change fires a function that is being executed after 2 seconds. We're not retuning anything or setting any new values, but simply reacting to a change on an existing property we set before in the `data` object.

## Saving Time with Shorthands (Section 2, lecture 26)

As I've already used before, we can swap `v-bind:(attribute)` with `:(attribute)`, and `v-on:(event)` with `@(event)`

## Exercise 3

<!-- Check code in Exercise-3 folder  -->

##
