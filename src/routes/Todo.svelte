<script>
	import { slide, crossfade } from 'svelte/transition';
	import { flip } from 'svelte/animate';

    /** @type {{name: string, completed: boolean, id: number}} */
    export let todo

</script>

<div>
    <input bind:value={todo.name} />
    <input bind:checked={todo.completed} type="checkbox" />
</div>

<style>
    input {
        border: none;
    }
</style>