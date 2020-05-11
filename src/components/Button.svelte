<script>
    import { createEventDispatcher } from 'svelte';
    
    const dispatch = createEventDispatcher();

    function handleResponse(answer) {
        dispatch('response', { answer });
    }    
    
    export let className;
    export let answer;
</script>

<button class="{className}" on:click="{() => handleResponse(answer)}">
    <slot></slot>
</button>

<style>
    .btn--primary {
        --backgroundColor: 117, 181, 213;
    }
    
    .btn--secondary {
        --backgroundColor: 193, 119, 190;
    }
</style>