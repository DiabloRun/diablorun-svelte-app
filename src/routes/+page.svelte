<script lang="ts">
	import CharacterLayout from '$lib/layouts/CharacterLayout.svelte';
	import { onMount } from 'svelte';
	import { popup } from '@skeletonlabs/skeleton';
	import type { PopupSettings } from '@skeletonlabs/skeleton';

	const popupHover: PopupSettings = {
		event: 'hover',
		target: 'popupHover',
		placement: 'bottom'
	};

	let character: any = {
		attributes: {
			strength: 0,
			dexterity: 0,
			vitality: 0,
			energy: 0
		},
		speeds: {
			fcr: 0,
			frw: 0,
			fhr: 0,
			ias: 0
		},
		resitances: {
			fire: 0,
			cold: 0,
			lightning: 0,
			poison: 0
		}
	};
	let items: any[] = [];
	let quests: any[] = [];
	let error: string | null = null;
	let isLoading = true;

	onMount(async () => {
		try {
			const response = await fetch('https://api.diablo.run/snapshots/users/RyuQuezacotl');

			if (!response.ok) {
				throw new Error(`HTTP error: ${response.status} - ${response.statusText}`);
			}

			const responseData = await response.json();
			character = {
				...responseData.character,
				attributes: {
					strength: responseData.character.strength,
					dexterity: responseData.character.dexterity,
					vitality: responseData.character.vitality,
					energy: responseData.character.energy
				},
				speeds: {
					fcr: responseData.character.fcr,
					frw: responseData.character.frw,
					fhr: responseData.character.fhr,
					ias: responseData.character.ias
				},
				resistances: {
					fire: responseData.character.fire_res,
					cold: responseData.character.cold_res,
					lightning: responseData.character.light_res,
					poison: responseData.character.poison_res
				}
			};
			items = responseData.items;
			quests = responseData.quests;
		} catch (err) {
			console.error('Error:', err);
			error = 'An error occurred while fetching data.';
		} finally {
			isLoading = false;
		}
	});
</script>

{#if isLoading}
	<p>Loading data...</p>
{:else if error}
	<p>{error}</p>
{:else}
	<CharacterLayout>
		<svelte:fragment slot="title">
			<ol class="breadcrumb">
				<li class="crumb">
					<a href="/elements/breadcrumbs" class="variant-soft-primary btn btn-sm">Users</a>
				</li>
				<li class="crumb-separator" aria-hidden>&rsaquo;</li>
				<li class="crumb">
					<a href="/">
						<img
							src={character.user_profile_image_url}
							alt={character.user_name}
							class="h-10 w-10 rounded-full"
							use:popup={popupHover}
						/>
					</a>
					<div class="card p-4 shadow" data-popup="popupHover">
						<div>{character.user_name}</div>
					</div>
				</li>
				<li class="crumb-separator" aria-hidden>&rsaquo;</li>
				<li class="crumb">
					<a href="/elements/breadcrumbs" class="variant-soft-primary btn btn-sm">Characters</a>
				</li>
				<li class="crumb-separator" aria-hidden>&rsaquo;</li>
				<li class="crumb">
					{character.name}
				</li>
			</ol>
		</svelte:fragment>

		<svelte:fragment slot="sidebarLeft">
			<h1>{character.name}</h1>
			<h2>{character.hero}</h2>
			<p>{character.gold_total} gold</p>
			<p>{character.gold} gold in hand</p>
			<p>{character.gold_stash} gold in stash</p>
			<p>{character.area} area</p>
			<p>{character.difficulty}</p>
			<p>{character.difficulty}</p>
			<p>Players {character.players}</p>
			<p>{character.hardcore ? 'Softcore' : 'Hardcore'}</p>
			<p>{character.lod ? 'Classic' : 'Expansion'}</p>
			<p>{character.dead ? 'Dead' : 'Alive'}</p>
			<p>{character.deaths} deaths</p>
			<p>{character.town_visits}</p>
			<div class="grid grid-cols-4 text-center">
				{#each Object.keys(character.attributes) as attribute}
					<div class="border-t border-surface-700/50 px-4 py-8">
						<div class="font-mono text-2xl">{character.attributes[attribute]}</div>
						<div class="text-sm font-light text-surface-400">{attribute}</div>
					</div>
				{/each}
			</div>
			<div class="grid grid-cols-4 text-center">
				{#each Object.keys(character.speeds) as speed}
					<div class="border-t border-surface-700/50 px-4 py-8">
						<div class="font-mono text-2xl">{character.speeds[speed]}</div>
						<div class="text-sm font-light uppercase text-surface-400">{speed}</div>
					</div>
				{/each}
			</div>
			<div class="grid grid-cols-4 text-center">
				{#each Object.keys(character.resistances) as resistance}
					<div class="border-t border-surface-700/50 px-4 py-8">
						<div class="font-mono text-2xl">{character.resistances[resistance]}</div>
						<div class="text-sm font-light text-surface-400">{resistance}</div>
					</div>
				{/each}
			</div>
		</svelte:fragment>

		{#if items.length > 0}
			<div class="grid grid-cols-3 gap-6">
				{#each items as item}
					<div class="variant-soft-surface rounded p-6 text-center">
						<img
							class="mx-auto h-20 object-contain"
							src={item.imageUrl}
							alt={item.name}
							width="32"
							height="32"
						/>

						<div class="font-bold" class:text-yellow-500={item.quality === 'yellow'}>
							{item.name}
						</div>
						<div>{item.base_name}</div>
						<div>{item.quality}</div>
						<ul>
							{#each item.properties.split(',') as property}
								{#if property.trim()}
									<li>{property}</li>
								{/if}
							{/each}
						</ul>
					</div>
				{/each}
			</div>
		{:else}
			<p>No items found.</p>
		{/if}

		<svelte:fragment slot="footer">
			Updated {character.update_time}
			Seconds played {character.seconds_played}
		</svelte:fragment>
	</CharacterLayout>
{/if}
