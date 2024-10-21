<script>
    import { state3 } from '$lib/foo.svelte.js';

    let counter = 1;

    function increment() {
        counter++;
    }

    let counter2 = $state(1);

    console.log(counter2);

    function increment2() {
        counter2++;
    }

    function increment3() {
        state3.count++;
    }


    import Input from '$lib/Input.svelte';
    let value = $state('asd');

    $inspect(value);
</script>

<div>
    <button onclick={increment}>Increment With legacy</button>
    <p>Counter With legacy: {counter}</p>
</div>
<div>
    <button onclick={increment2}>Increment With new</button>
    <p>Counter With new: {counter2}</p>
</div>
<div>
    <button onclick={increment3}>Increment With external state</button>
    <p>Counter With external state: {state3.count}</p>
</div>
<div>
    <Input bind:value={value} />
</div>