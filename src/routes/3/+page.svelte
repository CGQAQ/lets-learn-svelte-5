<script>
	import Input from './Input.svelte';

	let value = $state('');

	// #2: use $inspect to inspect the value of a reactive variable
	//     this is just like
	// $effect(() => console.log(value))
	//     but more elegant
	$inspect(value);
</script>

<!-- #1.1: the child component uses $bindable to create a bindable property,
           so we can use bind:value here
-->
<Input bind:value />
