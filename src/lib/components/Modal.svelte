<script>
	/**
	 * Author : LRoffic
	 * Author Github Profile : https://github.com/LRoffic
	 * Repository : https://github.com/LRoffic/SvelteKit-Modal-Component
	 * Version : 1.0.0
	*/
	import {fade} from "svelte/transition"
	
	generate: 'ssr'
	
	import Spinner from "$components/Spinner.svelte";
	import Button from "$components/Button.svelte";

	export let visible = "hidden";
	export let isLoading = false;
	export let Buttons = [];
	export let showCancel = true;
	export let showCloseButton = true;
	export let ModalTitle = "";
	export let ModalSize = "md";

	let Modal,ModalBackdrop = "";

	let ModalBasis = "basis-6/12";

	if(ModalSize === "sm")
		ModalBasis = "basis-4/12";
	if(ModalSize === "md")
		ModalBasis = "basis-6/12";
	if(ModalSize === "lg")
		ModalBasis = "basis-8/12";
	if(ModalSize === "xl")
		ModalBasis = "basis-10/12";

	export function show() {
		ModalBackdrop = "animate__animated animate__fadeIn";

		Modal = "animate__animated animate__fadeInUp animate__duration-1s";

		visible = "visible";
	}

	export function hide() {
		isLoading = true;

		ModalBackdrop = "animate__animated animate__fadeOut animate__duration-1s"; 

		Modal = "animate__animated animate__fadeOutDown";

		setTimeout(() => {
			visible = "hidden";
			isLoading = false;
		}, 1200);
	}
</script>

<div class="fixed flex inset-0 box-border !z-[1060] bg-slate-700 bg-opacity-80 {ModalBackdrop} {visible}"  on:click|self={() => hide()}>
	<div class="relative flex flex-col m-auto bg-slate-800 shadow-xl rounded-xl {Modal} {ModalBasis} items-center">	
		{#if isLoading}
			<div class="absolute float-left size-full !z-[2080] bg-slate-200 bg-opacity-30 rounded-xl" transition:fade>
				<div role="status" class="absolute -translate-x-1/2 -translate-y-1/2 top-2/4 left-1/2">
					<svg aria-hidden="true" class="w-8 h-8 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/><path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/></svg>
					<span class="sr-only">Loading...</span>
				</div>
			</div>
		{/if}
		{#if showCloseButton}
			<button class="absolute top-0 right-0 p-4 mt-0 mr-0 rounded-tr-lg cursor-pointer hover:bg-red-500 mb--5" on:click|self={() => hide()}>
				<i class="bi bi-x-lg"></i>
			</button>
		{/if}
		<div class="flex flex-col p-4 pt-0">
			<div class="justify-center flex-auto p-3">
				{#if !ModalTitle}
					<h2 class="py-4 mb-4 text-xl font-bold text-center text-gray-200">{ModalTitle}</h2>
				{:else}
					<div class="py-4 mb-4"></div>
				{/if}
				<slot />
			</div>
			<div class="p-2">
				<div class="flex items-center justify-center space-x-1 text-center">
					{#if Buttons.length > 0}
						{#each Buttons as button}
							<Button type={button.type} onClick={button.onClick} disabled={button.disabled} hoverColor={button.hoverColor} title={button.title} customClass={button.customClass}>
								{#if button.spinner}
                                    <Spinner color={button.spinnerColor} />
                                {/if}
                                {#if button.text}
                                    {button.text}
                                {/if}
							</Button>
						{/each}
					{/if}
					{#if showCancel}
						<button class="p-4 rounded-lg bg-slate-900 hover:bg-red-500" on:click={() => hide()}>
							Cancel
						</button>
					{/if}
				</div>
			</div>
		</div>
	</div>
</div>