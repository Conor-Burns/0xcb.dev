<script lang="ts">
	import Card from '$lib/components/atoms/Card.svelte';
	import Image from '$lib/components/atoms/Image.svelte';
	import Footer from '$lib/components/organisms/Footer.svelte';
	import Header from '$lib/components/organisms/Header.svelte';
	import RelatedPosts from '$lib/components/organisms/RelatedPosts.svelte';
	import TableOfContents from '$lib/components/organisms/TableOfContents.svelte';

	import type BlogPost from '$lib/data/blog-posts/model';
	import { keywords, siteBaseUrl, title } from '$lib/data/meta';

	export let data: { post: BlogPost };
	$: ({ post } = data);

	$: showCardLayout = Boolean(post.showImage && post.coverImage?.src);

	let metaKeywords = keywords;

	$: {
		if (post?.categories?.length) {
			metaKeywords = post.categories.concat(metaKeywords);
		}
		if (post?.tags?.length) {
			metaKeywords = post.tags.concat(metaKeywords);
		}
		if (post?.keywords?.length) {
			metaKeywords = post.keywords.concat(metaKeywords);
		}
	}
</script>

<svelte:head>
	{#if post}
		<meta name="keywords" content={metaKeywords.join(', ')} />

		<meta name="description" content={post.excerpt} />
		<meta property="og:description" content={post.excerpt} />
		<meta name="twitter:description" content={post.excerpt} />
		<link rel="canonical" href="{siteBaseUrl}/{post.slug}" />

		<title>{post.title} - {title}</title>
		<meta property="og:title" content="{post.title} - {title}" />
		<meta name="twitter:title" content="{post.title} - {title}" />

		{#if post.socialImage?.src}
			<meta property="og:image" content="{siteBaseUrl}{post.socialImage.src}" />
			<meta name="twitter:image" content="{siteBaseUrl}{post.socialImage.src}" />
		{:else if post.coverImage?.src}
			<meta property="og:image" content="{siteBaseUrl}{post.coverImage.src}" />
			<meta name="twitter:image" content="{siteBaseUrl}{post.coverImage.src}" />
		{/if}
	{/if}
</svelte:head>

{#if post}
	<div class="article-layout" class:has-cover={showCardLayout}>
		{#if showCardLayout && post.coverImage}
			<div class="cover-image-wrapper">
				<Image
					additionalClass="cover-image"
					src={`${post.coverImage.src}`}
					alt={post.coverImage.alt}
				/>
				<div class="cover-image-overlay" />
			</div>
		{/if}

		<Header />

		<main class="article-main" class:has-cover={showCardLayout}>
			<div class="article-content-wrapper" class:card-layout={showCardLayout}>
				{#if showCardLayout}
					<Card additionalClass="article-card">
						<slot slot="content" />
					</Card>
				{:else}
					<div style="overflow: hidden;"><slot /></div>
				{/if}

				{#if post.showToc}
					<TableOfContents />
				{/if}
			</div>

			{#if post.relatedPosts && post.relatedPosts.length > 0}
				<div class="container">
					<RelatedPosts posts={post.relatedPosts} />
				</div>
			{/if}
		</main>

		<Footer />
	</div>
{/if}

<style lang="scss">
	@import '$lib/scss/_mixins.scss';

	.article-layout {
		--body-background-color: var(--color--post-page-background);
		--body-background-color-rgb: var(--color--post-page-background-rgb);
		background-color: var(--body-background-color);

		&.has-cover {
			--body-background-color: var(--color--page-background);
			--body-background-color-rgb: var(--color--page-background-rgb);

			:global(.article-card) {
				background-color: var(--color--post-card-background);
			}
		}
	}

	.cover-image-wrapper {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 60vh;

		:global(img.cover-image) {
			width: 100%;
			height: 100%;
			object-fit: cover;
		}
	}

	.cover-image-overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 60vh;
		background-image: linear-gradient(
			to bottom,
			var(--body-background-color) 0%,
			rgba(var(--body-background-color-rgb), 0) 100%
		);
	}

	.article-main {
		min-height: 55vh;

		position: relative;
		padding-top: 40px;
		padding-bottom: 80px;
		--inline-padding: 20px;
		padding-inline: var(--inline-padding);

		@include for-iphone-se {
			--inline-padding: 0;
		}

		@include for-tablet-portrait-up {
			--inline-padding: 25px;
		}

		@include for-tablet-landscape-up {
			--inline-padding: 30px;
		}

		&.has-cover {
			padding-top: 200px;
			padding-inline: 0;

			:global(.article-card) {
				padding-inline: var(--inline-padding);
			}
		}
	}

	.article-content-wrapper {
		display: flex;
		justify-content: center;
		gap: var(--inline-padding);

		&.card-layout {
			padding-inline: var(--inline-padding);

			@include for-phone-only {
				padding-inline: 0;
			}
		}

		:global(.table-of-contents) {
			flex: 0 0 240px;
		}
	}

	:global(.article-card) {
		max-width: 85ch;
	}

	:global(.article-card .wrapper .body) {
		padding: 0;
		width: 100%;
	}
</style>
