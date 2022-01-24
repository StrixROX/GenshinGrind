<template>
	<div class="wrapper">
		<h1>Characters</h1>
		<button @click="_clearSelected">Clear All</button>

		<div class="char-card-container">
			<CharCard
				v-for="char_id in char_selected"
				:key="char_id"
				:char_id="char_id"
				@charRemoved="onCharRemoved"
			/>

			<div class="add-char-card" @click="showAddCharDialog = true">
				<span><i class="plus-icon"></i></span>
			</div>
		</div>

		<div v-if="showAddCharDialog">
			<AddCharCardDialogue
				@closeAddCharCardDialogue="showAddCharDialog = false"
				@charSelected="charSelected"
			/>
		</div>
	</div>
</template>

<script>
import { defineAsyncComponent } from "vue";

const CharCard = defineAsyncComponent(() =>
	import(/* webpackChunkName: "CharCard" */ "./components/CharCard.vue")
);
const AddCharCardDialogue = defineAsyncComponent(() =>
	import(
		/* webpackChunkName: "AddCharCardDialogue" */ "./components/AddCharCardDialogue.vue"
	)
);

export default {
	name: "App",
	components: {
		CharCard,
		AddCharCardDialogue,
	},
	data() {
		return {
			char_selected: [],
			showAddCharDialog: false,
		};
	},
	methods: {
		charSelected(char_id) {
			this.char_selected.push(char_id);
			this.showAddCharDialog = false;
		},
		onCharRemoved(char_id) {
			this.char_selected = this.char_selected.filter((x) => x != char_id);
		},

		_clearSelected() {
			this.char_selected = [];
		},
	},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300&display=swap");

body {
	background-color: #ededed;
}

* {
	box-sizing: border-box;
	font-family: "Open Sans", sans-serif;
	letter-spacing: 3px;
}

i.plus-icon {
	display: inline-block;
	position: relative;
	top: 50%;
	left: 50%;
	width: 2px;
	height: 30px;
	color: inherit;
	background: currentColor;
	border-radius: 5px;
	transform: translate(-50%, -50%);
}
i.plus-icon::after {
	content: "";
	display: inline-block;
	width: 100%;
	height: 100%;
	color: inherit;
	background-color: currentColor;
	border-radius: 5px;
	transform: rotate(-90deg);
}
.close-btn {
	display: block;
	position: absolute;
	top: 20px;
	left: auto;
	right: 25px;
	bottom: auto;
	width: 30px;
	height: 30px;
	transform: rotate(-45deg);
	transition: opacity 0.2s;
	opacity: 0.2;
	cursor: pointer;
}
.close-btn:hover {
	opacity: 1;
}

.wrapper {
	padding: 0 5%;
}

.char-card-container {
	display: flex;
	width: 100%;
	flex-direction: row;
	justify-content: flex-start;
	align-items: flex-start;
	padding: 20px;
	overflow-x: auto;
}

.add-char-card {
	width: 400px;
	min-width: 400px;
	height: 500px;
	margin: 0 10px;
	border-radius: 5px;
	box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
	cursor: pointer;
}

.add-char-card span {
	display: inline-block;
	position: relative;
	top: 50%;
	left: 50%;
	width: 150px;
	height: 150px;
	color: #ccc;
	transform: translate(-50%, -50%);
	border: 2px solid currentColor;
	border-radius: 100%;
	user-select: none;
}
</style>
