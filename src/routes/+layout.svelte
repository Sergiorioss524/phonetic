<script>
	import "../app.css";
	import {Navbar} from '$lib'
	import Footer from '$lib/Footer.svelte';
</script>
<div id="app-wrapper">
	<Navbar />

	<main>
		<slot />
	</main>

	<Footer />
</div>