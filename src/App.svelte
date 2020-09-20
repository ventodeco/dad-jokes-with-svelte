<script>
	import{ onMount } from 'svelte';
	import { getRandomJoke, getSearchJokes } from './requests.js';
	import { Col, Container, Row, Button } from 'sveltestrap';

	import Transition from './Transition.svelte';
	import JokeForm from './Form.svelte';
	import Joke from './Joke.svelte';
	let randomJoke;
	let jokes = [];
	let mode = "loading";
	onMount(async () => {
		await onRandomJoke();
	});

	async function onSearch(e) {
		try {
			mode = "loading";
			sleep(500);
			jokes = await getSearchJokes(e.detail);
			sleep(500);
			mode = "search";
		} catch(e) {
			alert("Lagi error bosq");
		}
	}

	async function onRandomJoke() {
		try {
			mode = "loading";
			sleep(500);
			randomJoke = await getRandomJoke();
			sleep(500);
			mode = "random";
		} catch(e) {
			alert("Lagi error bosq");
			return '';
		}
	}

	const sleep = (delayMS) => new Promise(res => setTimeout(res, delayMS))

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
	<JokeForm on:search={onSearch} />

	<Row>
		<Col>
			<Button on:click={onRandomJoke} class="randomJoke" color="danger">Random Joke</Button>
		</Col>
	</Row>
	{#if mode === "random"}
		<Transition>
			<Row>
				<Col>
					<Joke joke="{randomJoke}" />
				</Col>
			</Row>
		</Transition>
	{/if}

	{#if mode === "search" && jokes.length > 0}
		
		{#each jokes as jokeObj (jokeObj.id)}
			<Transition>
				<Row>
					<Col>
						<Joke joke="{jokeObj.joke}" />
					</Col>
				</Row>
			</Transition>
		{/each}
		
	{/if}

	{#if mode === "search" && jokes.length == 0}
		<Transition>
			<Row>
				<Col>
					<Joke joke="Jokes on you!!! :))) Please try another search" />
				</Col>
			</Row>
		</Transition>
	{/if}
</Container>