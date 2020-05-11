<script>
    import { onMount } from 'svelte';
    import { fly } from 'svelte/transition';
    import { createEventDispatcher, onDestroy } from 'svelte';

    const dispatch = createEventDispatcher();
    const close = () => dispatch('close');

    let toast;
    
    export let classModifier;
    export let icon;
    export let type;
    export let text;
    export let autoClose;

    let classAndClassModifier = null;

    onMount(() => {
        classAndClassModifier = `toast toast--${classModifier}`;

        if (autoClose) {
            setTimeout(close, 1500);
        }
    });

</script>

<!-- 
    Un elemento sólo puede tener un único atributo CLASS
    por eso no podemos hacer 'class' y class="{prop}"

    así que hay que meter una propiedad computada
 -->

<output class={classAndClassModifier} in:fly="{{ y: 32, duration: 560 }}" out:fly="{{ y: -24, duration: 400 }}" role="alert">
    {#if icon}
        <svg class="toast__icon" xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-describedby="toast-title toast-description" role="img">
            {#if type === 'success'}
                <title id="toast-title">Visto</title>
                <desc id="toast-descrip">Un círculo con un visto dentro</desc>
                <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                <polyline points="22 4 12 14.01 9 11.01"></polyline>
                {:else if type === 'error'}
                <title id="toast-title">Cruz</title>
                <desc id="toast-descrip">Un círculo con una cruz dentro</desc>
                <circle cx="12" cy="12" r="10"></circle>
                <line x1="15" y1="9" x2="9" y2="15"></line>
                <line x1="9" y1="9" x2="15" y2="15"></line>
            {/if}
        </svg>
    {/if}

    <p class="toast__text">{text}</p>

    <button class="toast__close" on:click={close}>
        <svg class="toast__icon" xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
    </button>
</output>

<style>
    .toast {
        --frontColor: 255, 255, 255;
        --backgroundColor: darkgrey;
        display: inline-flex;
        align-items: center;
        padding: 1rem 1rem 1rem 1.5rem;
        border-radius: 0.25rem;
        border: 1px solid rgba(var(--backgroundColor), 0.37);
        color: rgba(var(--frontColor), 1);
        background-color: rgba(var(--backgroundColor), 0.67);
        position: fixed;
        top: 3rem;
    }

    /* TYPES */
    .toast--success {
        --backgroundColor: 72, 199, 116;
    }
    .toast--error {
        --backgroundColor: 241, 70, 104;
    }

    /* CHILDS */
    .toast > * + * {
        margin-left: 1rem;
    }

    .toast__icon {
        display: block;
        width: 1.5rem;
        height: 1.5rem;
    }    

    .toast__text {
        margin-top: 0;
        margin-bottom: 0;
    }

    .toast__close {
        padding: 1rem;
        margin: -1rem -1rem -1rem 1rem;
        border: 0;
        color: rgba(var(--frontColor), 1);
        background-color: transparent;
    }

    .toast__close:hover {
        transform: none;
    }
</style>