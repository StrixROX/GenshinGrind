<template>
	<div class="add-char-dialog-wrapper">
		<div class="add-char-dialog">
			<div
				class="close-btn"
				@click="this.$emit('closeAddCharCardDialogue')"
			>
				<i class="plus-icon"></i>
			</div>
			<h2 class="title">Choose Character</h2>
			<div class="choose-chars">
				<div
					class="char"
					v-for="char in all_chars"
					:key="char"
					@click="this.$emit('charSelected', char.id)"
				>
					<img :src="getCharIcon(char.icon)" class="char-icon" />
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "AddCharCardDialogue",
	data() {
		return {
			all_chars: null,
		};
	},
	methods: {
		fetchAllChars() {
			return fetch("https://api.ambr.top/assets/data/avatar.json");
		},
		getCharIcon(icon_name) {
			return "https://api.ambr.top/assets/UI/" + icon_name + ".png";
		},
	},
	created() {
		this.fetchAllChars()
			.then((res) => res.json())
			.then((res) => {
				this.all_chars = res.items;
			});
	},
};
</script>

<style scoped>
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
	cursor: pointer;
	margin: 10px;
}

.add-char-dialog .choose-chars .char .char-icon {
	width: 120px;
	height: 120px;
	border: 1px solid #ccc;
	border-radius: 100%;
}
</style>
