<script>
	import 'bootstrap';
	import '../app.scss';
	import Header from './Header.svelte';
	import './styles.css';
	import { onDestroy } from 'svelte';
	import Notifications from 'svelte-notifications';
	import {redirect} from '@sveltejs/kit';
	import { goto } from "$app/navigation";
	import {step} from './stores.js';

	// let currentStep ;
	// const unsubscribe = step.subscribe(value => {
	// 	currentStep = value;
	// });

	// onDestroy(unsubscribe);

	function gotoback() {
		var url = '';
		var nextStep = currentStep > 0 ? (currentStep - 1) % 3 : (2-currentStep)%3;
		switch(nextStep) {
			case 0: url = '/'; break;
			case 1: url = 'result1'; break;
			case 2: url = 'result2'; break;
			default: url = '/'; break;
		}
		step.set(nextStep);
		goto(url)
		// redirect(301, url);
	}
	function gotoforward() {
		var url = '';
		var nextStep = (currentStep + 1) % 3;
		switch(nextStep) {
			case 0: url = '/'; break;
			case 1: url = 'result1'; break;
			case 2: url = 'result2'; break;
			default: url = '/'; break;
		}
		step.set(nextStep);
		goto(url)
	}
</script>

<Notifications>
	<div class="app">
		<Header />

		<main>
			<div>
				<slot />
			</div>
		</main>

		<footer />
	</div>
</Notifications>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		padding: 0;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 100%;
		margin: 0 auto;
		box-sizing: border-box;
	}

	.main-body {
		display: flex;
		justify-content: space-between;
		flex-direction: row;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}

	.link-btn {
		border: solid 1px;
		border-radius: 10px;
		padding: 5px 15px;
		background-color: #212529;
		color: #eef1f8;
		width: 100%;
		width: 90px;
	}
	.link-btn:hover {
		background-color: #eef1f8;
		color: #212529;
		transition: 0.3s all;
	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
	@media (max-width: 760px) {
		main {
			padding-left: 0;
			padding-right: 0;
		}
		.forward-btn,
		.back-btn {
			/* display: none; */
		}

		.main-body {
			/* display: inline-block; */
		}
	}
</style>
