<script lang="ts">
	import { Access, type User } from '$lib/data';

	const {
		user,
		access = Access.Public,
		compact = false,
		self = false,
	}: {
		user: User;
		access?: Access;
		/** If true, don't show the picture */
		compact?: boolean;
		/** Whether the user is viewing their own profile */
		self?: boolean;
	} = $props();
</script>

<div class={['user', self && 'self']} onclick={e => (e.target as Element).classList.contains('user') && (location.href = '/users/' + user.id)}>
	{#if !compact && access >= (!self && user.settings.hide_profile_picture ? Access.Protected : Access.Public)}
		<img src={user.picture} alt={user.first_name} />
	{/if}

	{user.first_name}
	{#if self || access >= (user.settings.hide_full_name ? Access.Protected : Access.Friend)}
		{user.last_name}
	{/if}
</div>

<style>
	.user {
		cursor: pointer;
		width: max-content;
		height: max-content;
	}

	img {
		width: 2em;
		height: 2em;
		border-radius: 50%;
		vertical-align: middle;
		margin-right: 0.5em;
	}
</style>
