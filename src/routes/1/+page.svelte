<script>
	// #1: the legacy way still works
	let counter = 1;
	function increment() {
		counter++;
	}

	// #2: unless you used the new way
	//     uncomment to see the warnings, and the old way will not work
	// let counter2 = $state(1);
	// function increment2() {
	//     counter2++;
	// }
</script>

<div>
	<button onclick={increment}>Increment With legacy</button>
	<p>Counter With legacy: {counter}</p>
</div>

<!-- <div>
    <button onclick={increment2}>Increment With new</button>
    <p>Counter With new: {counter2}</p>
</div> -->
