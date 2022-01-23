<template>
	<div class="char-card" v-if="show">
		<p class="text">{{ char.name.EN }}</p>
	</div>
	<div class="char-card loading" v-else>
		<p class="text">Loading</p>
	</div>
</template>

<script>
export default {
	name: "CharCard",
	props: ["char_id"],
	data() {
		return {
			show: false,
			char: null,
		};
	},
	methods: {
		fetchCharData(char_id) {
			return fetch(
				"https://api.ambr.top/assets/data/avatar/" + char_id + ".json"
			);
		},
		getCharIcon(icon_name) {
			return "https://api.ambr.top/assets/UI/" + icon_name + ".png";
		},
	},
	created() {
		this.fetchCharData(this.char_id)
			.then((res) => res.json())
			.then((res) => {
				this.char = res;
				this.show = true;
			});
	},
};
</script>

<style scoped>
.char-card {
	width: 400px;
	min-width: 400px;
	height: 500px;
	margin: 0 10px;
	border-radius: 5px;
	box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.char-card.loading .text {
	position: relative;
	top: 50%;
	transform: translate(0, -50%);
	text-align: center;
	animation: anim-flash 1s ease-out infinite;
}

@keyframes anim-flash{
	0% { opacity: 0; }
	50% { opacity: 1; }
	100% { opacity: 0; }
}
</style>
