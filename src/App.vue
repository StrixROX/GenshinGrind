<template>
	<div class="wrapper">
		<h1>Characters</h1>
		<button @click="_clearSelected">Clear</button>
		
		<div class="char-card-container">
			<CharCard v-for="char_id in char_selected" :key="char_id" :char_id="char_id" />

			<div class="add-char-card" @click="showAddCharDialog=true">
				<span><i class="plus-icon"></i></span>
			</div>
		</div>

		<div v-if="showAddCharDialog">
			<AddCharCard @closeAddCharCardDialogue="showAddCharDialog=false" @charSelected="charSelected" />
		</div>
	</div>
</template>

<script>
import { defineAsyncComponent } from 'vue'

const CharCard = defineAsyncComponent(
	() => import(/* webpackChunkName: "CharCard" */ "./components/CharCard.vue")
);
const AddCharCard = defineAsyncComponent(
	() => import(/* webpackChunkName: "AddCharCard" */ "./components/AddCharCard.vue")
);

export default {
	name: "App",
	components: {
		CharCard,
		AddCharCard,
	},
	data() {
		return {
			char_selected: [],
			showAddCharDialog: false,
		}
	},
	methods: {
		charSelected(char_id) {
			this.char_selected.push(char_id);
			this.showAddCharDialog = false;
		},
		_clearSelected(){
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

/*  */

.add-char-dialog-wrapper {
	position: absolute;
	top: 0;
	left: 0;
	width: 100vw;
	height: 100vh;
	background-color: rgba(0, 0, 0, 0.6);
}

.add-char-dialog {
	position: relative;
	display: flex;
	flex-direction: column;
	top: 50%;
	left: 50%;
	width: 80%;
	height: 90%;
	max-width: 900px;
	max-height: 600px;
	padding: 40px 30px;
	background-color: white;
	border-radius: 5px;
	transform: translate(-50%, -50%);
}

.add-char-dialog .title {
	text-align: center;
	margin: 0;
	padding: 0 0 40px 0;
}

.add-char-dialog .choose-chars {
	display: flex;
	width: 100%;
	height: 100%;
	flex-direction: row;
	justify-content: center;
	align-items: flex-start;
	flex-wrap: wrap;
	overflow-y: auto;
}

.add-char-dialog .choose-chars .char {
	width: 120px;
	height: 120px;
	margin: 10px;
	border: 1px solid #ccc;
	border-radius: 100%;
	cursor: pointer;
}

.add-char-dialog .close-btn {
	display: block;
	position: absolute;
	top: 20px;
	right: 25px;
	width: 30px;
	height: 30px;
	transform: rotate(-45deg);
	cursor: pointer;
}
</style>
