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

