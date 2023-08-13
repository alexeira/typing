<script lang="ts">
	import '../app.css'
</script>

<svelte:head>
	<title>typing</title>
</svelte:head>

<div class="layout">
	<nav>
		<h1>⌨️ typing</h1>
	</nav>

	<main>
		<slot />
	</main>
</div>

<style>
	h1 {
		font-family: 'Lexend Deca', sans-serif;
		font-size: 1.8rem;
		letter-spacing: 2px;
	}

	.layout {
		width: min(90%, 100ch);
		margin-inline: auto;
		height: 100%;
		display: grid;
		grid-template-rows: auto 1fr;
		align-items: center;
		padding: 2rem;
	}
</style>
