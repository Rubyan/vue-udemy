# Vue
## key concepts
```
v-once: render something once
v-bind: bind to attribute
v-on: input directive
  v-on:mouseover
  v-on:click
  v-on:keyup example: <input type="text" v-bind:value="counter" v-on:keyup="alertMe">
v-model: 2 way binding

event modifier: addition to directive, like this stop: v-on.keyup.stop
key modifier: bind directive to specific key: v-on.keyup.enter
multiple key modifiers are also possible: v-on.keyup.enter.tab.space
```

```
watch: execute function upon variable change
computed: optimized functions, dynamically calculated. Use them as if they are data properties. data is cached.
methods: functions that run always when a variable has changed

when using a closure you cannot acces the Vue instance,
so pass it using vm = this;
```


## shorthands
```
v-on:click = @click (at click)
v-bind:href = :href (leave v-bind out)
```

## classes
```
use <div class="block" :class="{red: true}"></div> to add class red to div
or use v-model to specify the class directly
<input type=text v-model="color">
<div class="block" :class="color"></div>
you can even specify multiple classes with the array syntax
<div class="block" :class="[color, {red: isRed}]"></div>
```

## inline style

```
<div class="block" :style="{'background-color': color}"></div>
:style => specify style
'background-color' => the property. You can also use backgroundColor without quotes
color => vue property with the color
You can also combine styles:
<div class="block" :style="[myStyle, {height: width + 'px'}]"></div>
```

## jsfiddle links
Getting Started: https://jsfiddle.net/smax/pcjtcmdm/
Template Syntax: https://jsfiddle.net/smax/bkk97b7g/
Events: https://jsfiddle.net/smax/7zdak05g/
Two-Way-Binding: https://jsfiddle.net/smax/ut0tsbcu/
Computed Properties & Watch: https://jsfiddle.net/smax/yLjqxmw0/
Dynamic Classes: https://jsfiddle.net/smax/gowg40ym/
Dynamic Styles: https://jsfiddle.net/smax/3rvdLq5y/

## Conditionals and lists
v-if v-else v-else-if like you expect
use ```<template>``` to group content. The template tag is not rendered itself
use v-show to show/hide with display: none
use v-for="item in items" directive to render lists
use v-for="(item, i) in items" to get the index
you can also do v-for in the template element
Looping is possible through lists and objects:
```
persons: [
  { name: 'max', age: 27 },
  { name: 'anna', age: 26}
]
<li v-for="person in persons>
  <div v-for="(key, value, i) in person">
  {{ key }} - {{ value }}
```
Create a range/list of numbers: ```<v-for="n in 10">```
Use v-bind:key or :key to have Vue update the complete item in the list
  instead of only the position: ``` v-for="person in persons :key=person.id" ```

excercise: https://jsfiddle.net/hx2gb8o0/

## project 1 monster slayer
see project1-monster-slayer

## The vue js instance
multiple instances:
    var vm1 = new Vue({ el: '#app2', data: {'title': 'yo'})
    vm1.title = 'blah'
but you cannot add new properties
```
vm1.$el is the html part of Vue
vm1.$data is the data part of Vue, so you can also pass it
$refs are named elements with special attribute ref:
<button ref="yo" @click="clickButton">
this.$refs.yo.innerText='nice';
note that you change the dom, not the internal Vue template!
vm1.$mount("#app") does the same as the el: "#app" property

components: reusable objects
Vue.component('divname', {template: '<h1>hello</h1>'});
```
## about the virtual dom
vue updates the virtual dom quickly and then only updates the real dom with the diff
```
lifecycle methods (in the root Vue obj)
beforeCreate
create
beforeMount
mounted
beforeUpdate
updated
beforeDestroy
destroyed
```
## development workflow
you can compile an es6 js app into js5 code and serve it pre compiled. nodejs is the package manager and local server
vscode plugin: vetur https://www.youtube.com/watch?v=05tNXJ-Kric

## vue cli
the vue cli allows us to fetch project templates
``` npm install -g vue-cli ```
now you can choose templates:
simple
webpack-simple (start here)
webpack
browserify / browserify-simple

install vue-cli globally: ```sudo npm install -g vue-cli```
create vue app: ```vue init webpack-simple myapp ```

- babel will transpile es6 to es5 so you can run in any browser
- index.html serves the /dist/build.js file that was compiled
the file is bundled as one file
- package.json is for dependencies
- main.js is executed first. Here is the Vue instance created
```
new Vue({
  el: '#app',
  render: h => h(App) // es6 function that renders a vue template
})
```
The single file App.vue structure always has:
- template
- script
- style (optional)

now do cmnd+option+J and open vue dev tools to debug your app

## components
create reusable pieces of code. They shall not have a property named 'data' but a function that returns the data object. Syntax of a global temlate
```
<mydiv></mydiv>

Vue.component('mydiv', {
	data: function() {
  	return {
    	status: 'OK'
      }
  },
  template: '<p>Server status: {{ status }}'
})
```

You can create a source file ie Header.vue with content:
```
<template>
    <div>
        <h1>Server Status</h1>
    </div>
</template>

<script>
export default {
	data: function() {
  	return {
    	my-var: 'OK'
      }
  },
}
</script>
```

Now include in in the main.js like this:
```
import Header from './Header.vue'
Vue.component('my-header', Header)

or

    export default {
        components: {
            appUser: User
        }
    }
```
And use ```<my-header></my-header>``` in the main code

the question is, how to have index go up
how to show certain rows in table with v-show and v-for.
and how can we pass props...
## directory structure
you could place all components in folder 'components', or group files by feature, like 'users' or 'shop'

The native dom is case insensitive. But when using components you could use PascalCase or camelCase.
But you could use app-header and appHeader together. The best practice is to use hypens: app-header in the dom.

## scoped style
By default any style in a component is applied everywhere. Override by using:
```<style scoped></style>```
Vue adds a special attribute like data-v-70b76ce2 to the div and adds the style in the main header section.

## props
used to pass info to components:
call
```
<my-component name='hard-coded'></my-component>
<my-component :name="name"></my-component>

export default {
  props: ['array', 'of', 'properties']
}
```
For validating props you can use objects, array's and the required key like this:
```
props: {
  name: String,
  otherName: [String, Array],
  myProp: {
    type: String,
    required: true
  }
}
You can also use key 'default' for a default value. This can also be a function in case of an object.
```
The prop in the component is a reference (pointer) to an object.

The component can emit events with ```this.$emit('nameWasReset', "Johan");```
You can now listen with ```v-on:nameWasReset="name = $event"``` or with the @ sign: ```@nameWasReset="name = $event"```
The emitted value gets assigned to the name.

It is also possible to pass a callback function to the component as a prop that gets called in the on-click handler.
See example Section Code (Start)

There are several methods to change a prop in a component:
- by emitting an event
- by passing a callback fn
- event bus!

## event bus
create the bus in main.js before the main vue instance:```export const eventBus = new Vue();```
import the bus in the component: ```import { eventBus } from '../main';```
now emit the events on the bus: ``` eventBus.$emit('myEvent', value)```
and listen for events:
```
    created() {
        eventBus.$on('selectServer', (data) => {
            this.data.property = data.property;
        })
    }
```
You can also add functions and data to the event bus because it's just a Vue intance

## exercise server details
- how to show details on sub page: render component on click.
- hoe ga je uit een lijst een geselecteerde server tonen:
1. hou data bij met 'selected server' deze is leeg maar heeft waarde indien server gekozen door gebruiker
dan met v-show toon dit element en zijn functies

- where to store the chosen server
- the data of each server is stored in servers.vue, so should the state of the chosen server also be store there
  - as separate property
  - as property in the details of each server?

- why is this.selected not updated?
- how to reset the state

### solution
until 2:30 ok with Server component
at 5:55 changing id to server
the whole server object is passed as an event
you must handle the events like this to make this.server work:
```
    created() {
        eventBus.$on('selectServer', (server) => {
            this.server = server;
        })
    }
```
on 8:55 he explains this very well.

# Modules / Slots
We want to quote some combination of h2 and p tags, but cannot do this with props. The
solution is a component with the reserved ```<slot></slot>``` syntax. You can use this
if you want to show content like slide shows.  It is called like:

    <app-quote>
    <h2>header</h2>
    <p>data</p>
    </app-quote>

- the child component style is applied, but it can also be applied in the parent
- the parent vars are calulated before passing the data to the child component

## named slots
Give multiple slots their own name

    <!-- parent -->
    <app-quote>
      <h2 slot="title">header</h2>
      <p slot="content">header</p>
    </app-quote>

    <!-- child     -->
    <slot name="title"></slot>
    <slot name="content"></slot>

A slot without a name becomes the default slot.

A named slot without content can show default data: ```<slot name="sub">default</slot>```

## dynamic components
render them like this:

    <component :is="selectedComponent" >
        default content
    </component>

# project 2 quotes
approach:
1. identify the components:
- quote  -> displaying quotes in a nice div with float and margin, and onhover: red
- counter  -> bar that increases from 0-10
- newQuote  -> textbox with Add Quote button to add quotes
- info  -> just a message with info

2. business rules:
- maximum of 10 quotes, after that an popup when adding more
- clicking a quote removes it

3. data store:
- quotes: plain array with strings

4. page layout:
boostrap 4 columns width

The solution of Max is differen on:
- using a quotegrid and slots to render the quote
- using bootstrap panels
- using the given font from the css
- creating divs like: ```div.col-sm-8<tab>```
- using a form and @click.prevent
- using @click.native modifier
- styling the progressbar directly:

    :style="{width: (quoteCount/maxCount) * 100 + '%'}"

# forms
the .lazy modifier make vue submit data when moving away (like blur)
.trim trims the data
.number converts to a number
you can chain the modifiers like v-model.trim.number
show line breaks with ```style="white-space: pre"```
multiple checkboxes: bind them to the same model and make array data property: data[]
for radiobuttons: vue detects this and makes 1 at the time available
dropdown:

    <select
          id="priority"
          class="form-control"
          v-model="selectedPriority">
          <option v-for="priority in priorities">{{ priority }}
          </option>
    </select>
   priorities: ['important', 'normal', 'should have', 'could have', 'Medium'],

custom v-model: use to make your own switch as a component:

    :value = mymodel.property
    @input = "mymodel.property = $event.target.value"

# directives
A v- notation that tells Vue to do something. Each directive has 5 hooks:
- bind: once directive is attached
- inserted: in dom
- update: once component is updated (without children)
- componentUpdated: like above but with children
- unbind: once removed

```
main.js:
Vue.directive('color', {
  bind(el, binding, vnode) {
    // <p v-color:background="'red'">whoo</p>
    console.log('got arg ' + binding.arg) // background
    console.log('got value ' + binding.value) // red
    el.style.backgroundColor = binding.value
  }
})
```

// v-mydirective:arg=value.modifier

# filters and mixins
you have to create them yourself, and use them with a pipe |

global filter. in main.js:
```
Vue.filter('to-lowercase', function(value) {
  return value.toLowerCase();
})
```
local filter:
```
script:
filters: {
  to-uppercase(value) {
    return value.toUpperCase();
  }
}

template with changed filters:
<p>{{ text | toUppercase | to-lowercase }}
```

## filter alternative: computed properties
Vue only recalculates if neccesary
```
<input v-model="filterText">
<ul>
  <li v-for="fruit in filteredFruits>{{ fruit }}</li>
</ul>

data() {
  return {
    fruits: ['apple', 'banana', 'mango', 'melon'],
    filterText: ''
  }
},
computed: {
  filteredFruits() {
    return this.fruits.filter((element) => {
      return element.match(this.filterText);
    });
  }
}
```

## mixins
use to prevent code duplication
the mixin is merged into the other code
other code remains intact
has lifecycle hooks
order: mixin, component

create fruitMixin.js
```
export const reverseMixin = {

    data(){
```

include:
```
<script>
import { fruitMixin } from './fruitMixin';
export default {
  mixins: [fruitMixin]
}
</script>
```

warning: a global mixin is added in every instance:
``` Vue.mixin({ created() { console.log('created')}}) ```
typically use in development

if you change data in a mixin it is only updated in the component,ie it is not shared with other parts

## transitions
animate one element with v-if or v-show in transition block
Vue will analyze the css for you for duration
```
<transition name="fade" mode="out-in">
  <p v-if="show>foobar</p>
</transition>

classes attached (with example):
* = fade (see name)
1. *-enter         opacity: 0;
2. *-enter-active  transition: opacity 1s;
3. *-leave         /* opacity: 1; */
4. *-leave-active  transition: opacity 1s; opacity: 0;

There are six classes applied for enter/leave transitions.

v-enter: Starting state for enter. Added before element is inserted, removed one frame after element is inserted.

v-enter-active: Active state for enter. Applied during the entire entering phase. Added before element is inserted, removed when transition/animation finishes. This class can be used to define the duration, delay and easing curve for the entering transition.

v-enter-to: Only available in versions 2.1.8+. Ending state for enter. Added one frame after element is inserted (at the same time v-enter is removed), removed when transition/animation finishes.

v-leave: Starting state for leave. Added immediately when a leaving transition is triggered, removed after one frame.

v-leave-active: Active state for leave. Applied during the entire leaving phase. Added immediately when leave transition is triggered, removed when the transition/animation finishes. This class can be used to define the duration, delay and easing curve for the leaving transition.

v-leave-to: Only available in versions 2.1.8+. Ending state for leave. Added one frame after a leaving transition is triggered (at the same time v-leave is removed), removed when the transition/animation finishes.

There is also transition mode:
out-in: Current element transitions out first, then when complete, the new element transitions in.

```

## animations
slide example with @keyframes from to and transform: translateY
you could add type="animation" to the transition so Vue will wait
Example
```

.flip-enter-active {
    animation: flip-in 0.5s ease-out forwards;
}

.flip-leave-active {
    animation: flip-out 0.5s ease-out forwards;
}

@keyframes flip-out {
    from {
        transform: rotateY(0deg);
    }
    to {
        transform: rotateY(90deg)
    }
}

@keyframes flip-in {
    from {
        transform: rotateY(90deg);
    }
    to {
        transform: rotateY(0deg)
    }
}
```

another example with slide:
```
.slide-enter-active {
    animation: slide-in 0.15s ease-out forwards;
}
.slide-leave-active {
  animation: slide-out 0.15s ease-out forwards;
}

@keyframes slide-in {
    from {
        transform: translateY(-30px);
        opacity: 0;
    }
    to {
        transform: translateY(0);
        opacity: 1;
    }
}
@keyframes slide-out {
    from {
        transform: translateY(0);
        opacity: 1;
    }
    to {
        transform: translateY(-30px);
        opacity: 0;
    }
}

```

### on load animation
use ```<transition name="yo" appear>```

### animate.css
specify the classes you wnat to use, but remove empty classes
```
<transition
  appear
  <!-- enter-class="" -->
  enter-active-class="animated bounce"
  <!-- leave-class="" -->
  leave-active-class="animated shake"
>
```

## fading between two animations
add a unique key to the div in the transitions:
```
<transition mode="out-in">
  <div key="uniqueName2"></div>
  <div key="uniqueName2"></div>
</transition>
```

## animate with javascript
you can use the transition JS hooks before-after-enter-leave
``` html
<button class="btn btn-primary" @click="load = !load">Load/remove element</button>

<transition
  @before-enter="beforeEnter"
  @enter="enter"
  @after-enter="afterEnter"
  @enter-cancelled="enterCancelled"

  @before-leave=... etc
  :css="false"
>
<div style="width: 100px; height: 100px; background-color: red" v-if="load"></div>
</transition>
methods: {
  beforeEnter(el) {
    console.log('beforeEnter');
    this.elementWidth = 100;
    el.style.width = this.elementWidth + 'px';
  },
  enter(el, done) {
    console.log('enter');
    let round = 1;
    const interval = setInterval(() => {
      el.style.width = (this.elementWidth + round * 10) + 'px';
      round++;
      if (round > 10) {
        clearInterval(interval);
        done();
      }
    }, 20)

  },
  leave(el, done) {
    console.log('leave');
    // done is a function that needs to be called to inform Vue
    done();
  },
  // etc

}
```
you can also use components with :is

## transition groups
https://www.udemy.com/course/vuejs-2-the-complete-guide/learn/lecture/5975866#overview


dynamic list with add/remove
```
  <ul class="list-group">
    <li v-for="(number, index) in numbers"
      :key="index"
      class="list-group-item"
      @click="removeItem(index)"
      style="cursor: pointer"
      >{{number}}</li>
  </ul>

  addItem(){
    this.numbers.push(1);
  },
  removeItem(index) {
    this.numbers.splice(index, 1);
  },
```
The transition group brings a new property: *-move

# exercise super quiz
animation: shrink width to 0, then remove

code analysis:
2 components
## component question
question has 4 buttons, calling onAnswer(btnData[0].correct)
fn genQuestion creates 2 random digits 1-100 and a mode ADD/SUB
the buttondata is populated with 4 wrong answers
one button is overwritten with proper answer
the boolean 'isCorrect' is emitted to App.vue that renders a component with mode app-question or app-answer

## component answer
button w method onNextQuestion, emitting 'confirmed' so mode becomes app-question

# HTTP
use package vue-resource https://github.com/pagekit/vue-resource
```npm install --save vue-resource```
main.js:
```
import VueResource from 'vue-resource';
Vue.use(VueResource);
```
To use a resource: ```this.$http ```
Now use Firebase as backend: Cloud service with database and authentication functionality
Create a new firebase project: vuejs-http and create a realtime database
Create vue app to post user object
Get the firebase link from the firebase project page
Get/Post example to firebase:
```
 methods: {
    fetchData() {
        this.$http.get('https://vuejs-http-342ed.firebaseio.com/data.json')
            .then(response => {
                // response.json() is a promise
                return response.json();
            })
            .then(data => {
                console.log(data);
                const resultArray = [];
                for (const key in data) {
                    resultArray.push(data[key])
                }
                console.log(resultArray);
                this.users = resultArray;
            });
    },
    submit() {
        console.log(this.user)
        this.$http.post('https://vuejs-http-342ed.firebaseio.com/data.json', this.user)
            .then(response => {
                console.log(response);
            }, error => {
                console.log(error);
            });
    }
}
```
Also consider setting the root url in main.js:
```Vue.http.options.root = "https://vuejs-http-342ed.firebaseio.com/data.json"```

## intercepting data
in main.js:
```
// array of functions to apply on each request
Vue.http.interceptors.push((request, next) => {
  console.log(request);
  if (request.method == 'POST') {
    request.method = 'PUT';
  }
  next();
});
```
## resources
https://github.com/pagekit/vue-resource/blob/develop/docs/resource.md

# routing
install the router:
```npm install --save vue-router```
routes.js: create some routes
```
import User from './components/user/User.vue';
import Home from './components/Home.vue';

export const routes = [
    { path: '', component: Home },
    { path: '/user', component: User },
];
```
main.js: import the router
```
import Vue from 'vue'
import VueRouter from 'vue-router';
import App from './App.vue'
import { routes } from './routes';

Vue.use(VueRouter);

const router = new VueRouter({
  routes,
  mode: 'history' // default mode is hash
})

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})

```
App.vue: render the route
```
    <router-view>
      <!-- the components are rendered here automatically -->
    </router-view>
```
with mode 'history' you need to reconfigure the server

showing active links:
```
<template>
    <ul class="nav nav-pills">
        <router-link to="/" tag="li" active-class="active" exact><a>Home</a></router-link>
        <router-link to="/user" tag="li" active-class="active" ><a>User</a></router-link>
    </ul>
</template>
```
navigate with js in a method:
```
    this.$router.push({path: '/'});
```
routes with params:
```
{ path: '/user/:id', component: User }, // /user/123
<router-link to="/user/10" tag="li" active-class="active" ><a>User</a></router-link>
```
watching route for changes:
```
    watch: {
        '$route' (to, from) {
            this.id = to.params.id;
        }
    },
```
In Vue2.2 you can use props: https://github.com/vuejs/vue-router/tree/dev/examples/route-props

named routes:
```
{ path: ':id/edit', component: UserEdit, name: 'userEdit' },
<router-link :to="{ name: 'userEdit', params: { id: $route.params.id }}" ><a>edit</a></router-link>
```

query params:

    <router-link
        :to="{
            name: 'userEdit',
            params: { id: $route.params.id },
            query: { locale: 'en', q: 100}
        }"
        class="btn btn-primary"
        >edit
    </router-link>
    Locale: {{ $route.query.locale }}

redirects:

    { path: '/redirect-me', redirect: '/user' }
    { path: '/redirect-me', redirect: {name: 'home'} }
    { path: '*', redirect: { name: 'home'}} // catch all

transitions:
just wrap the router-view in a transition

anchor / hash property: scroll user to anchor tag on page:
```
    <router-link
        :to="{
            name: 'userEdit',
            params: {
                id: $route.params.id
            },
            query: {
                locale: 'en',
                q: 100
            },
            hash: '#data'
        }"
        class="btn btn-primary"
        >edit
    </router-link>
```

main.js:
```
const router = new VueRouter({
  routes,
  mode: 'history',
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition;
    }
    if (to.hash) {
      return { selector: to.hash };
    }
    //return {x: 0, y: 700};
  }
})
```

intercepting routes. You can control if a user can visit a link.
This can be done in main.js,  routes.js, or in
a component in function beforeRouteEnter(to, from, next) { next();}
```
        { path: ':id', component: UserDetail, beforeEnter: (to, from, next) => {
            console.log('inside routes.js');
            next();
        } },
```
You must call always next(). A component will not even load if next() is not called.

Lazy loading in routes. Google lazy loading + webpack. Only load part of the application when we need it.
```
const User = resolve => {
    require.ensure(['./components/user/User.vue'], () => {
        resolve(require('./components/user/User.vue'))
    })
};
```

Visual studio multiline edit tip: press ALT and select multiple cursor locations.

### lazy loading in vue cli 3
```
const User = () => import('./components/user/User.vue');
...
export const routes = [
    ...,
    {
        path: '/user', components: {
            default: User,
            ...
        }
    }
];
```

# vuex
see project lesson-vuex
centralized state management, when props or the event bus is not enough.
Key is that the state of objects is stored on a centralized location.

central store: holds the state

concepts: state, getters, mutations, actions

install vuex:
```npm install --save vuex```
maak dan een store.js aan:
```
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export const store = new Vuex.Store({
    state: {
        funds: 10000
    }
});
```
change main.js:
```
import Vue from 'vue'
import App from './App.vue'

import { store } from './store/store';

new Vue({
  el: '#app',
  store,
  render: h => h(App)
})
```

Use getters to get data and perform calculations: TODO CHECK
```
    getters: {
        funds: state => {
            return state.funds;
        }
    }
```
Useage in the view:
```
    computed: {
        counter() {
            return this.$store.getters.funds;
        }
    }
```

You can use ...mapGetters([]) to map them automatically, but then you cannot use own properties. to fix this, use the spread operator (...):
```
        computed: {
            ...mapGetters([
                'doubleCounter',
                'stringCounter'
            ])
        }
```
This will however not compile with the current babel, so install this:
```npm install --save-dev babel-preset-stage-2```
finally, change the file .babelrc by adding:
```
    ["stage-2"]
```
### mutations
Use mutations to change the state, like getters get the state.
A mutation will change the state, and the components will get the new state. Creater mutations in the vuex store, then call them using import { mapMutations }  and then ...mapMutations

Mutations should run synchronous, so do not use setTimeout() in them.
in the store:
```
    mutations: {
        buy: (state, payload) => {
            state.funds -= payload;
        }
    }
```
in the component:
```
<script>
    import { mapActions } from 'vuex';
    export default {
        methods: {
            ...mapActions([
                'asyncIncrement',
                'asyncDecrement'
            ])
        }
    }
</script>
```
### actions
Extra function where you may run async tasks, and commit the results. You can use setTimeout here.
```
    import { mapActions } from 'vuex';
    export default {
        methods: {
            ...mapActions([
                'increment',
                'decrement'
            ])
        }
    }
```

### summary
Vuex uses state, getters, mutations, actions. Actions commit mutations. Getters return state.
The components dispatch actions.

## folder structure
create 'modules' folder with counter.js etc
also create separate files actions.js, getters.js, mutations.js
actions.js:
```
export const updateValue = ({ commit }, payload) => {
    commit('updateValue', payload);
};
```
store.js:
```
import * as actions from './actions';

export const store = new Vuex.Store({
    state: {
        value: 0
    },
    actions,
    modules: {
        counter
    }
});
```
## namespaces
Since all these files are merged together do not use the same name for the functions.

# Project 3 Stock trader
main screen:
```
menu:
  Stock Trader
  Portfolio
  Stocks

  End Day
  Save and Load >
    Save Data -> save to db
    Load Data
  Funds: funds (start w $10.000)

md12 box with instructions and funds
```

stocks
```
4x stock component, md6 each. with green header with name and price, input quantity and buy button
after buy, the funds are calculated and the input field is cleared
bwm price 110, buy 10, funds are 8900
```

portfolio
```
show the bought stocks: name, price, quantity: BMW (price: 110|quantity: 10)
with inputbox and sell button
```

end day button:
the sell box changes
BMW price: 75|quantity: 10

stocks page:
all stock have new prices. buy again 10 BMW, funds are $8.150

Portfolio page:
BMW price: 75|quantity: 20

end day button multiple times
BWM price: 79, 81
sell 20, funds 9770

### Issues
+ navbar not working. cause: example bootstrap4, css bootstrap 3
+ navbar dropdown not working. cause no js included
+ active home link always active, cause: did not use 'exact' in link
+ routes added without problems
+ adding vuex
how to add stocks from state.stocks to portfolio
1. add portfolio counter to stocks
2. create separate portfolio storage with join table
+ big issue: access the store inside the store: just use store.fn instead of this.$store.fn

### Solutions
add spread operator
create components
install vue-router
create routes
add router-view
build header component
copy navbar from bootstrap
create stocks with dummy stocks

### differences
max puts the router-view inside div.row and div.col-xs-12
input type=number
pull-left / pull-right for alignment
use buy button instead of link
add to button: :disabled="qty <= 0 || !Number.isInteger(qty)
the mutations are capital strings
stocks are in a separatate vuex module
import data file: ``` import stocks from '../../data/stocks'; ```
stocks.js:
```
export default [
  { id: 1, name: 'xyz'}
];
```
the portfolio is a separate module with its own stocks[] and funds
the finder is different:
```
const record = state.stocks.find(e => e.id == stock.id)
```
dropdown menu:  either use bootstrap or create your own:
```
// create property
isDropdownOpen: false;
// dynamically add 'show' class to dropdown after click
@click="isDropdownOpen = !isDropdownOpen"
:class="{open: isDropdownOpen}"

example:
        <li class="nav-item dropdown"
            :class="{show: isDropdownOpen}"
            @click="isDropdownOpen = !isDropdownOpen"
            >
          <a
            class="nav-link dropdown-toggle"
            href="#"
            id="navbarDropdown"
            role="button"
            data-toggle="dropdown"
            aria-haspopup="true"
            aria-expanded="false"
          >Save & Load</a>
          <div class="dropdown-menu" :class="{show: isDropdownOpen}" aria-labelledby="navbarDropdown">
            <a @click="saveData" class="dropdown-item" href="#">Save Data</a>
            <a @click="loadData" class="dropdown-item" href="#">Load Data</a>
          </div>
        </li>
```
vue-resource: use root url option
save data: do not use vuex. in navbar/header: create object to save data:
```
const data = { funds: this.$store.getters.funds, etc}
```
the load data should be in an action because async

# deployment
create aws account
create s3 bucket and allow all permissions
enable static webhosting, both index.html
google 'aws bucket policy static website' and copy the bucket policy
example:
```
 {
  "Version":"2012-10-17",
  "Statement":[{
	"Sid":"PublicReadGetObject",
        "Effect":"Allow",
	  "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::vuejs-stocktrader-001/*"
      ]
    }
  ]
}
```
now upload index.html and dist/build.js

# Axios
axios is an alternative to vue-resource
install:
``` npm install --save axios

import axios from 'axios';
  axios.post(
    'https://vuejs-axios-bfafd.firebaseio.com/users.json',
    formData)
  .then(res => console.log(res))
  .catch(error => console.log(error))

  axios.get('https://vuejs-axios-bfafd.firebaseio.com/users.json')
    .then(res => console.log(res))
    .catch(error => console.log(error))
  }

converting js obj to arr:
          const data = res.data
          const users = []
          for (const key in data) {
            const user = data[key]
            user.id = key
            users.push(user)
          }
          console.log(users)
  ```
setting baseURL in main.js
```
import axios from 'axios';
axios.defaults.baseURL = 'https://vuejs-axios-bfafd.firebaseio.com/'
axios.defaults.headers.common['Authorization'] = 'yo test'
axios.defaults.headers.get['Accepts'] = 'application/json'

```
### interceptors
function executed on every request
```
// middleware, add interceptor
const reqInterceptor = axios.interceptors.request.use(config => {
  console.log('Request interceptor', config)
  config.headers['XYZ'] = 'b'
  return config
})

axios.interceptors.response.use(res => {
  console.log('Response interceptor', res)
  return res
})

// remove interceptor
axios.interceptors.request.eject(reqInterceptor)
```
### custom axios instances
create axios-auth.js
```
import axios from 'axios'
const instance = axios.create({
    baseURL: 'https://vuejs-axios-bfafd.firebaseio.com'
})

instance.defaults.headers.common['ABCD'] = 'ABCD'

export default instance
```
component.vue:
```
import axios from '../../axios-auth';
```
# authentication
send username+pass
receive token
store token in localStorage
on next requests, always add token
firebase auth: enable email login
firebase database rules:
```
{
  "rules": {
    ".read": "auth != null",
    ".write": true
  }
}
```
now google for 'firebase auth rest api' and look for email login
copy the web key from the project page
post to the proper url:
```
baseURL: 'https://identitytoolkit.googleapis.com/v1'
endpoint: ''/accounts:signUp?key=abcdlongkey'
```
### protect a route:
in routes.js:
add store ```import store from '@/store'```, then
```
  {
    path: '/dashboard',
    component: DashboardPage,
    beforeEnter (to, from, next) {
      if (store.state.idToken) {
        next()
      } else {
        next('/signin')
      }
    }
  }
  ```

### logout
store.js
```
  import router from './router'
  actions: {
      logout({commit}) {
        commit('logout')
        router.replace('/signin')
      }
  }
```

# Vue CLI 3
install vue cli 3:
```
sudo npm install -g @vue/cli
```
this will change the vue-init binary!
now create a project:
on error, remove node_modules directory in your home dir
```
vue create <project-name>
  now use preset or use custom config
  custom config:
  - add plugins
    - pwa
    - eslint
  - save as preset
now develop and build
add plugins later is now possible!
```
the config is stored in ~/.vuerc
now start the server:
``` npm run serve ```
files in src/assets will be optimized by webpack
## plugins
naming conventions: ```vue-cli-plugin-<name>```
for example name=vuetify
now install in command line using:
```vue add vuetify```
it asks you for options
you still can install axios the normal way npm install --save axios

## css / sass
if you change from css to sass:
```
<style scoped lang="scss">
/* sass syntax */
footer {
  small {
    color: red;
  }
}
</style>
```
now you can have an error because a sass-loader could not be found
run:
```
npm install --save sass-loader node-sass
```
## env vars
Create a .env file in the root with key value pairs and prefix VUE_APP
``` VUE_APP_MYVAR=value```
now reference it in data:
```url: process.env.VUE_APP_MYVAR,```
also restart the dev server
default env files:
```
.env
.env.development
.env.test
.env.production (for npm run build)
```
they overwrite the default .env file

## building the app
building uses ue-cli-service that is installed automatically locally
first edit the package.json file
look for "scripts" and change to:
```
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "build:development": "vue-cli-service build --mode development"
  }
 ```
 save and run on the cli:
 ``` npm run build:development ```
 check the dist folder for the results

## instant prototyping
install global cli-service:
``` npm install -g @vue/cli-service-global ```
now you can run build on a global level, but also run a simple vue component! Create a component:
```
<template>
<h1>Hello!</h1>
</template>
```
and run: ``` vue serve Hello.vue ```
## different build targets
there are 3 ways:
```
1. build as vue app:
   vue build [--target app]
   includes normal vue app bundle, contains code+framework
2. build as library:
   vue build --target lib
   creates a vue library bundle for usage in vue
3. build as web component:
   vue build --target wc
   this is a custom html element that you can use without vue!
   it is a reusable web component as a javascript file
   can be dumped into any page where you import vue
```
Example with Hello.vue:
``` vue build Hello.vue --target app ```
now install a global webserver
```
npm install -g http-server
cd dist
http-server
```
it will pick up the index.html and server the app

Now lets build a lib:
``` vue build Hello.vue --target lib ```
with the ouput in folder dist you could create an npm package.

And finally the web component
``` vue build Hello.vue --target wc --name my-greeting ```
now you can use in ANY web app:
```
<meta charset="utf-8">
<title>my-greeting demo</title>
<script src="https://unpkg.com/vue"></script>
<script src="./my-greeting.js"></script>

<my-greeting></my-greeting>
```

## using the old templates
vue init will not work. to solve:
``` npm install -g @vue/cli-init ```
you can now run again
``` vue init webpack-simple my-old-project ```

## using the GUI
run ```vue ui``` and follow along!

# Meetup project
This project is available on Youtube but is uses vuetify 1.5.
Make sure to read the upgrade guide: https://vuetifyjs.com/en/getting-started/releases-and-migrations
The course: https://www.youtube.com/watch?v=at6QjSwKOuA&list=PL55RiY5tL51qxUbODJG9cgrsVd7ZHbPrt&index=6
```
vue create yt-devmeetup
//choose router, standard config, es lint
//now add the vuetify plugin:
vue add vuetify
npm run serve
```
### visibility
https://vuetifyjs.com/en/styles/display#visibility
Visible on small devices only:
``` class="d-flex d-sm-none" ```
Hidden on small devices only:
``` class="d-none d-sm-flex" ```
example:
```
    <v-app-bar>
      <v-toolbar >
        <v-app-bar-nav-icon class="d-flex d-sm-none" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <v-toolbar-title>DevMeetup</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items class="d-none d-sm-flex">
          <v-btn text to='/test'>
            <v-icon left>mdi-account-supervisor</v-icon>
            View Meetups
          </v-btn>
        </v-toolbar-items>
      </v-toolbar>
    </v-app-bar>
```
colors: google material-colors, for example:
```<v-toolbar class="purple darken-1">```
with v-toolbar dark you can make the text white

### links
button links without router-link:
```
<v-btn
  v-for="item in menuItems"
  :key=item.title
  text
  router
  :to="item.link"
  >
  <v-icon left>{{ item.icon }}</v-icon>
  {{ item.title }}
</v-btn>
```

### vuetify working layout:
```
<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      app
      temporary
    >
      <v-list  dense>
        <v-list-item
          v-for="item in menuItems"
          :key=item.title
          router
          :to="item.link"
          >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
    >
      <v-toolbar>
        <v-app-bar-nav-icon
          class="d-flex d-sm-none"
          @click.stop="drawer = !drawer">
        </v-app-bar-nav-icon>
        <v-toolbar-title>
          <router-link to='/' tag='span' style="cursor: pointer">DevMeetup</router-link>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items class="d-none d-sm-flex">
          <v-btn
            v-for="item in menuItems"
            :key=item.title
            text
            router
            :to="item.link"
            >
            <v-icon left>{{ item.icon }}</v-icon>
            {{ item.title }}
          </v-btn>
        </v-toolbar-items>
      </v-toolbar>
    </v-app-bar>

    <v-content>
      <v-container

      >
        <router-view></router-view>
      </v-container>
    </v-content>

  </v-app>
</template>

<script>
```

### Home component
row layout, should wrap
inside are v-flex items with xs12 sm6

vuetify 1.5: v-container, v-layout, v-flex, v-spacer

vuetify 2.x: v-container, v-row, v-col, v-spacer

md6 xs12 etc:

pull buttons left and right with:
```
<p class="text-left">Left aligned text on all viewport sizes.</p>  <p class="text-sm-left">Left aligned text on SM (small) or wider.</p>
```

to add margin below:
```<v-row class="mb-2">```

### updating vue, vuetify
update the version in package.json and run ```npm install```

the 'router' tag is not needed anymore

### meetup component

Meetups.vue:
```
<template>
  <v-container>
    <v-row>
      <v-col>
        <p>hello world</p>
      </v-col>
    </v-row>
  </v-container>
</template>

```
### sizing
change the number of columns depending on the screen size
```
<v-col cols="12"> // default 12 columns (which is also default)
<v-col sm="6"> // on xs, use 12. on small AND greater, use 6 columns
so use sm(600-960px), md(-1264), lg(-1904), xl(1904+)
also use offset-md-1 to offset 1 on md screens
```
### meetup
since Vuetify 2 is different than 1, we will follow the docs now for styling. Push items to the right with v-spacer. This is a nice card:
```
```

# Adding vuex
```
1. vue add vuex
2. vue ui
```

### simple router to link
```<v-btn :to="'/meetups/' + meetup.id" text>```

### some shortcuts in vscode
```
CMD+P,@ -> show file structure and functions in a file
CMD-K Z -> zen mode

### meetups/id route
the id is passed to the component and can be used in the getter
component:
```
export default {
  props: ['id'],
  computed: {
    meetup () {
      return this.$store.getters.loadedMeetup(this.id)
    }
  }
}
```
routes.js:
```
  {
    path: '/meetups/:id',
    name: 'Meetup',
    props: true,
    component: Meetup
  },
```
store.js getter:
```
    loadedMeetup (state) {
      console.log('getters: loadedMeetup')
      return (meetupId) => {
        return state.loadedMeetups.find((meetup) => {
          console.log('trying id', meetup.id)
          return meetup.id === meetupId
        })
      }
    }
```

### new meetup
uses forms with validation

# Firebase SDK
copy the web code from the project page
main.js:
```
import * as firebase from 'firebase'`
  created () {
    firebase.initializeApp({
      apiKey: '',
      authDomain: '',
      databaseURL: '',
      projectId: '',
      storageBucket: '',
      messagingSenderId: '',
      appId: ''
    })
  }
```

### track login / loading state
add state loading, authError to vuex so you can show progress
create component with v-alert, closable, emitting onClose
include component in signin / signup page
show component with v-if="error"
onDismissed dispatch the clear error action

### store data in firebase
plan
create collection (meetups)
```
      var db = firebase.firestore()

      db.collection('meetups').add({
        title: meetup.title,
        location: meetup.location,
        imageUrl: meetup.imageUrl,
        description: meetup.description,
        date: meetup.date
      })
        .then(
          docRef => {
            console.log('Added meetup with id', docRef.id)
            meetup.id = docRef.id
            commit('createMeetup', meetup)
          })
        .catch(
          error => {
            console.log('Error adding meetup:', error)
          })
```
questions: where to initialize the db
when to load the meetups => created hook in main.js

now add a loader to the caroussel:
pick a loader animation:
```
<v-progress-circular
  indeterminate
  color="primary"
></v-progress-circular>
```
it is tracked by the state.loading

### log in automatically in firebase
set up auth listener in main.js:
```
    firebase.auth().onAuthStateChanged(user => {
      if (user) {
        // User is signed in.
        console.log('onAuthStateChanged: found user', user)
        const LoggedInUser = {
          id: user.uid,
          registeredMeetups: []
        }
        this.$store.commit('setUser', LoggedInUser)
      } else {
        // No user is signed in.
        console.log('onAuthStateChanged no user found')
      }
    })
```

### upload images
use <v-file-input>
store as b64 in a text field for preview
store binary as image
upload all to firebase
get the id back of the meetup
use the id in the image and associate with the image
finally get the url of the image
now update the meetup in firebase with the url

add doc to firebase:
```
var db = firebase.firestore()
db.collection('meetups').add(meetup)
  .then(docRef => {
    // get the id from firebase
    id = docRef.id
    console.log('Added meetup with id', id)
    return id // this id is returned and available in the next .then()
  })
```

update document in firebase with new field
```
  const docRef = db.collection('meetups').doc(id)
  console.log('with docref', docRef)

  docRef.update({
    imageUrl: imageUrl
  }).then(() => {
    console.log('updated success')
  })
```

show edit button if user is owner:
```
  <v-card-title>
    <h3 class="primary--text">{{ meetup.title }}</h3>
    <template v-if="meetup.creatorId == user.id">
      <v-spacer></v-spacer>
      <app-edit-meetup-details-dialog></app-edit-meetup-details-dialog>
    </template>
  </v-card-title>
```