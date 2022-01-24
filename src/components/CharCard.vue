<template>
	<div class="char-card" v-if="show">
		<div class="close-btn" @click="this.$emit('charRemoved', char_id)">
			<i class="plus-icon"></i>
		</div>
		<img :src="getCharIcon(char.icon)" class="char-icon" />
		<h2 class="char-name">{{ char.name.EN }}</h2>
		<div class="char-upgradable">
			<span class="name">Level</span>
			<div class="val">
				<input
					type="number"
					name="char-lvl-i"
					value="1"
					class="val-init"
					min="1"
					max="10"
				/>
				<span>→</span>
				<input
					type="number"
					name="char-lvl-f"
					value="10"
					class="val-final"
					min="1"
					max="10"
				/>
			</div>
		</div>
		<span class="sep"></span>
		<div
			class="char-upgradable"
			v-for="talent in getUpgradableTalents()"
			:key="talent"
		>
			<span class="name">{{ talent.name.EN }}</span>
			<div class="val">
				<input
					type="number"
					name="char-lvl-i"
					value="1"
					class="val-init"
					min="1"
					max="10"
				/>
				<span>→</span>
				<input
					type="number"
					name="char-lvl-f"
					value="10"
					class="val-final"
					min="1"
					max="10"
				/>
			</div>
		</div>
	</div>
	<div class="char-card loading" v-else>
		<p class="text">Loading Data</p>
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
		fetchCharData() {
			return fetch(
				"https://api.ambr.top/assets/data/avatar/" +
					this.char_id +
					".json"
			);
		},
		getCharIcon(icon_name) {
			return "https://api.ambr.top/assets/UI/" + icon_name + ".png";
		},
		objectFilter(obj, f) {
			return Object.keys(obj)
				.filter((key) => f(obj[key]))
				.reduce((res, key) => ((res[key] = obj[key]), res), {});
		},
		getUpgradableTalents() {
			return this.objectFilter(
				this.char.talent,
				(x) =>
					x.hasOwnProperty("promote") &&
					Object.keys(x.promote).length > 1
			);
		},
	},
	created() {
		this.fetchCharData()
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
	position: relative;
	width: 400px;
	min-width: 400px;
	min-height: 500px;
	margin: 0 10px;
	padding: 40px 45px;
	border-radius: 5px;
	box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.char-card.loading .text {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	text-align: center;

	animation: anim-flash 1s ease-out infinite;
}
@keyframes anim-flash {
	0% {
		opacity: 0;
	}
	50% {
		opacity: 1;
	}
	100% {
		opacity: 0;
	}
}

.char-icon {
	position: relative;
	left: 50%;
	width: 200px;
	border: 1px solid #ccc;
	border-radius: 100%;
	transform: translate(-50%, 0);
}

.char-name {
	position: relative;
	left: 50%;
	margin: 20px 0 30px 0;
	text-align: center;
	transform: translate(-50%, 0);
}

span.sep {
	display: block;
	position: relative;
	left: 50%;
	height: 0;
	width: 0;
	border: 2px solid #bbb;
	border-radius: 100%;
	transform: translate(-50%, 0);
	margin: 20px 0;
}

.char-upgradable {
	display: flex;
	width: 100%;
	justify-content: space-between;
	align-items: flex-start;
	margin: 20px 0;
}
.char-upgradable .name {
	max-width: 50%;
}
.char-upgradable .val span {
	margin: 0 5px;
}
</style>
