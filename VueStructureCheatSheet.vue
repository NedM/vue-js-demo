// Vue structure

<script>
import thingComponent from './path/to/thingComponent';

// Declare global variables
let count = 0; 

export default {
	components: {
		thingComponent,
	},
	computed: {
		// Cached computations resulting from other data values updating
		// See https://vuejs.org/guide/essentials/computed.html
	},
	data: function() {  // shorthand: "data()"
		return {
			value_1: 'something',
			counter: 0,
			greeting: 'Greetings',
			newGreeting: '',
			name: 'world',
			status: 200,
			responseContent: '',
			waitTime: 500
		}
	},
	methods: {
		// Non-cached computations
		async fetchData() {
			const headers = { 
				'Accept': 'application/json', 
				'Content-Type': 'application/json'
			}
	      	const init = { method: 'GET', headers: headers }
			const url = `https://httpstat.us/${this.status + this.counter}?sleep=${this.waitTime}`
			const response = await fetch(url, init)
			this.responseContent = await response.json()
		},
		updateGreeting() {
			this.greeting = this.newGreeting
			this.newGreeting = ''
		},
		updateName(event) {
			const form = event.target
			const formData = new FormData(form)
			console.log(`values: ${[...formData.values()]}`) // console.log(`values: ${Array.from(formData.values())}`)
			const newName = formData.get('nameInput')
			this.name = newName
			form.elements['nameInputField'].value = ''
		}
	},
	mounted() {
		// Lifecycle method. See https://vuejs.org/guide/essentials/lifecycle.html
		// Do something when the components mounts
		console.log('All mounted up, boss.')
		this.fetchData()
	},
	watch: {
		// Methods called when the value of the corresponding data value changes
		greeting(newGreet) {
			console.log('Greeting changed to ' + newGreet)
		},
		name(newName) {
			console.log(`Name changed to ${newName}`)
		}
		counter(newCount) {
			this.fetchData();
		} 
	},

}
</script>

<template>
	<div>
		<input v-model="newGreeting" placeholder="Greeting" />
		<button @click="updateGreeting">Update</button>
	</div>
	<div>
		<form @submit.prevent="updateName">
			<input name="nameInput" placeholder="Name" />
			<button>Update</button>
		</form>
	</div>
	<p>`${greeting}, ${name}`</p>
	<pre>{{ responseContent }}</pre>
</template>
```