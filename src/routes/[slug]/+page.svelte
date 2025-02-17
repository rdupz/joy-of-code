<script lang="ts">
	import { browser, dev } from '$app/environment'

	import { updateViews } from '$lib/database'
	import { formatDate } from '$lib/utils'
	import * as config from '$lib/site/config'

	import Card from './card.svelte'
	import Clipboard from './clipboard.svelte'
	import Overlay from './overlay.svelte'

	export let data

	const { content, frontmatter } = data.post

	let editUrl = `${config.fileUrl}/${frontmatter.slug}/${frontmatter.slug}.md`
	let image = `${config.postImage}${encodeURIComponent(frontmatter.title)}.png`

	if (!dev && browser) {
		updateViews(frontmatter.slug)
	}
</script>

<svelte:head>
	<title>{frontmatter.title}</title>

	<meta content={frontmatter.description} name="description" />

	<meta content={frontmatter.title} property="og:title" />
	<meta content={image} property="og:image" />
	<meta content={config.siteUrl} property="og:url" />
	<meta content={frontmatter.description} property="og:description" />
	<meta content={config.siteName} property="og:site_name" />

	<meta content={config.twitterHandle} name="twitter:creator" />
	<meta content="summary_large_image" name="twitter:card" />
	<meta content={frontmatter.title} name="twitter:title" />
	<meta content={frontmatter.description} name="twitter:description" />
	<meta content={image} name="twitter:image" />
</svelte:head>

<Clipboard />

<main>
	<Overlay />

	<article class="prose">
		<hgroup>
			<h1>{frontmatter.title}</h1>
			<p>Published {formatDate(frontmatter.published)}</p>
		</hgroup>

		{@html content}
	</article>

	<div class="cards">
		<Card preset="support" />
		<Card preset="newsletter" />
		<Card preset="edit" {editUrl} />
	</div>
</main>

<style>
	.cards {
		display: grid;
		row-gap: var(--spacing-32);
		max-width: var(--post-txt-length);
		margin: var(--spacing-64) 0;
		margin-inline: auto;
	}
</style>
