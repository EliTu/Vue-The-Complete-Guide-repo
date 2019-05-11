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

But what if we really wanted to stick to the original title and output it in the `<h1>` tag, while also mutating the value of `title` to 'Hello' like we did? We can do just that with the `v-once` directive, that comes by default with Vue. We pass it into an HTML element hat holds an interpolation, like we have with `<h1>`. By adding this to an element, all the content inside of it will only be rendered once, and won't changed again when the `sayHello` method is being called and mutate the value of `title`.

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
