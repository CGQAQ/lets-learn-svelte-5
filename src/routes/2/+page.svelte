<script>
	import { untrack } from 'svelte';
	// #1.4 you can also use the runes in `.svelte.js` or `.svelte.ts` files to make shared states
	import { rune, increment2 } from './runes-in-js.svelte.js';

	// #1: the new way, using $state, an reactive state
	//     this is the recommended way to create reactive state starting from Svelte 5
	// See: https://svelte-5-preview.vercel.app/docs/runes#$state
	let counter = $state(1);
	function increment() {
		counter++;
	}

	// #1.1: $state can also be used to create an object
	let user = $state({ name: 'John', age: 30 });
	// #1.2: there's also $state.raw, which creates a shallow reactive object
	//     this means that the object is reactive, but its properties are not
	//     you can only set the whole object at once in order to trigger reactivity
	let shallowState = $state.raw(['Alice', 'Bob', 'Charlie']);
	// shallowState.push('Dave'); // this will not work, you need to set the whole array at once
	// shallowState = [...shallowState, 'Dave']; // this will work
	// #1.3: there's also $state.snapshot, which strips the original object out of the Proxy
	//     this is useful if you want to create a non-reactive version of a reactive state
	const originalObject = { name: 'John', age: 30 };
	const reactiveState = $state(originalObject);
	let snapshot = $state.snapshot(reactiveState);
	structuredClone(snapshot); // this will not throw, as snapshot it not a Proxy anymore
	// structuredClone(reactiveState); // this will throw

	// #2: derived state
	//     this is a reactive state that is derived from one or more other reactive states
	//     it will automatically update when the source reactive states change
	// See: https://svelte-5-preview.vercel.app/docs/runes#$derived
	let counterTimesTwo = $derived(counter * 2);
	// #2.1: you can also use $derived.by to create a derived state from the result of a callback
	// let counterTimesTwo = $derived.by(() => counter * 2);

	// #3: reactive effects
	//     this is a function that will be called whenever the reactive state it depends on changes
	//     the dependencies are automatically tracked, so you don't have to worry about it
	// new way of doing $: in svelte 4
	// See: https://svelte-5-preview.vercel.app/docs/runes#$effect
	$effect(() => {
		console.log('effect: counterTimesTwo is now', counterTimesTwo);
		// #3.1 you can also return a cleanup function, which will be called when the effect is disposed

		// #3.2 you can use untrack to prevent the effect from running when the reactive state changes
		//     this is useful if you want to prevent unnecessary updates, just likes below.
		// console.log("counterTimesTwo is now", untrack(() => counterTimesTwo));

		return () => {
			console.log('Component unmounted');
		};
	});

	// #3.1: you can also use $effect.pre to let the effect run before DOM updates instead
	$effect.pre(() => {
		console.log('effect.pre: counterTimesTwo is now', counterTimesTwo);
		// #3.2: you can use $effect.tracking to check if you are in a tracking context
		console.log('Are we in tracking context(inside $effect.pre)?', $effect.tracking()); // true
	});

	// #3.2: you can use $effect.tracking to check if you are in a tracking context
	console.log('Are we in tracking context?(not in any effect)', $effect.tracking()); // false

	// #3.3: you can use $effect.root to create an effect root, which is a new context that will
	//     not be automatically disposed when the component unmounts
	//     you can manually dispose the root by calling cleanup()
	const cleanup = $effect.root(() => {
		console.log('effect.root: counterTimesTwo is now', counterTimesTwo);
		$effect(() => {
			console.log('effect.root: counterTimesTwo is now', counterTimesTwo);

			return () => {
				console.log('effect.root - $effect: effect unmounted');
			};
		});

		return () => {
			console.log('effect.root: Component unmounted');
		};
	});
</script>

<div>
	<button onclick={increment}>Increment the new way</button>
	<button onclick={() => cleanup()}>Dispose effect.root</button>
	<p>Counter: {counter}</p>
	<p>Counter Times Two: {counterTimesTwo}</p>

	<button onclick={increment2}>Increment the rune inside js</button>
	<p>Counter2: {rune.count2}</p>
</div>
