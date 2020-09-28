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

* beforeCreate() - before component is created
* created() - upon the components creation
* beforeMount() - before the component is mounted to DOM
* mounted() - upon the component being mounted to DOM
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
    console.log('component created but not mounted');
  }
}
```

### mounted()
component has been mounted and you have full access to the reactive data, templates, and rendered DOM elements
```js
export default {
  name: 'App',
  mounted(){
    console.log('component created and mounted');
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
## Properties and Watchers
Vue properties are similar to hooks. They run when a certain event occurs. Here are a list of properties :

* watch:{}
* computed:{}
* methods:{}

## Expressions
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

## Composition API
The composition api was introduced in vue 3. This is a new way of writing the logic for vue components. The logic previously seen has been using whats called the options api. The options api can still be used. The composition api aims to make the logic more grouped together and help some how with mixins (don't understand as of yet.)

Here is an example component utilizing the composition api :
```html
<template>
    <input type="text" v-model="state.username" @keyup.enter="state.foo = state.username">
    <div v-if="state.bar.length > 0">
      <h1>Repos</h1>
      <p v-for="b in state.bar" :key="b.name">{{ b.name }}</p>
    </div>
</template>

<script>
import {reactive, watch} from 'vue';

export default {
  name: 'Home',
  setup(){
    const state = reactive({
      username : null,
      bar : [],
      foo : null
    })

    watch(() => {
      if(state.foo != null){
        console.log('fooing...');
        fetch(`https://api.github.com/users/${state.username}/repos`)
          .then((res) => res.json())
          .then((data) => {
            state.bar = data;
          });
        state.foo = null;
      }
    })

    return{
      state,
    }

  }
}
</script>
```

Logic is placed within the setup function which is run when the component is created.

`const state = reactive({ ... })` is where you define your data objects. You must then return the state and access in template like `state.dataObject`

### Hooks and Watches in Composition API
* ~~beforeCreate~~ -> use setup()
* ~~created~~ -> use setup()
* beforeMount -> onBeforeMount
* mounted -> onMounted
* beforeUpdate -> onBeforeUpdate
* updated -> onUpdated
* beforeDestroy -> onBeforeUnmount
* destroyed -> onUnmounted
* activated -> onActivated
* deactivated -> onDeactivated
* errorCaptured -> onErrorCaptured
* onRenderTracked
* onRenderTriggered

### Accessing The Router
Previously to do something like reloading the route you would use `this.$router.go` however this is no longer available. Instead do something like this :
```js
<button @click.prevent="mefunky()">click meh</button>

import router from '@/router';

setup(){
  const mefunky = () => router.go()
}
```

## Introduction to Vuex
Vuex is essentially a global state whereby every component can read and write to the store.

### Installing Vuex
Install it via the following commands :
```bash
yarn add vuex@next --save
mkdir src/store #create if not already created
touch src/store/index.js #create if not already created
```

#### Contents of src/store/index.js :
```js
import { createStore } from 'vuex';

export default createStore({
  state:{
    user:'Bill Jones'
  },

  mutations:{
    SET_USER(state, user){
      state.user = user;
    }
  },
  actions:{
    setUser({ commit }, user){
      commit('SET_USER', user);
    }
  },

  modules:{
  }

})
```

### State vs Mutations vs Actions vs Modules
* State : the store where you define your data variables (e.g. user : "bill")
* Mutations : a function that gets run within the vuex store to update the state
* Actions : a function that you run in a component which runs your mutation
* Modules : a way of modularising states in a seperate file for better tidiness

### Running Actions From Components
You run actions from your component. These then trigger the mutation and thus update the store's data
Here is an example component making use of the action `setUser` as defined above :
```js
import { useStore } from 'vuex';
const store = useStore();

const updateStore = () => {
  store.dispatch('setUser', 'John Doe')
}
```

### Making Use of Modules
Modules allow you to seperate your data into their own files. Modules are stored in `src/store/` and are called `module.js`
Here is the updated `src/store/index.js` file :
```js
import { createStore } from 'vuex';
import { UserModule } from './user'

export default createStore({
  state:{},
  mutations:{},
  actions:{},

  modules:{
    User : UserModule
  }

})
```
Import the new module (`UserModule`) and then declare it in the modules section with a name of your chooising (`User`). Your data then goes in the (`src/store/user.js`) file and looks like this :
```js
export const UserModule = {
  namespaced: true,
  state:{
   firstname:'Bill',
   lastname:'Jones'
  },

  mutations:{
   SET_USER(state, [firstname, lastname]){
    state.firstname = firstname;
    state.lastname = lastname;
   }
  },

  actions:{
   setUser({ commit }, [firstname, lastname]){
    commit('SET_USER', [firstname, lastname]);
   }
  },
}
```
Finally, you then reference your module actions by module / action (`User/setUser`) and in the template reference store.state.module.data (remebering to return store) like so :

```js
<h1>{{ store.state.User.firstname }}</h1>

setup(){
  const myFunc = () => {
    store.dispatch('User/setUser', ['John', 'Doe'])
  }

  return {
    store
  }
}
```
