<script>
// @ts-nocheck

	import {onMount} from "svelte";
	import gsap from "gsap";
	import Bounded from "$lib/components/Bounded.svelte";
	import { PrismicRichText, PrismicText } from "@prismicio/svelte";
	import LogoBackground from "./LogoBackground.svelte";
	import StylizedLogoMark from "./StylizedLogoMark.svelte";

	import background from "./background.jpg";

	import clsx from "clsx";
	import IconNpm from '~icons/fa6-brands/npm';
	import IconFigma from '~icons/fa6-brands/figma';
	import IconFly from '~icons/fa6-brands/fly';
	import IconCloudflare from '~icons/fa6-brands/cloudflare';
	import IconGithub from '~icons/fa6-brands/github';
	import IconDigitalOcean from '~icons/fa6-brands/digital-ocean';

	const icons = {
		digitalocean: IconDigitalOcean,
		cloudflare: IconCloudflare,
		npm: IconNpm,
		github: IconGithub,
		figma: IconFigma,
		fly: IconFly
	};

	/** @type {import("@prismicio/client").Content.IntegrationsSlice} */
	export let slice;

	onMount(() => {

		const prefersReducedMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;

		if (prefersReducedMotion) return;

		const tl = gsap.timeline({
			defaults: { ease: "power1.inOut" },
            repeat: -1,
		});

		tl.to(".pulsing-logo", {
			keyframes: [
				{
					filter: "brightness(2)",
					opacity: 1,
					duration: 0.4,
					ease: "power2.in"
				},
				{
					filter: "brightness(1)",
                    opacity: 0.7,
                    duration: 0.9,
				}
			]
		});

		tl.to(".signal-line", {
			keyframes: [
				{backgroundPosition:"0% 0%"},
				{
					backgroundPosition:"100% 100%",
					stagger: { from: "center", each: 0.3 },
					duration: 1,
				}
			]
		},
		"-=1.4"
		);

		tl.to(".pulsing-icon", {
			keyframes: [
				{
					opacity: 1,
					stagger: {
						from: "center", each: 0.3
					},
					duration: 1,
				},
				{
					opacity: 0.4,
					stagger: {
						from: "center", each: 0.3
					},
					duration: 1,
				}
			]
		},
	    "-=2"
        );
	});
</script>

<Bounded data-slice-type={slice.slice_type} data-slice-variation={slice.variation} class="relative overflow-hidden">
	<img src={background} alt="" class="absolute inset-0 h-full w-full object-cover" />
	<LogoBackground />

	<div class="relative">
		<h2
			class="mx-auto max-w-full text-balance bg-gradient-to-b from-violet-50 to-violet-300 bg-clip-text py-2 text-center text-5xl fton-medium text-transparent md:text-7xl">
			<PrismicText field={slice.primary.heading} />
		</h2>

		<div class="mx-auto mt-6 max-w-md text-balance text-center text-gray-300">
			<PrismicRichText field={slice.primary.body} />
		</div>

		<div class="flex flex-col items-center md:flex-row">
			{#each slice.primary.integrations_box as item, index}
				{#if item.icon}
					{#if index === Math.floor(slice.primary.integrations_box.length/2)}
						<StylizedLogoMark />
						<div class="signal-line rotate-180"></div>
					{/if}
					<!-- Render content for item -->
					<div class="pulsing-icon rounded-full flex aspect-square shrink-0 items-center justify-center border border-violet-50/30
					bg-violet-50/25 p-3 text-3xl text-violet-100 opacity-40 md:text-3xl lg:text-5xl">
						<svelte:component this={icons[item.icon]} />
					</div>
					{#if index !== slice.primary.integrations_box.length -1}
					<div class={clsx("signal-line", index >= Math.floor(slice.primary.integrations_box.length/2) ? "rotate-180" : "rotate-0")}></div>
					{/if}
				{/if}
			{/each}
		</div>
	</div>
</Bounded>
