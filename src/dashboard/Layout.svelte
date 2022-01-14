<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';
	import { browser } from '$app/env';
	import { open } from './provider/store';
	import Overlay from './provider/Overlay.svelte';
	import TopNavigation from './topnavigation/Index.svelte';
	import SideNavigation from './sidenavigation/Index.svelte';

	/*	w-[calc(100%-16rem)] class get the remain width of the main tag from lg:viewport by subtracting
(the total width by the width of the side navigation component which is w-64 = 16rem)*/

	const style = {
		open: 'lg:w-full',
		close: 'lg:pl-4 lg:w-[calc(100%-16rem)]',
		mainContainer: `flex flex-col w-full h-screen pl-0 lg:space-y-4`,
		container: `bg-gray-100 h-screen overflow-hidden relative lg:p-4`,
		main: `h-screen overflow-auto pb-36 pt-8 px-2 md:pb-8 md:pt-4 lg:pt-0`
	};

	onMount(() => {
		document.getElementsByTagName('body').item(0).removeAttribute('tabindex');
	});

	if (browser) {
		page.subscribe(() => {
			// close side navigation when route changes when viewport < 1024px
			if (window.innerWidth < 1024) {
				$open = false;
			}
		});
	}
</script>

<div class={style.container}>
	<div class="flex items-start">
		<Overlay />
		<SideNavigation mobilePosition="right" />
		<div class={`${style.mainContainer} ${$open ? style.open : style.close}`}>
			<TopNavigation />
			<main class={style.main}>
				<slot />
			</main>
		</div>
	</div>
</div>
