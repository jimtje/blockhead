<script lang="ts">
	import type { Ethereum } from '../data/ethereum/types'
	import type { DefiSDK } from '../data/ethereum/price/defi-sdk'
	import { getDefiBalances } from '../data/ethereum/price/defi-sdk'

	export let token = 'ETH'

	function formatDecimal(value, decimalPlaces) {
		return value * 0.1 ** decimalPlaces
	}

	export let provider: Ethereum.Provider
	export let address: string
	
	import Loading from './Loading.svelte'
	import TokenIcon from './TokenIcon.svelte'
	import TokenValue from './TokenValue.svelte'
	
	import { scale } from 'svelte/transition'

	const defiProtocolColors: Record<DefiSDK.ProtocolName, string[]> = {
		'Aave': ['#77c0c7', '#b56da4'],
		'Aave • Uniswap Market': [],
		'Ampleforth': [],
		'Aragon': [],
		'Balancer': [],
		'Bancor': [],
		'bZx bZx': ['#33dfcc', '#003cda'],
		'Chi Gastoken by 1inch': ['#d21318', '#3d3d3c'],
		'Compound': ['#01d395'],
		'C.R.E.A.M.': [],
		'Curve': [/*'#840000',*/ '#fd2700', '#f9ea02', '#91ff67'/*, '#0213fa'*/], // unofficial
		'DDEX • Lending': [],
		'DDEX • Margin': [],
		'DDEX • Spot': [],
		'DeFi Money Market': [],
		'DODO': [],
		'dYdX': [],
		'Idle': [],
		'iearn.finance (v2/v3)': [],
		'Harvest': [],
		'KeeperDAO': [],
		'KIMCHI': [],
		'KyberDAO': [],
		'Chai': [],
		'Dai Savings Protocol': [],
		'Maker Governance': [],
		'Multi-Collateral Dai': [],
		'Matic': [],
		'Melon': [],
		'mStable': ['#000000'],
		'Nexus Mutual': [],
		'Pickle Finance': [],
		'PieDAO': ['#d90a9d', '#9811dc', '#7732f8', '#28d4ff'], // unofficial
		'PoolTogether': ['#4a31a5'],
		'SashimiSwap': [],
		'SushiSwap': [],
		'Swerve': [],
		'Synthetix': ['#1e1a31'],
		'TokenSets': [],
		'SetToken V2': [],
		'Uniswap V1': [],
		'Uniswap V2': ['#ff007a'],
		'yearn.finance • Vaults': ['#0273f6', '#075fb9'], // unofficial
		'0x Staking': []
	}

	const makeCardGradient = (colors = []) =>
		`linear-gradient(155deg, ${colors.length == 1 ? [`${colors[0]}20`, `${colors[0]}55`] : colors.map(color => `${color}50`)}`
</script>

<style>
	.stack {
		display: grid;
		grid: "area";
	}
	.stack > :global(*) {
		grid-area: area;
		place-items: start;
	}

	.containing {
		font-size: 0.8em;
		text-align: left;
	}
</style>

{#if provider && address}
	{#await getDefiBalances(provider, address)}
		<div class="stack">
			<Loading>
				<span slot="icon"><TokenIcon {token} /></span>
				Reading DeFi balances...
			</Loading>
		</div>
	{:then defiBalances}
		{#each defiBalances as protocol}
			<div in:scale class="card" style="--card-background-image: {makeCardGradient(defiProtocolColors[protocol.metadata.name])})">
				<h3><img src={`https://${protocol.metadata.iconURL}`} alt={protocol.metadata.name} width="20"/> {protocol.metadata.name}</h3>
				{#each protocol.adapterBalances as adapterBalance}
					{#if adapterBalance.metadata.adapterType === 'Debt'}
						<h4>{adapterBalance.metadata.adapterType}</h4>
					{/if}
					{#each adapterBalance.balances as {base: baseBalance, underlying}}
						<TokenValue token={baseBalance.metadata.symbol} value={formatDecimal(baseBalance.amount, baseBalance.metadata.decimals)} />
						{#if underlying.length}
							<div class="containing">
								{#each underlying as underlyingBalance}
									<p>┖ <TokenValue token={underlyingBalance.metadata.symbol} value={formatDecimal(underlyingBalance.amount, underlyingBalance.metadata.decimals)} /></p>
								{/each}
							</div>
						{/if}
					{/each}
				{/each}
			</div>
		{/each}
	{:catch error}
		{error}
	{/await}
{/if}
