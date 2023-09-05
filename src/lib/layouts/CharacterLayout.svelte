<script lang="ts">
	import { AppShell } from '@skeletonlabs/skeleton';
	import { AppBar } from '@skeletonlabs/skeleton';
	import { AppRail, AppRailTile, AppRailAnchor } from '@skeletonlabs/skeleton';

	let currentTile: number = 0;
</script>

<AppShell
	slotPageContent="p-12"
	slotHeader="border-b border-surface-700/50"
	slotFooter="bg-surface-800 p-4 border-t border-surface-700/50"
	slotSidebarLeft="border-r border-surface-700/50"
	slotSidebarRight="bg-surface-800 w-[360px] border-l border-surface-700/50"
>
	<svelte:fragment slot="header">
		<AppBar gridColumns="grid-cols-3" slotDefault="place-self-center" slotTrail="place-content-end">
			<svelte:fragment slot="lead">
				<img src="/logo.png" alt="Diablo.run" class="h-7" />
			</svelte:fragment>
			<h1><slot name="title" /></h1>
			<svelte:fragment slot="trail">
				<button type="button" class="variant-filled btn-icon">(icon)</button>
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<svelte:fragment slot="sidebarLeft">
		<AppRail>
			<svelte:fragment slot="lead">
				<AppRailAnchor href="/">(icon)</AppRailAnchor>
			</svelte:fragment>
			<!-- --- -->
			<AppRailTile bind:group={currentTile} name="tile-1" value={0} title="Items">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Items</span>
			</AppRailTile>
			<AppRailTile bind:group={currentTile} name="tile-2" value={1} title="Inventory">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Inventory</span>
			</AppRailTile>
			<AppRailTile bind:group={currentTile} name="tile-3" value={2} title="Skills">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Stash</span>
			</AppRailTile>
			<AppRailTile bind:group={currentTile} name="tile-3" value={2} title="Skills">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Cube</span>
			</AppRailTile>
			<AppRailTile bind:group={currentTile} name="tile-3" value={2} title="Skills">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Merc</span>
			</AppRailTile>
			<AppRailTile bind:group={currentTile} name="tile-3" value={2} title="Skills">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Skills</span>
			</AppRailTile>
			<!-- --- -->
			<svelte:fragment slot="trail">
				<AppRailAnchor href="/" target="_blank" title="Account">(icon)</AppRailAnchor>
			</svelte:fragment>
		</AppRail>
	</svelte:fragment>
	<svelte:fragment slot="sidebarRight">
		<slot name="sidebarLeft" />
	</svelte:fragment>
	<!-- (pageHeader) -->
	<!-- Router Slot -->
	<slot />
	<!-- ---- / ---- -->
	<!-- (pageFooter) -->
	<svelte:fragment slot="footer">
		<slot name="footer" />
	</svelte:fragment>
</AppShell>
