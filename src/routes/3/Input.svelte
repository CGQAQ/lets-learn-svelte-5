<script>
	// #1 you can use $props to get the props passed to the component
	//   `export let value` in svelte 4
	//  rest is the rest of the props
	//   `$$props` in svelte 4
	// use $bindable to create a bindable property (two way binding), the first argument is the initial value
	let { value = $bindable(''), ...rest } = $props();

	// #3: use $host to get the reference of the component that contains this component,
	//      this component needs to be a custom element
	// console.log("the host element is ", $host());
</script>

<input type="text" bind:value />
