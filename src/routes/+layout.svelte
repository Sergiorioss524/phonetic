<script>
	import "../app.css";
	import {Navbar} from '$lib'
	import Footer from '$lib/Footer.svelte';
</script>


<Navbar />

<slot />

<Footer/>