<script>
	import Toast from './components/Toast.svelte';
	import Card from './components/Card.svelte';
	
	const SOLUTIONS = ['no', 'yes', 'no', 'no'];

	let name = '#quedateencasa';
	let question = 0;
	let points = 0;
	let quiz = false;
	let showToast = false;
	let toastConfig = {
		icon: false,
		autoClose: true
	};

	const toastConfigSuccess = {
		classModifier: 'success',
		type: 'success',
		text: '¡Bien contestado! Sigue así',
	}

	const toastConfigError = {
		classModifier: 'error',
		type: 'error',
		text: 'De eso nada, no funciona así',
	}

	function initQuiz() {
		quiz = true;
	}

	function reInitQuiz() {
		question = 0;
		points = 0;
	}

	function handleResponse(event) {
		if (SOLUTIONS[question] === event.detail.answer) {
			points += 1;
			toastConfig = {...toastConfig, ...toastConfigSuccess};
		} else {
			toastConfig = {...toastConfig, ...toastConfigError};
		}
		
		question += 1;
		showToast = true;
	}
</script>

<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
	<style>
		* {
			box-sizing: border-box;
			font-family: 'Poppins', sans-serif;
		}

		button {
			--backgroundColor: 193, 119, 190;	
			display: inline-block;
			padding: 1rem 3rem;
			margin-bottom: 0;
			border: 0;
			border-radius: 0.25rem;
			font-size: 1rem;
			color: #f8fdfe;
			background-color: rgba(var(--backgroundColor), 1);
			transition: all 200ms;
			cursor: pointer;		
		}

		button:hover, button:focus {
			transform: translateY(-0.25rem);
			background-color: rgba(var(--backgroundColor), 0.9);
		}
		
		button[disabled] {
			opacity: 0.47;
			cursor: not-allowed;
		}

		button[disabled]:hover {
			transform: none;
		}
				
	</style>
</svelte:head>

<main class="content">
	<article class="board">
		<header class="board__header">
			<h1 class="board__title">{name}</h1>
			<p class="board__subtitle">Un quiz <a class="board__link" href="https://boream.com/" target="_blank" rel="noopener">Boream</a> para la cuarentena</p>
			{#if quiz}
				<p class="board__score">Puntos: <span aria-live="polite">{points}</span></p>
			{:else}
				<button class="btn" on:click="{initQuiz}">Empezar</button>
			{/if}
		</header>
		
		<div class="board__content" aria-live="polite">
			{#if quiz}
				{#if showToast}
					<Card isDisabled="{showToast}"></Card>
				{:else if question === 0}
					<Card on:response="{handleResponse}" isDisabled="{showToast}">
						<h2 slot="header">En la fase 0 ¿puedo montar en bicicleta tándem?</h2>
					</Card>
				{:else if question === 1}
					<Card on:response="{handleResponse}" isDisabled="{showToast}">
						<h2 slot="header">¿Debe pasar la cuarentena un libro si voy a prestarlo?</h2>
					</Card>
				{:else if question === 2}
					<Card on:response="{handleResponse}" isDisabled="{showToast}">
						<h2 slot="header">Si lavarse las manos es eficiente, ¿puedo lavarme las manos con los guantes y reutilizarlos?</h2>
					</Card>
				{:else if question === 3}
					<Card on:response="{handleResponse}" isDisabled="{showToast}">
						<h2 slot="header">Si el bicho desaparece con las altas temperaturas, ¿no es conveniente el cambio climático?</h2>
					</Card>
				{:else}
					<h2 class="board__result">
						{#if points > 1}
							<span>&#127881; &#127881; &#127881;</span>Puedes estar tranquilo, te irá bien.<br/>Pero recuerda seguir las recomendaciones de organismos oficiales
						{:else}
							<span>&#128169; &#128169; &#128169;</span>Mejor quédate en casa un poco más<br />y sigue las recomendaciones de los organismos oficiales
						{/if}
					</h2>
					<button on:click="{reInitQuiz}">Volver a jugar</button>
				{/if}
			{/if}
		</div>
	</article>
	
	{#if showToast}
		<Toast on:close="{() => showToast = false}" {...toastConfig}>
		</Toast>
	{/if}
</main>

<style>
	.content {
		display: flex;
		justify-content: center;
		min-height: calc(100vh - 1rem);
		padding: 15vh 1em;
		margin: 0 auto;
		text-align: center;
		color: rgb(17, 98, 135);
		background-color: #f0f8fa;
	}

	.board {
		position: relative;
		height: 35rem;
	}

	.board__header p {
		margin-bottom: 2.5rem;
	}		

	.board__link {
		--frontColor: 193, 119, 190;
		font-weight: 400;
		color: rgba(var(--frontColor), 1);
		background-image: linear-gradient(90deg, currentColor 0, currentColor);
		background-position: 0 95%;
		background-size: 0 2px;
		background-repeat: no-repeat;		
		transition: all 200ms;
	}

	.board__link:hover {
		--frontColor: 117, 181, 213;
		background-size: 100% 2px;
		text-decoration: none;
	}

	.board__title {
		margin-top: 0;
		margin-bottom: 1rem;
		color: rgba(117, 181, 213, 1);
		font-size: 3rem;
		font-weight: 600;
	}

	.board__score {
		font-weight: 600;
	}

	.board__score > span {
		display: inline-block;
		width: 1rem;
	}

	.board__result {
		max-width: 32rem;
		font-weight: 400;
		margin-bottom: 2rem;
	}

	.board__result > span {
		display: block;
		margin-bottom: 2rem;
		font-size: 32px;
		line-height: 1;
	}
</style>