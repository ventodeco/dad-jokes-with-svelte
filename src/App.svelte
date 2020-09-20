<script>
	import{ onMount } from 'svelte';
	import { getRandomJoke } from './requests.js';
	import { Col, Container, Row, Button } from 'sveltestrap';

	import JokeForm from './Form.svelte';
	import Joke from './Joke.svelte';
	let randomJoke;
	let mode = "random";
	onMount(async () => {
		await onRandomJoke();
	});

	async function onRandomJoke() {
		try {
			randomJoke = await getRandomJoke();
		} catch(e) {
			alert("Lagi error bosq");
			return '';
		}
	}

</script>

<style>
	:global(.randomJoke){
		width: 100%;
	}

	:global(.row) {
		margin-top: 10px;
	}

</style>

<Container>
	<Row>
		<Col>
			<h1>Dad Jokes!!! :)</h1>
		</Col>
	</Row>
	<JokeForm />

	<Row>
		<Col>
			<Button on:click={onRandomJoke} class="randomJoke" color="danger">Random Joke</Button>
		</Col>
	</Row>

	<Row>
		<Col>
			<Joke joke="{randomJoke}" />
		</Col>
	</Row>
</Container>