<script lang="ts">
	import '../app.css';
	import Header from '$lib/components/Header.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import { page } from '$app/stores';
	
	$: currentPage = $page.route.id;
</script>

<svelte:head>
	<title>OHDSI Africa - Observational Health Data Sciences and Informatics</title>
	<meta name="description" content="Building collaborative health data research across Africa through open-source solutions and standardized data models." />
</svelte:head>

<Header {currentPage} />

<main>
	<slot />
</main>

<Footer />

<style>
	main {
		min-height: calc(100vh - 200px);
	}
</style>