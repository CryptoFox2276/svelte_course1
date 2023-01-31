<script>
	import { memory } from './stores.js';
	import {getNotificationsContext } from 'svelte-notifications';
	import { spring, tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
	import { fly, fade } from 'svelte/transition';

	const {addNotification} = getNotificationsContext();
	const progress = tweened(0, {
		duration: 1000,
		easing: cubicOut
	});;

	let carBrandHasError = false;
	let carBrandHasErrorMessage = '';
	let zipCodeError = false;
	let zipCodeErrorMessage = '';
	let firstNameError = false;
	let firstNameErrorMessage = '';
	let lastNameError = false;
	let lastNameErrorMessage = '';
	let carModelError = false;
	let carModelErrorMessage = '';

	let carBrandValidateRange = ['Audi', 'BMW', 'Nissan'];
	let zipCodeValidateRange = ['65000', '66000', '67000', '68000'];

	let isSuccessVisible = false;
	let submitted = false;

	let carBrand = '';
	let zipCode = '';
	let firstName = '';
	let lastName = '';
	let carModel = '';
	let firstRegistration = false;

	function init() {
		carBrandHasError = false;
		carBrandHasErrorMessage = '';
		zipCodeError = false;
		zipCodeErrorMessage = '';
		firstNameError = false;
		firstNameErrorMessage = '';
		lastNameError = false;
		lastNameErrorMessage = '';
		carModelError = false;
		carModelErrorMessage = '';

		carBrand = '';
		zipCode = '';
		firstName = '';
		lastName = '';
		carModel = '';
		firstRegistration = false;
	}

	function checkValidation() {
		isSuccessVisible = true;
		var isValidation = true;
		if (carBrand === '') {
			isValidation = false;
			carBrandHasError = true;
			carBrandHasErrorMessage = 'Please input car brand \n';
		} else if (!carBrandValidateRange.includes(carBrand)) {
			isValidation = false;
			carBrandHasError = true;
			carBrandHasErrorMessage = 'Value validation error';
		} else {
			isValidation = true;
			carBrandHasError = false;
			carBrandHasErrorMessage = '';
		}
		if (zipCode === '') {
			isValidation = false;
			zipCodeError = true;
			zipCodeErrorMessage = 'Pleaese input zip code';
		} else if (!zipCodeValidateRange.includes(zipCode)) {
			isValidation = false;
			zipCodeError = true;
			zipCodeErrorMessage = 'Value validation error';
		} else {
			isValidation = true;
			zipCodeError = false;
			zipCodeErrorMessage = '';
		}
		if (firstName === '') {
			isValidation = false;
			firstNameError = true;
			firstNameErrorMessage = 'Please input first name';
		} else {
			isValidation = true;
			firstNameError = false;
			firstNameErrorMessage = '';
		}
		if (lastName === '') {
			isValidation = false;
			lastNameError = true;
			lastNameErrorMessage = 'Please input last name';
		} else {
			isValidation = true;
			lastNameError = false;
			lastNameErrorMessage = '';
		}
		if (carModel === '') {
			isValidation = false;
			carModelError = true;
			carModelErrorMessage = 'Please input car model';
		} else {
			isValidation = true;
			carModelError = false;
			carModelErrorMessage = '';
		}
		return isValidation;
	}

	function handleSubmit() {
		
		if (!checkValidation()) {
			addNotification({
				text: 'Validation failed.',
				position: 'bottom-left',
				type: 'error',
				removeAfter: 4000,
			})
			return;
		}
		var formData = {
			carBrand: carBrand,
			zipCode: zipCode,
			firstName: firstName,
			lastName: lastName,
			carModel: carModel,
			firstRegistration: firstRegistration
		};

		$memory = [...$memory, formData];

		progress.set(1);
		init();

		addNotification({
			text: 'Successully Saved',
			position: 'bottom-left',
			type: 'success',
			removeAfter: 6000,
		})
		console.log($memory);
	}
</script>
	<section>
		<div class="container">
			<div class="row text-center">
				<h2 transition:fade>Sign UP</h2>
				<p transition:fade>Please fill out below form and click save button</p>
			</div>
			<hr style="padding-bottom: 0px; margin-bottom: 10px"/>
			<progress value={$progress} color="gray"></progress>
			<div class="container">
				<form id="surveyForm" class="mt-4" class:submitted on:submit|preventDefault={handleSubmit}>
					<div class="row">
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="brand" class="form-label" transition:fade>Car Brand( Audi, BMW, Nissan )</label>
							<input
								type="text"
								class="form-control"
								placeholder="Car Brand"
								bind:value={carBrand}
								on:input={checkValidation}
								transition:fade
							/>
							{#if carBrandHasError == true}
								<p class="error-alert" transition:fade>{@html carBrandHasErrorMessage}</p>
							{/if}
						</div>
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="code" class="form-label" transition:fade>Zip Code( 65000, 66000, 67000, 68000 )</label>
							<input
								type="text"
								class="form-control"
								placeholder="Zip Code"
								bind:value={zipCode}
								on:input={checkValidation}
								transition:fade
							/>
							{#if zipCodeError == true}
								<p class="error-alert" transition:fade>{zipCodeErrorMessage}</p>
							{/if}
						</div>
					</div>
					<div class="row">
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="firstName" class="form-label" transition:fade>First Name:</label>
							<input
								type="text"
								class="form-control"
								placeholder="First Name"
								bind:value={firstName}
								on:input={checkValidation}
								transition:fade
							/>
							{#if firstNameError == true}
								<p class="error-alert" transition:fade>{firstNameErrorMessage}</p>
							{/if}
						</div>
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="lastName" class="form-label" transition:fade>Last Name:</label>
							<input
								type="text"
								class="form-control"
								placeholder="Last Name"
								bind:value={lastName}
								on:input={checkValidation}
								transition:fade
							/>
							{#if lastNameError == true}
								<p class="error-alert" transition:fade>{lastNameErrorMessage}</p>
							{/if}
						</div>
					</div>
					<div class="row">
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="carModel" class="form-label" transition:fade>Car Model:</label>
							<input
								type="text"
								class="form-control"
								placeholder="Car Model"
								bind:value={carModel}
								on:input={checkValidation}
								transition:fade
							/>
							{#if carModelError == true}
								<p class="error-alert" transition:fade>{carModelErrorMessage}</p>
							{/if}
						</div>
					</div>
					<div class="form-check pt-3">
						<input
							class="form-check-input"
							type="checkbox"
							id="check1"
							name="option1"
							on:click={() => firstRegistration = !firstRegistration}
							checked={firstRegistration}
						/>
						<label class="form-check-label">First Registration</label>
					</div>
					<div class="row justify-content-center pt-5">
						<button class="btn btn-full col-lg-3 col-md-5" transition:fade>Save</button>
					</div>
				</form>
			</div>
			<div class="container">
				<form id="surveyForm" class="mt-4" class:submitted on:submit|preventDefault={handleSubmit}>
					<div class="row">
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="brand" class="form-label" transition:fade>Car Brand( Audi, BMW, Nissan )</label>
							<input
								type="text"
								class="form-control"
								placeholder="Car Brand"
								bind:value={carBrand}
								on:input={checkValidation}
								transition:fade
							/>
							{#if carBrandHasError == true}
								<p class="error-alert" transition:fade>{@html carBrandHasErrorMessage}</p>
							{/if}
						</div>
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="code" class="form-label" transition:fade>Zip Code( 65000, 66000, 67000, 68000 )</label>
							<input
								type="text"
								class="form-control"
								placeholder="Zip Code"
								bind:value={zipCode}
								on:input={checkValidation}
								transition:fade
							/>
							{#if zipCodeError == true}
								<p class="error-alert" transition:fade>{zipCodeErrorMessage}</p>
							{/if}
						</div>
					</div>
					<div class="row">
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="firstName" class="form-label" transition:fade>First Name:</label>
							<input
								type="text"
								class="form-control"
								placeholder="First Name"
								bind:value={firstName}
								on:input={checkValidation}
								transition:fade
							/>
							{#if firstNameError == true}
								<p class="error-alert" transition:fade>{firstNameErrorMessage}</p>
							{/if}
						</div>
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="lastName" class="form-label" transition:fade>Last Name:</label>
							<input
								type="text"
								class="form-control"
								placeholder="Last Name"
								bind:value={lastName}
								on:input={checkValidation}
								transition:fade
							/>
							{#if lastNameError == true}
								<p class="error-alert" transition:fade>{lastNameErrorMessage}</p>
							{/if}
						</div>
					</div>
					<div class="row">
						<div class="col-lg-6 col-md-12 pb-3">
							<label for="carModel" class="form-label" transition:fade>Car Model:</label>
							<input
								type="text"
								class="form-control"
								placeholder="Car Model"
								bind:value={carModel}
								on:input={checkValidation}
								transition:fade
							/>
							{#if carModelError == true}
								<p class="error-alert" transition:fade>{carModelErrorMessage}</p>
							{/if}
						</div>
					</div>
					<div class="form-check pt-3">
						<input
							class="form-check-input"
							type="checkbox"
							id="check1"
							name="option1"
							on:click={() => firstRegistration = !firstRegistration}
							checked={firstRegistration}
						/>
						<label class="form-check-label">First Registration</label>
					</div>
					<div class="row justify-content-center pt-5">
						<button class="btn btn-full col-lg-3 col-md-5" transition:fade>Save</button>
					</div>
				</form>
			</div>
		</div>
	</section>

<style>
	.container {
		max-width: 1200px;
		margin: 0 auto;
	}

	h2 {
		margin-top: 0;
		padding: 0.75rem 0;
	}

	.form-control {
		display: inline-block;
	}

	.form-control,
	.btn-full {
		border-radius: 3px;
	}

	.submitted input:invalid {
		border: 1px solid #c00;
	}

	.submitted input:focus:invalid {
		outline: 1px solid #c00;
	}

	.error-alert {
		padding: 0.375em 0.75em;
		text-align: left;
		color: #c00;
		border-radius: 3px;
		font-size: 0.9rem;
	}
	p.error-alert {
		padding-bottom: 0 !important;
		margin-bottom: 0 !important;
	}
	.btn {
		border: solid 1px;
		border-radius: 10px;
		background-color: #212529;
		color: #fff;
	}
	.btn:hover {
		background-color: #eff2f9;
		color: #212529;
		transition: 0.3s ease;
	}
	progress {
		width: 100%;
	}
</style>
