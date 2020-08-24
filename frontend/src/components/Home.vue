<template>
<div id="home">
	<div id="top-content">
		<h1>Global Chatroom</h1>
		<user-name-widget :user-name="userName" @submit-name-change="submitNameChange"/>
	</div>

	<div id="chats">
		<p v-for="(message, $index) in receivedMessages" :key="$index">
			<strong>{{message.userName}}:</strong>
			<span> {{message.messageText}}</span>
		</p>
	</div>

	<input type="text" id="chat-input" name="chat-input" v-model="chatInput" @keyup.enter=submitChat>

</div>
</template>

<script>
import UserNameWidget from './UserNameWidget.vue';

let io = require('sails.io.js')( require('socket.io-client') );
io.sails.url = 'localhost:1337';
io.sails.useCORSRouteToGetCookie = false;

export default {
	name: 'Home',
	components: {
		UserNameWidget,
	},
	data() {
		return {
			chatInput: '',
			receivedMessages: [],
			userName: 'anonymous',
			anonymousUserNameOptions: ['cheetah', 'snake', 'mammoth', 'moose', 'jackrabbit', 'hippo', 'dance-master', 'hooligan', 'whippersnapper'],
		}
	},
	methods: {
		// Send chat message to server
		submitChat() {
			const self = this;
			io.socket.post('/submit-chat', {
				messageText: this.chatInput,
				userName: this.userName
			}, function (resData, jwres) {
				// Handle response
				console.log(resData);
				console.log(jwres);
				self.chatInput = '';
			});
		},
		// Update client's username
		submitNameChange(newUserName) {
			const self = this;
			io.socket.post('/name-change', {
				oldUserName: self.userName,
				newUserName: newUserName,
			}, function (resData, jwres) {
				// Handle response
				console.log(resData);
				console.log(jwres);
				self.chatInput = '';
				self.userName = newUserName;
			});
		}
	},
	mounted() {
		// Randomize anonymous username e.g. 'anonymous hippo'
		const anonymousAnimal = this.anonymousUserNameOptions[Math.floor(Math.random() * this.anonymousUserNameOptions.length)];
		this.userName = `anonymous ${anonymousAnimal}`;

		// Subscribe to socket updates
		io.socket.on('message', (msg) => {
			this.receivedMessages.push(msg);
		});
	},
}
</script>

<style>
#home {
	display: block;
	position: relative;
	margin: 32px;
}

#top-content {
	display: flex;
	position: relative;
	justify-content: space-between;
	align-items: center;
}

#chats {
	display: block;
	position: relative;
	margin-bottom: 32px;
	height: 450px;
	text-align: left;
	overflow-y: scroll;
	box-shadow: 2px 2px 15px rgba(0, 0, 0, .25);
	padding: 16px;
}

#chat-input {
	width: 100%;
}
</style>