<script lang="ts">
	export let segment: string;
</script>

<style>
	nav {
		font-size: 1.2em;
		font-weight: 300;
	}

	ul {
		list-style-type: none;
		display: flex;
	}
	li {
		display: flex;
	}

	a {
		text-decoration: none;
		padding: 0.75rem;
		transition: 0.2s, font-weight 0.2s;
	}

	[aria-current] {
		border-bottom: 2px solid var(--primary-color);
		margin-bottom: -1px;
		font-weight: bold;
	}

	.logo {
		font-size: 1.4em;
	}
</style>

<nav>
	<ul>
		<li><a aria-current="{segment === undefined ? 'page' : undefined}" href="."><span class="logo">Blockhead</span></a></li>
		<li><a aria-current="{segment === 'portfolio' ? 'page' : undefined}" href="portfolio">Portfolio</a></li>
		<li><a aria-current="{segment === 'trade' ? 'page' : undefined}" href="trade">Trade</a></li>
		<!-- <li><a aria-current="{segment === 'invest' ? 'page' : undefined}" href="invest">Invest</a></li> -->

		<!-- for the blog link, we're using rel=prefetch so that Sapper prefetches
			 the blog data when we hover over the link or tap it on a touchscreen -->
		<li><a rel=prefetch aria-current="{segment === 'explorer' ? 'page' : undefined}" href="explorer">Explorer</a></li>

	</ul>
</nav>
