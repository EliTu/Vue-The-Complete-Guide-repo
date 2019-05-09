<!-- markdownlint-disable MD010 -->
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