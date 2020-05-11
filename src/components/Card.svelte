<script>
    import Button from './Button.svelte';

    export let isDisabled = true;
</script>

<section class="card" aria-disabled="{isDisabled}">
    <div class="card__content">
        <header class="card__header">
            <slot name="header">
                <p class="card__loading">
                    <span>.</span>
                    <span>.</span>
                    <span>.</span>
                </p>
            </slot>
        </header>
        <div class="card__body">
            {#if !isDisabled}
                <div class="card__actions">
                    <Button on:response className="{'btn btn--primary'}" answer="yes">
                        Sí
                    </Button>
                    <Button on:response className="{'btn btn--secondary'}" answer="no">
                        No
                    </Button>
                </div>
            {/if}
        </div>
    </div>
</section>

<style>
    @keyframes bounce {
        0%, 100% {
            transform: none;
        }
        51% {
            transform: translateY(-0.375rem);
        }
    }
    
    .card {
        position: relative;
    }
    
    .card::before,
    .card::after {
        content: '';
        display: block;
        height: 14rem;
        border-radius: 0.125rem;        
        position: absolute;
        top: 0;
        left: 0;
        z-index: 0;
    }

    .card::before {
        width: 80%;
        border: 1px solid rgb(222, 230, 232);
        background-color: rgb(247, 250, 250);
        transition: all 200ms 50ms ease-in-out;
        transform: translate3d(10%, -0.5rem, 0);
    }

    .card::after {
        width: 90%;
        border: 1px solid rgb(225, 234, 236);
        background-color: rgb(251, 253, 255);
        transition: all 200ms 100ms ease-in-out;
        transform: translate3d(5%, -0.25rem, 0);
    } 

    .card:hover::before {
        transform: translate3d(10%, -1.875rem, 0);
        transition: all 300ms cubic-bezier(.17,.67,.59,1.62);        
    }
    
    .card:hover::after {
        transform: translate3d(5%, -1.5rem, 0);
        transition: all 300ms cubic-bezier(.17,.67,.51,1.42);        
    }

    .card:hover .card__content {
        transform: translate3d(0, -1rem, 0);
        transition: all 200ms cubic-bezier(.17,.67,.51,1.42);
    }

    .card[aria-disabled="true"] {
        pointer-events: none;
    }

    .card__loading > span {
        display: inline-block;
        font-size: 1.5rem;
        animation: bounce infinite 1s;
    }

    .card__loading > span:nth-child(2) {
        animation-delay: 100ms;
    }

    .card__loading > span:nth-child(3) {
        animation-delay: 200ms;
    }


    .card__content {
        min-height: 14rem;
        max-width: 30rem;
        padding: 2rem 2rem 3rem;        
        border: 1px solid rgb(232, 240, 242);
        border-radius: 0.125rem;
        background-color: rgb(251, 254, 255);        
        position: relative;
        z-index: 1;
        transition: all 200ms ease-in-out;
    }

    .card__actions {
        margin-top: 3rem;
    }
</style>