<script lang="ts">
	export let date: string | Date
	export let layout: 'horizontal' | 'vertical' = 'vertical'

	$: dateObject = new Date(date)
</script>

<style>
	.layout-vertical {
		display: flex;
		flex-direction: column;
		gap: 0.25em;
	}
	.layout-vertical .time {
		font-size: 0.8em;
	}
</style>

<time class="date layout-{layout}" datetime={dateObject.toISOString()}>
	<span class="day">{dateObject.toLocaleDateString()}</span>
	<span class="time">{dateObject.toLocaleTimeString()}</span>
</time>