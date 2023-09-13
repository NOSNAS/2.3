<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref, watch } from 'vue';
import { reactive, computed } from 'vue'
import TodoItem from './component.vue'
import { defineProps } from 'vue';
import FancyButton from './views/prop.vue'

//templete syntax
const message = 'Hello world';
const message1 = 'hello world'
const dynamicID = 'id'
const objectOFattrs = {
  id: dynamicID,
  class:'container'
};
//method, ref, setup
const count = ref(0)
function method_add() {
  count.value++
}

//cpmputed
const author = reactive({
  name: 'kazuo ishiguro',
  books: [
    'Vue 2 - never let me go',
    'Vue 3 - mystery of math',
    'Vue 4 - holly from hell'
  ]
})

const ContainBook = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})

//class and style
const isActive = ref(true);
const activeColor = ref('pink')
const fontSize = ref(30)

//list rendering 
const itemlist = reactive({
  heading: 'basic fundamental of math',
  author: 'Josh Smith',
  date: '2022-10-10'
})

const list = ref([
  { name: 'Task 1', isComplete: false },
  { name: 'Task 2', isComplete: true },
  { name: 'Task 3', isComplete: false },
]);

const list_of_things = ref('')
const todos = ref([])

let listid = 4

function component() {
  todos.value.push({
    id: listid++,
    title: list_of_things.value
  })
  list_of_things.value = ''
}


//Event Handling
const counter = ref(5)

const name = ref('Vue.js')

function method_handler(method) {
  alert(`Hello ${name.value}!`)
  // `event` is the native DOM event
  if (method) {
    alert(method.target.tagName)
  }
}

//input binding
const check_box = ref(true)

const value = ref('select a value')

const options = ref('')

const file = ref('')

const age = ref(0); 

//watcher
const question = ref('')
const answer = ref('insert your question ')

watch(question, async (question) => {
  if (question.indexOf('?') > -1) {
    answer.value = 'finding the answer..'
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'sorry answer not find. ' + error
    }
  }
})

//component
const props = defineProps(['foo']);
const receivedProp = props.foo;


</script>


<template>

   <!--template syntax-->
    <p>--templete syntax--</p>
    <div>template code</div>
    <div>{{ message }}</div>
    <div v-html="message1"></div>
    <div v-bind:id="dynamicID">the dynamicID</div>
    <div>{{1 + 1}}</div>
    <div>{{ message ? 'yes' : 'no' }}</div>
    <!--methods-->
    <P>--method,ref,setup--</P>
    <button @click="method_add">{{ count }}</button>
    <!--computed-->
    <p>--computed--</p>
    <p>Test lf Kazuro Ishiguro have published books:</p>
    <span>{{ ContainBook }}</span>
    <!--classes and style-->
    <p>--classes and style--</p>
    <div :class="{pink:true}">Component will be pink if true</div>
    <div :class="{active: isActive}">the class is active?</div>
    <div :style="{ color: activeColor, fontSize: fontSize + 'px' }">STYLE</div>
    <!--list rendering-->
    <p>--list rendering--</p>
    <p>*object*</p>
    	<ul>
        <li v-for="(value, key, index) in itemlist">
		    {{ index }}. {{ key }}: {{ value }}
		    </li>
      </ul>
    <p>*range*</p>
    <span v-for="n in 5">{{ n }}</span>
    <p>*template and v-if*</p>
    <ul>
      <template v-for="todo in list">
        <li v-if="!todo.isComplete">
          {{ todo.name }}
        </li>
      </template>
    </ul>

    <p>*component*</p>
    <form v-on:submit.prevent="component">
    <label for="new-todo">Add things need to be done</label>
    <input
      v-model="list_of_things"
      id="new-todo"
    />
    <button>Add</button>
  </form>
  <ul>
    <todo-item
      v-for="(things, index) in todos"
      :key="things.id"
      :title="things.title"
      @remove="things.splice(index, 1)"
    ></todo-item>
  </ul>

  <!--Event Handling-->
  <p>--Event Handling--</p>
	<p>click to add more numbers:( {{ counter }} )</p>
  <button @click="counter++">Add number</button>
  <br>
	<button @click="method_handler">good morning</button>

  <!--Form Input Bindings-->
  <p>--Form Input Bindings--</p>
  <p>please insert the message: {{ the_message }}</p>
  <input v-model="the_message" placeholder="input" />

	<input type="checkbox" id="checkbox" v-model="check_box" />
	<label for="checkbox">{{ check_box }}</label>

  <div>VALUE: {{ value }}</div>
  <input type="radio" id="python" value="python" v-model="value" />
  <label for="python">python</label>

  <input type="radio" id="java" value="java" v-model="value" />
  <label for="java">java</label>
  <br>

  <span> OPTIONS: {{ options }}</span>
  <select v-model="options">
    <option disabled value="">choose a option</option>
    <option>Apple</option>
    <option>Banana</option>
    <option>Cherry</option>
  </select>
  <br>

  <span>write a paragraph:</span>
	<p style="white-space: pre-line;">{{ file }}</p>
	<textarea v-model="file" ></textarea>
  <br>
  
  <p>*v-model modifiers*</p>
  <input v-model.lazy="msg" />
  <p>Message that syncs the input with the data: {{ msg }}</p>
  <input v-model.number="age" />
  <p>Message which automatically typecast as a number: {{ age }}</p>
  <input v-model.trim="gsm" />
  <p>Message can be trimmed automatically: {{ gsm }}</p>

  <!--watcher-->
  <p>watcher</p>
  <p>
    Ask a question and inser a question mark at the end:
    <input v-model="question" />
  </p>
  <p>{{ answer }}</p>

  <!--components-->
  <p>--components--</p>
  <p>*props*</p>
  <div>
    <p>Received prop value: {{ receivedProp }}</p>
  </div>
  <p>*events $emit has included in list_of_things*</p>

  <p>*using slot*</p>
  <FancyButton>
    TEST <!-- slot content -->
 	</FancyButton>






  <nav>
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/about">About</RouterLink>
  </nav>

  <RouterView />
</template>

<style scoped>

.pink{
  background-color: pink;
  width:200px;
  padding: 10px;
  color: #fff;
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
