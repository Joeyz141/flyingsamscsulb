<script lang="ts">
    import { onDestroy } from "svelte";
    import { animateScroll } from "svelte-scrollto-element";
    import { shadow } from "$lib/utils/constants";
    import { fade, slide } from "svelte/transition";
  
    animateScroll.setGlobalOptions({
      onStart: (element, offset) => {
        open = false;
      },
    });
  
    interface Route {
      id: string;
      route: string;
      name: string;
      title: string;
      color?: string;
    }
  
    export let pages: Route[] = [
      {
        id: "#home",
        route: "/home",
        name: "home",
        title: "Home",
      }, 
      {
        id: "#work",
        route: "/work",
        name: "work",
        title: "Experience",
      },
      // {
      //   id: "#projects",
      //   route: "/projects",
      //   name: "projects",
      //   title: "Projects",
      // },
      // {
      //   id: "#skills",
      //   route: "/skills",
      //   name: "skills",
      //   title: "Skills",
      // },
     
      {
        id: "#about",
        route: "/about",
        name: "about",
        title: "About",
      },
    ];
    const menuDuration = 200;
    let open = false;
    let colors = {
      nav: "bg-zinc-300 dark:bg-zinc-900",
      button: {
        active:
          "bg-zinc-200 hover:bg-zinc-300 dark:bg-zinc-800 dark:hover:bg-zinc-700 shadow-md",
        inactive:
          "bg-zinc-50 hover:bg-zinc-200 dark:bg-zinc-700 dark:hover:bg-zinc-600 shadow-md",
      },
      buttonText: "text-zinc-800 dark:text-zinc-200",
    };
  
    let activeHash = "";
    let observer: IntersectionObserver;
    let innerHeight: number;
  
    function scrollSpy(height: number, navHeight: number = 64) {
      observer?.disconnect();
      observer = new IntersectionObserver(
        (entries) =>
          entries
            .filter((e) => e.isIntersecting)
            .reverse()
            .forEach((entry) => {
              activeHash = "#" + entry.target.id;
            }),
        {
          rootMargin: `${navHeight}px 0px -${height - navHeight}px 0px`,
        }
      );
  
      pages.forEach((route) =>
        observer.observe(document.getElementById(route.name))
      );
    }
  
    $: if (innerHeight) {
      scrollSpy(innerHeight);
    }
  
    onDestroy(() => {
      observer?.disconnect();
    });
  </script>
  
  <svelte:window bind:innerHeight />
  <!-- This example requires Tailwind CSS v2.0+ -->
  <nav class="fixed top-0 w-full {shadow} {colors.buttonText} {colors.nav} z-50">
    <div class="px-2 max-w-7xl sm:px-6 lg:px-8">
      <div class="relative flex items-center justify-between h-16">
        <div class="absolute inset-y-0 left-0 flex items-center sm:hidden">
          <!-- Mobile menu button-->
          <button
            on:click={() => (open = !open)}
            type="button"
            class="inline-flex items-center justify-center p-2 rounded-md
                      {colors.button.inactive}"
            aria-controls="mobile-menu"
            aria-expanded={open}
          >
            {#if !open}
              <span class="sr-only">Open main menu</span>
              <svg
                class="w-6 h-6"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                aria-hidden="true"
                in:fade={{ duration: menuDuration }}
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M4 6h16M4 12h16M4 18h16"
                />
              </svg>
            {:else}
              <span class="sr-only">Close main menu</span>
              <svg
                class="w-6 h-6"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                aria-hidden={!open}
                in:fade={{ duration: menuDuration }}
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M6 18L18 6M6 6l12 12"
                />
              </svg>
            {/if}
          </button>
        </div>
        <!-- expanded nav page -->
        <div
          class="flex items-center justify-center flex-1 sm:items-stretch sm:justify-start"
        >
          <div class="hidden sm:block">
            <div class="flex space-x-4">
              <!-- Current: "bg-neutral-900 text-white", Default: "text-neutral-300 hover:bg-neutral-700 hover:text-white" -->
              {#each pages as route}
                <a
                  href={route.id}
                  on:click={() => {
                    animateScroll.scrollTo({
                      element: document.querySelector(route.id),
                    });
                  }}
                  class="px-4 py-2 text-sm font-medium rounded-md
                                  {activeHash === route.id
                    ? colors.button.active
                    : colors.button.inactive ?? colors.button.inactive}"
                  aria-current="page">{route.title}</a
                >
              {/each}
            </div>
          </div>
        </div>
      </div>
    </div>
  
    <!-- Mobile menu, show/hide based on menu state. -->
    {#if open}
      <div
        class="sm:hidden"
        id="mobile-menu"
        transition:slide={{ duration: menuDuration }}
      >
        <div class="px-2 pt-2 pb-3 space-y-1">
          {#each pages as route}
            <a
              href={route.id}
              on:click={() => {
                open = false;
                animateScroll.scrollTo({
                  element: document.querySelector(route.id),
                });
              }}
              class="{colors.buttonText} block px-3 py-2 rounded-md text-base font-medium
                      {activeHash === route.id
                ? colors.button.active
                : colors.button.inactive ?? colors.button.inactive}"
              aria-current="page"
            >
              {route.title}
            </a>
          {/each}
        </div>
      </div>
    {/if}
  </nav>