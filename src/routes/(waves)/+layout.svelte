<script>
	import Waves from '$lib/components/organisms/Waves.svelte';
	import Header from '$lib/components/organisms/Header.svelte';
	import Footer from '$lib/components/organisms/Footer.svelte';

	import { description, image, keywords, title, siteBaseUrl } from '$lib/data/meta';
</script>

<svelte:head>
	<link rel="“canonical”" href={siteBaseUrl} />
	<meta name="keywords" content={keywords.join(', ')} />

	<meta name="description" content={description} />
	<meta property="og:description" content={description} />
	<meta name="twitter:description" content={description} />

	<title>{title}</title>
	<meta property="og:title" content={title} />
	<meta name="twitter:title" content={title} />

	<meta property="og:image" content={image} />
	<meta name="twitter:image" content={image} />
</svelte:head>

<Waves />

<Header showFullLogo={true} />

<main>
	<div class="background-blurrer" />
	<div class="blob one" />
	<div class="blob two" />
	<div class="blob three" />
	<slot />
</main>

<Footer />

<style lang="scss">
	main {
		position: relative;
		overflow-x: clip;
	}

	.background-blurrer {
		position: absolute;
		top: 0;
		left: 0;
		height: 100%;
		width: 100%;
		background: rgba(var(--color--page-background-rgb), 0.7);
		z-index: -1;
		-webkit-backdrop-filter: blur(100px);
		backdrop-filter: blur(100px);
	}
	.blob {
		position: absolute;
		animation: float 2s cubic-bezier(0.55, 0.5, 0.45, 0.5) infinite;
		animation-fill-mode: both;
		background: var(--color--primary-shade);
		z-index: -2;
		--size: 400px;
		height: var(--size);
		width: var(--size);

		&.one {
			border-radius: var(--radius-blob-1);
			top: max(600px, calc(15% - var(--size)));
			left: 10%;
			animation-duration: 10s;
		}
		&.two {
			background: var(--color--secondary-shade);
			border-radius: var(--radius-blob-2);
			--size: 420px;
			top: max(600px, calc(45% - var(--size)));
			left: 70%;
			animation-duration: 10s;
		}
		&.three {
			border-radius: var(--radius-blob-3);
			--size: 440px;
			top: max(700px, calc(75% - var(--size)));
			left: -10%;
			animation-duration: 10s;
		}
	}
</style>
