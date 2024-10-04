<script>
	import { page } from '$app/stores';
	import {quintOut} from "svelte/easing";
	import { slide } from "svelte/transition";
	import {onMount} from "svelte";

	// import logo from '$lib/img/logo.svg';

	const tabs = [
		{ name: 'Home', url: '/' },
		{ name: 'about', url: '/about' },
		{ name: 'login', url: '/login' }
	];

	let menuOpen = false;
	onMount(() => {
		menuOpen = window.innerWidth > 800;

		// re-open menu if resized to desktop
		window.addEventListener('resize', () => {
			menuOpen = window.innerWidth > 800;
		});
	});
</script>

<header>
	<div class="logo">
		<a href="/">
			<!--			<img src="{logo}" alt="" />-->
		</a>
	</div>

	<nav>
		{#if menuOpen}
			<ul transition:slide={{  duration: 300, easing: quintOut, axis: 'y' }}>
				{#each tabs as tab}
					<li aria-current={$page.url.pathname === tab.url ? 'page' : undefined}>
						<a href="{tab.url}" on:click={() => {menuOpen = window.innerWidth > 800 }}>{tab.name}</a>
					</li>
				{/each}
			</ul>
		{/if}

		<label class="hamburger">
			<input type="checkbox" id="menu-toggle" bind:checked={menuOpen} >
			<svg viewBox="0 0 32 32">
				<path class="line line-top-bottom" d="M27 10 13 10C10.8 10 9 8.2 9 6 9 3.5 10.8 2 13 2 15.2 2 17 3.8 17 6L17 26C17 28.2 18.8 30 21 30 23.2 30 25 28.2 25 26 25 23.8 23.2 22 21 22L7 22"></path>
				<path class="line" d="M7 16 27 16"></path>
			</svg>
		</label>
	</nav>
</header>

<style>
	header {
		display: flex;
		z-index: 10;
		padding: 2em clamp(0em, 2vw, 5em);
		justify-content: space-between;
	}

	nav a {
		display: flex;
		height: 90%;
		align-items: center;
		padding: 0 2em;
		color: var(--color-bg-1);
		font-weight: 700;
		font-size: 1rem;
		text-transform: uppercase;
		letter-spacing: .2em;
		text-decoration: none;
		transition: .3s;
	}

	li:last-of-type a {
		color: var(--color-bg-2);
		background-color: var(--color-bg-1);
		border-radius: 2em;
		margin: 0 0 0 2em;
		text-decoration: none;
	}

	.logo a {
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100%;
	}

	.logo img {
		margin-left: 2vw;
		height: 3em;
		object-fit: contain;
	}

	/* Burger menu button */
	.hamburger {
		display: none;
		z-index: 11;
		cursor: pointer;
		margin-left: 1.2rem;
	}

	.hamburger input {
		display: none;
	}

	.hamburger svg {
		height: 3.2em;
		transition: transform 600ms cubic-bezier(0.4, 0, 0.2, 1);
	}

	.line {
		fill: none;
		stroke: var(--color-bg-1);
		stroke-linecap: round;
		stroke-linejoin: round;
		stroke-width: 3;
		transition: stroke-dasharray 600ms cubic-bezier(0.4, 0, 0.2, 1),
		stroke-dashoffset 600ms cubic-bezier(0.4, 0, 0.2, 1);
	}

	.line-top-bottom {
		stroke-dasharray: 12 63;
	}

	.hamburger input:checked + svg {
		transform: rotate(-45deg);
	}

	.hamburger input:checked + svg .line-top-bottom {
		stroke-dasharray: 20 300;
		stroke-dashoffset: -32.42;
	}

	/* nav */
	nav {
		display: flex;
		justify-content: center;
		view-transition-name: navbar;
	}

	ul {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 0;
		margin: 0;
		height: 3em;
	}

	li {
		position: relative;
		height: 100%;
	}

	li[aria-current='page']::before {
		content: '';
		width: 30px;
		height: 3px;
		position: absolute;
		top: 0;
		left: calc(50% - 15px);
		background-color: var(--color-bg-1);
		view-transition-name: indicator;
	}

	a:hover {
		color: var(--color-theme-1);
	}

	@media (max-width: 800px) {
		.hamburger {
			display: block;
		}

		li[aria-current='page']::before {
			display: none;
		}

		ul {
			position: fixed;
			top: 0;
			left: 0;
			width: 100vw;
			height: 100vh;
			display: flex;
			padding-top: 6rem;
			flex-direction: column;
			justify-content: start;
			align-items: start;
			gap: 1.2rem;
			-webkit-backdrop-filter: blur(8px);
			backdrop-filter: blur(8px);
		}

		ul::after {
			content: "";
			position: fixed;
			top: 0;
			left: 0;
			width: 100vw;
			height: 100vh;
			background-color: var(--color-bg-1);
			opacity: .4;
			z-index: -11;
			-webkit-backdrop-filter: blur(8px);
			backdrop-filter: blur(8px);
		}

		li {
			display: flex;
			justify-content: center;
			width: 100%;
			height: 3em;
		}

		li:last-of-type a {
			color: var(--color-bg-0);
			background-color: var(--color-theme-1);
			border-radius: 0;
			margin: 0;
			width: 100%;
			justify-content: center;
		}

		nav a {
			display: flex;
			height: 90%;
			align-items: center;
			padding: 0 2em;
			color: var(--color-theme-1);
			font-weight: 700;
			font-size: 1.8rem;
			text-transform: uppercase;
			letter-spacing: .2em;
			text-decoration: none;
		}
	}
</style>
