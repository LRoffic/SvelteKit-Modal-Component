<script lang="ts">
	import "../app.css";
	import 'animate.css';
	
	import hljs from "highlight.js";
	
	import Modal from "$components/Modal.svelte";

	let content = "";
	let showCancel = false;
	let showCloseButton = false
	let ModalTitle = "";
	let ModalSize = "md";

	let exampleModal;

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
	}
	
	$: source = " <script>\n \
	import Modal from \"$components/Modal.svelte\";\n \
	\n \
	let myModal;\n \
<\/script>\n \
<Modal bind:this={myModal}"+options+">"+content+"<\/Modal>";
	
	$: highlighted = hljs.highlightAuto(source).value;
	
	let showCode = false;
</script>
<div class="my-4">
	<input type="text" bind:value={ModalTitle} class="m-4 border-2 border-black rounded-lg" placeholder="Modal Title here">
</div>
<div class="my-4">
	<textarea class="m-4 border-2 border-black rounded-lg" bind:value={content} placeholder="Modal Body here"></textarea>
</div>
<div class="my-4">
	<input type="checkbox" bind:checked={showCancel} id="showCancel">
	<label for="showCancel">Show Cancel Button</label>
</div>
<div class="my-4">
    <input type="checkbox" bind:checked={showCloseButton} id="showCloseButton">
    <label for="showCloseButton">Show Close Button</label>
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

<button on:click={() => (exampleModal.show())} class="p-4 m-4 border-4 border-black rounded-lg hover:bg-black hover:text-white">Show Example Modal</button>
<Modal bind:this={exampleModal} {ModalTitle} {showCancel} {showCloseButton} {ModalSize}>{@html content}</Modal>

<span class="text-blue-500 hover:cursor-pointer" on:click={() => showCode = !showCode}>Show code</span>
{#if showCode}
	<pre class="p-0 m-6 text-white bg-gray-800 rounded-lg"><code class="svelte">{@html highlighted}</code></pre>
{/if}