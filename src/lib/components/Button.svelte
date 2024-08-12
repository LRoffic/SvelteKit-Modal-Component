<script>
	/**
	 * Author : LRoffic
	 * Author Github Profile : https://github.com/LRoffic
	 * Repository : https://github.com/LRoffic/SvelteKit-Modal-Component
	 * Version : DEV-1.0.01
	*/
	export let type;
	export let disabled = false;
	export let hoverColor, onClick, title, customClass;

	let buttonClass;
	
	let primaryButton = "bg-slate-900 rounded-lg p-4 ";
	let secondaryButton = "bg-slate-700 rounded-lg p-4 ";
	let buttonDisabled = " opacity-50 cursor-not-allowed";

	let buttonHover = "hover:bg-green-400 ";
	let buttonHoverRed = "hover:bg-red-500 ";

	if(type === "Secondary")
	    buttonClass = secondaryButton;
	else
	    buttonClass = primaryButton;

	if(disabled)
	    buttonClass += buttonDisabled;
	else{
		if(hoverColor === "Red")
			buttonClass += buttonHoverRed;
		else
			buttonClass += buttonHover;
	}

	buttonClass += customClass;
</script>

{#key buttonClass}
	<button class={buttonClass} on:click={onClick} {title}>
		<slot>Button</slot>
	</button>
{/key}