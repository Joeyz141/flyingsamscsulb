<script lang="ts">
	import '../app.postcss';
	import { AppShell, AppBar } from '@skeletonlabs/skeleton';
	import { LightSwitch } from '@skeletonlabs/skeleton';
						
	// Highlight JS
	import hljs from 'highlight.js/lib/core';
	import 'highlight.js/styles/github-dark.css';
	import { storeHighlightJs } from '@skeletonlabs/skeleton';
	import xml from 'highlight.js/lib/languages/xml'; // for HTML
	import css from 'highlight.js/lib/languages/css';
	import javascript from 'highlight.js/lib/languages/javascript';
	import typescript from 'highlight.js/lib/languages/typescript';

	hljs.registerLanguage('xml', xml); // for HTML
	hljs.registerLanguage('css', css);
	hljs.registerLanguage('javascript', javascript);
	hljs.registerLanguage('typescript', typescript);
	storeHighlightJs.set(hljs);

	// Floating UI for Popups
	import { computePosition, autoUpdate, flip, shift, offset, arrow } from '@floating-ui/dom';
	import { storePopup } from '@skeletonlabs/skeleton';
	storePopup.set({ computePosition, autoUpdate, flip, shift, offset, arrow });

	import Home from '../components/Home.svelte'

	import { writable } from 'svelte/store';

let menuOpen = writable(false); // This will control the menu state

</script>

<!-- App Shell -->
<AppShell>
	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar>
			<svelte:fragment slot="lead">
				<strong class="text-xl uppercase">Flying Sams CSULB</strong>
				<br><br>
				<LightSwitch />
			</svelte:fragment>
			<svelte:fragment slot="trail">
				<a
					class="btn btn-sm variant-ghost-surface"
					href="/"
				
				>
					Home
				</a>
				<a
					class="btn btn-sm variant-ghost-surface"
					href="/about"
				>
					About
				</a>
				<a
					class="btn btn-sm variant-ghost-surface"
					href="/calendar"
				
				>
					Calendar
				</a>
				<a
					class="btn btn-sm variant-ghost-surface"
					href="/board"
				
				>
					Board
				</a>
			</svelte:fragment>
		</AppBar>

		
		  
	</svelte:fragment>

	<slot />
</AppShell>
 
<style>
	.hamburger {
	  background: none;
	  border: none;
	  font-size: 24px;
	  cursor: pointer;
	  display: none;
	}
  
	@media (max-width: 768px) {
	  .hamburger {
		display: block; /* Show only on small screens */
	  }
	}
  </style>
  