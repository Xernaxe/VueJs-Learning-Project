<template>
	<div class="fetchedContent">
		<div class="mediaWrapper">
			<h2 class="title">{{ response.title }}</h2>
			<img
				v-if="contentType == 'image'"
				class="img"
				:src="[hd ? response.hdurl : response.url]"
				alt=""
			/>
			<iframe
				v-if="contentType == 'video'"
				width="320"
				height="240"
				:src="response.url"
			></iframe>
		</div>
		<div class="misc">
			<div class="wrapper">
				<button @click="toggleVisible" class="seeMore">See More!</button>
				<button v-if="contentType == 'image'" @click="toggleHd" class="seeMore">
					Switch to HD version
				</button>
				<div class="wrapperbutton">
					<h2>Choose a date:</h2>
					<label for="date">Date</label>
					<input v-model="date" type="date" name="date" />
					<button class="seeMore" @click="fetchByDate">Search</button>
				</div>
			</div>
			<div :class="['details', changeStyle]">
				<p>{{ response.date }}</p>
				<p>{{ response.explanation }}</p>
				<br />
				<p>Copyright: {{ response.copyright }}</p>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'fetchContent',
	data() {
		return {
			response: {},
			seeMore: false,
			hd: false,
			contentType: 'image',
			date: '',
		};
	},
	mounted() {
		fetch(
			`https://api.nasa.gov/planetary/apod?api_key=1913sbcbOjB55ZGbPvadG97lVrWmudzCFaxdoUjj`
		)
			.then((response) => response.json())
			.then((data) => {
				this.response = data;
				this.contentType = data.media_type;
				console.log(data);
			});
	},
	computed: {
		changeStyle() {
			if (this.seeMore) {
				return 'visible';
			} else {
				return 'hidden';
			}
		},
	},
	methods: {
		toggleVisible() {
			this.seeMore = !this.seeMore;
		},
		toggleHd() {
			this.hd = !this.hd;
		},
		fetchByDate() {
			fetch(
				`https://api.nasa.gov/planetary/apod?api_key=1913sbcbOjB55ZGbPvadG97lVrWmudzCFaxdoUjj&date=${this.date}`
			)
				.then((response) => response.json())
				.then((data) => {
					this.response = data;
					this.contentType = data.media_type;
				});
		},
	},
};
</script>

<style scoped>
.hidden {
	display: none;
}

.seeMore {
	background-color: #cacaca;
	min-width: 100px;
	max-width: 100px;

	height: 35px;
	display: flex;
	justify-content: center;
	align-items: center;
	border-radius: 15px;
	color: black;
	cursor: pointer;
}

.wrapper {
	display: flex;
	margin-top: 20px;
	gap: 20px;
	justify-content: center;
	align-items: center;
}

.mediaWrapper {
	display: flex;
	flex-direction: column;
	align-items: center;
}

.fetchedContent {
	display: flex;
	flex-direction: column;
	height: 100vh;
	align-items: center;
}

.img {
	max-width: 70vw;
	max-height: 70vh;
}

input {
	max-height: 50px;
	max-width: 100px;
}

.wrapperbutton {
	display: flex;
	flex-direction: column;
	align-items: center;
	gap: 5px;
}
.title {
	text-align: center;
	margin-bottom: 20px;
}

.details{
	max-width: 80vw;
}
</style>
