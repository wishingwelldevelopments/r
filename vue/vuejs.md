---
title: Getting Started With Vue 3
---
## Installation and Setup
Below are the steps required for installing the vue cli and creating a new project

```bash
npm i -g @vue/cli
vue create project-name
yarn serve
```

## Project Structure
Below is the structure of a vuejs project.
```txt
.
├── public
│   ├── favicon.ico
│   └── index.html -- template where components get injected
├── README.md
├── src
│   ├── App.vue -- master vue component
│   ├── assets -- asset store
│   │   └── logo.png
│   ├── components -- component store
│   │   └── HelloWorld.vue -- secondary component
│   └── main.js -- responsible for mounting App.vue to index
└── yarn.lock
```
## Vuejs Hooks
Vuejs comes with many methods that can be used for various different reasons. These are pretty much all self explanatory. Here are a list of most of the hooks :

* beforeCreate() - before compoenent is created
* created() - upon the components creation
* beforeMount() - before the compoenent is mounted to DOM
* mounted() - upon the compoenent being mounted to DOM
* beforeUpdate() - before data is updated
* updated() - upon data being updated
* beforeDestroy() - before component is destroyed
* destroyed() - upon component being destroyed. i.e. routing unmount

### beforeCreate()
The beforeCreate hook runs at the very initialization of your component. data has not been made reactive, and events have not been set up yet

### Created()
You are able to access the component's reactive data and modify them. Templates and Virtual DOM have not yet been mounted or rendered
```js
export default {
  name: 'App',
  created(){
    console.log('compoenent created but not mounted');
  }
}
```

### mounted()
compoenent has been mounted and you have full access to the reactive data, templates, and rendered DOM elements
```js
export default {
  name: 'App',
  mounted(){
    console.log('compoenent created and mounted');
  }
}
```

### updated()
called when ever a component's reactive data property is changed
```js
export default {
  data(){
   return{
    foo: 'herro'
   }
  },
  mounted(){
   console.log('component mounted. foo equals (herro) : ' + this.foo);
   this.foo = "bar" //changing foo equal to bar
  },
  updated(){
   console.log("foo updated to (bar) " + this.foo);
  },
}
```

## Vue Expressions
The text inside the mustache tags { } are called binding expressions. These consist of a single javascript expression and can optionally have a filter. Here is a basic example of using expression :
```js
<p>{{ myVariable }}</p>

<script>
  export default{
    data(){
      return{
        myVariable:'Hello World'
      }
    }
  }
</script>
```
This produces the following output :
```txt
Hello World
```

### More Examples
Because vue expressions can make use of regular javascript they can be used very powerfully. Here a some examples :
```js
//If myString equals foo then return "Foo" else return "Bar"
<p>{{ myString === "Foo" ? "Foo" : "Bar" }}</p>

//Get each item in array and square root dem
<p>{{ myArray.map(t => t ** 2) }}</p>

<script>
  export default{
    data(){
      return{
        myArray:[1,2,3,4,5],
        myString:'foo'
      }
    }
  }
</script>
```
This produces the following output :
```txt
Foo is Foo
[ 1, 4, 9, 16, 25 ]
```

## Vue Directives
HTML attributes created by vue that allow your elements to be reactive. Here are some of the available vuejs directives :

* v-if
* v-bind
* v-once
* v-model
* v-on (click, keyup, keydown)
* v-for
* v-text
* v-html
* v-pre
* v-ref
* v-show
* v-transition

Below is an example of the `v-once` directive :
```js
//The v-once directive forces the html element to rendered only once
//Thus it is NOT being updated to "Bar"
<p v-once>{{ myString }}</p>

<script>
  export default{
    data(){
      return{
        myString:"Foo"
      }
    },
    mounted(){
      this.myString = "Bar";
    }
  }
</script>
```
This produces the following output :
```txt
Foo
```
