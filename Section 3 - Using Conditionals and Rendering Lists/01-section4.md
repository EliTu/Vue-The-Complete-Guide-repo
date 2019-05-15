# Section 4 - Using Conditionals and Rendering Lists

## Conditional Rendering with `v-if` (Section 4, lecture 35)

Vue makes it simple to work with conditionals to show or hide template elements from the page, depending on the conditional. The elements being hidden, are in fact being completley detached from the file, as if they were "deleted", and not simply have `display: none`.

Showing and hiding parts of the DOM is a regular task when building projects, especially when we work with popups, messages etc, so we want to conditionally show elements on the page, for example an error message pops up if a wrong input was submitted.

### The `v-if` directive

This is a directive we can add to any template element and it's bound to a condition or a conditional property which resolves to a boolean.

```js
    <div id="app">
 		<p v-if="show">You can see me! <span> Extra nested</span></p>
 		<p>Do you also see me?</p>
 	</div>

 	<script src=" https://unpkg.com/vue/dist/vue.js"> </script>
 	<script>
 		new Vue({
 			el: '#app',
 			data: {
 				show: true
 			},
 		});
```

Since `show` property is set to true, the `<p>` tag that has the `v-if` directive attached to it will be displayed. If we were to create a button, attach an event listener that sets `show` to false on a click, the moment we will hit the button, that `<p>` tag will dissapear from the UI.

```js
    <div id="app">
 		<p v-if="show">You can see me! <span> Extra nested</span></p>
 		<p>Do you also see me?</p>
 		<button @click="showMe()">Vanish</button>
 	</div>

 	<script src=" https://unpkg.com/vue/dist/vue.js"> </script>
 	<script>
 		new Vue({
 			el: '#app',
 			data: {
 				show: true
 			},
 			methods: {
 				showMe() {
 					this.show = !this.show;
 				}
 			},
 		});
 	</script>
```

Inspecting the DOM in the dev tools, we can see that the element is completley removed from the DOM, and all that is left is an HTML comment indicating that something was there. It is important to remember that `v-if` really attaches and detaches elements to the DOM, not setting their visibility style.

### Adding `v-else` to a different template element

We can extend the functionality of `v-if` and make it display a different element if the condition is set to `false`, by passing a `v-else` directive on different element. `v-else` will automatically refer to the latest `v-if` in front of it.

```js
    <div id="app">
 		<p v-if="show">You can see me! <span> Extra nested</span></p>
 		<p v-else>Now you see me!</p>
 		<p>Do you also see me?</p>
 		<button @click="showMe()">Vanish</button>
 	</div>

 	<script src=" https://unpkg.com/vue/dist/vue.js"> </script>
 	<script>
 		new Vue({
 			el: '#app',
 			data: {
 				show: true
 			},
 			methods: {
 				showMe() {
 					this.show = !this.show;
 				}
 			},
 		});
```

Now upon the click on the button, the two `<p>` tags are switching once the state of `show` changes.

### `v-if` also affects nested elements

Important thing to note is that by passing `v-if` on a certain element, all the children of that element will also be affected by the conditional, so if the condition will be `false`, the children will dissapear as well, like we see in the example above with the `<span>` tag inside of the `<p>` tag.

## `v-else-if` in Vue 2.1 (Section 3, lecture 36)

If you're using Vue.js version 2.1 or higher, you now actually have access to a v-else-if  directive. Have a look at this link to learn more: https://vuejs.org/v2/guide/conditional.html#v-else-if.

## Using an Alternative `v-if` Syntax with `<template>` (Section 3, lecture 37)

We can set up an alternative to attach or remove using the HTML5 `<template>` tag. This tag doesn't get rendered in the DOM, meaning it's like a placeholder element, but whatever we will pass inside will be rendered to the DOM.

```js
    <div id="app">
 		<p v-if="show">You can see me! <span> Extra nested</span></p>
 		<p v-else>Now you see me!</p>

 		<template>
 			<p>Lorem ipsum dolor sit amet.</p>
 		</template>

 		<p>Do you also see me?</p>

 		<button @click="showMe()">Vanish</button>
 	</div>
```

If we inspect the elements on our page, we won't see any wrapper around the `<p>` tag that is inested in the `<template>`. This is an alternative for creating `<div>` tags just to nest elements, and thus it can prevent clutter on the page. Moreover, we can easily attach `v-if` to the `<template>` tag, which will also allow us to toggle the content nested in the `<template>`.

```js
 		<template v-if="show">
 			<p>Lorem ipsum dolor sit amet.</p>
 		</template>
```

Inside the `<template>` we can wrap multiple elements, and so a `<template>` is a suitable option to group or nest multiple elements we want to show or hide.

## Don't Detach it with `v-show` (Section 3, lecture 38)

We saw that the `v-if` directive hides and shows elements on the page by detaching and attaching them from the DOM. There is another directive with a rather similar functionality on the surface, and that is `v-show`. This directive also takes a boolean value or property, and in case of `false`, will hide the element and his children from the page, only this time it won't remove it from the DOM completely, but simply set `display: none` style to that element, but they will still be available at the DOM.

```js
    <p v-show="show">Do you also see me?<span>Extra</span></p>
```

Upon clicking on the button, the element will dissapear from the UI. By inspecting the elements, we see that it has a `style= "display: none"` style that is being attached to it. By pressing on the button again, that property will dissapear and we will see the element again.

### Use `v-if` or `v-show` ? 

When we don't want to detach the element completely, we should of course use `v-show`, to hide the element but not removing it completely. Using `v-if` would lead to a better performance, since it removes elements from the page, thus allocating more memory since there are less elements in the DOM, and so this should be the default.

Another point to make is that `v-show` doesn't work with `<template>`, and so if we work with that HTML5 element, we should only use `v-if`, and if we need to nest few elements and use `v-show` on them, we should use `<div>`.

## Rendering Lists with `v-for` (Section 3, lecture 39)

In case we want to output lists fast, we could do this in no time with `v-for` directive, which is a built in Vue loop that can easily loop though an array and output all its elements to the DOM, usually inside a list element like `<ul>` or `<ol>`, on a `<li>` element, which will replicate each array element in its own `<li>` tag.

The syntax is `v-for="(current element) in (target iterable)"`, basically choosing a name to represent the items in the iterable object (usually an array).

 In our example we have an array of ingredients, if we would like to loop through it and output an `<ul>` with each array element in its own `<li>` tag, we will first create a `<ul>` in the template, pass one `<li>` and then use the `v-for` inside the `<li>`. Then we could simply pass `ingredient`, which represent the current array element into interpolation, in order for us to display it.

```js
    <div id="app">
 		<ul>
 			<li v-for="ingredient in ingredients">{{ ingredient }}</li>
 		</ul>
 	</div>

 	<script src=" https://unpkg.com/vue/dist/vue.js"> </script>
 	<script>
 		new Vue({
 			el: '#app',
 			data: {
 				ingredients: ['meat', 'fruit', 'cookies'],
 			},
         });
    </script>
    // Output:
    //  meat
    // fruit
    // cookies
```

The output would be a `<ul>` with 3 `<li>`tags, displaying the ingredients one by one.

## Getting The Current Index (Section 3, lecture 40)

What if we want to display the current index of each array element and display it in the list near each of the items? We could also do that in the `v-for` directive syntax. For this we will have to wrap our current element in parantheses, pass a coma and add a variable name that will represent the index in the loop, usually `i` or `index`. Then we could simply pass the `i` in our interpolation, which in turn will give us the index number of each element in the looped array.

```js
<li v-for="(ingredient, i) in ingredients">{{ ingredient }} ({{ i }})</li>
// Output:
// meat (0)
// fruit (1)
// cookies (2)
```

## Using an Alternative `v-for` Syntax with `<template>` (Section 3, lecture 41)

