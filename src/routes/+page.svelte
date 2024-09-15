<script lang="ts">
	import "../app.css";
	import 'animate.css';
	import "bootstrap-icons/font/bootstrap-icons.css";
	
	import hljs from "highlight.js";
	
	import Modal from "$components/Modal.svelte";

	let exampleModal;
	
	let content = "";
	let showCancel = false;
	let showCloseButton = false
	let ModalTitle = "";
	let ModalSize = "md";
	let animations = true;

	let options = "";

	$:{
		options = "";

		if(ModalTitle)
			options += " ModalTitle=\""+ModalTitle+"\"";
		
		if(showCancel)
	    	options += " showCancel={true}";
		
        if(showCloseButton)
		    options += " showCloseButton={true}";

		if(ModalSize !== "md")
			options += " ModalSize=\""+ModalSize+"\"";

		if(!animations)
		    options += " animations={false}";
	}
	
	$: source = " <script>\n \
	import Modal from \"$components/Modal.svelte\";\n \
	\n \
	let myModal;\n \
<\/script>\n\n \
<button on:click={myModal.show()}>Show Example Modal</button>\n\n \
<Modal bind:this={myModal}"+options+">"+content+"<\/Modal>";
	
	$: highlighted = hljs.highlightAuto(source).value;
	
	let showCode = false;
</script>

<svelte:head>
	<title>Modal generator</title>
</svelte:head>

<div class="text-2xl text-center">
	Modal generator
</div>
<div class="m-4">
	<input type="text" bind:value={ModalTitle} class="p-2 border-2 border-black rounded-lg" placeholder="Modal Title here">
</div>
<div class="m-4">
	<textarea class="p-2 border-2 border-black rounded-lg" bind:value={content} placeholder="Modal Body here"></textarea>
</div>
<div class="m-4">
	<input type="checkbox" bind:checked={showCancel} id="showCancel">
	<label for="showCancel">Show Cancel Button : {showCancel ? "Yes" : "No"}</label>
</div>
<div class="m-4">
    <input type="checkbox" bind:checked={showCloseButton} id="showCloseButton">
    <label for="showCloseButton">Show Close Button : {showCloseButton ? "Yes" : "No"}</label>
</div>
<div class="m-4">
	<input type="checkbox" id="animations" bind:checked={animations}>
	<label for="animations">Modal animation (with 
		<a href="https://animate.style/" class="font-semibold text-blue-400 hover:underline" target="_blank">AnimateCSS</a>
	) : {animations ? "Yes" : "No"}</label>
</div>
<div class="my-4">
	<label for="size"></label>
	<select name="size" id="size" class="m-4 border-2 border-black rounded-lg" bind:value={ModalSize}>
		<option value="sm">sm</option>
		<option value="md">md</option>
		<option value="lg">lg</option>
		<option value="xl">xl</option>
	</select>
</div>

<hr />

<button on:click={exampleModal.show()} class="relative px-4 py-2 m-4 overflow-hidden border-4 border-black rounded-lg shadow-2xl before:ease before:absolute before:left-0 before:-ml-2 before:h-48 before:w-48 before:origin-top-right before:-translate-x-full before:translate-y-12 before:-rotate-90 before:bg-gray-900 before:transition-all before:duration-300 hover:text-white hover:shadow-black hover:before:-rotate-180">
	<span class="relative z-10">Show Example Modal</span>
</button>
<Modal bind:this={exampleModal} {ModalTitle} {showCancel} {showCloseButton} {ModalSize} {animations}>{@html content}</Modal>

<span class="font-semibold text-blue-400 hover:cursor-pointer hover:underline" on:click={() => showCode = !showCode}>
	Show code 
	{#if showCode}
		<i class="bi bi-caret-up-fill"></i>
	{:else}
		<i class="bi bi-caret-down-fill"></i>
	{/if}
</span>
{#if showCode}
	<pre class="p-0 m-6 text-white bg-gray-800 rounded-lg"><code class="svelte">{@html highlighted}</code></pre>
{/if}