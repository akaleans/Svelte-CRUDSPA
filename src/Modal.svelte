<script>
    // Bring in our showModal boolean, defaults to false.
    export let showModal = false;
</script>

<!-- Modal form, using <slot> as our input, sent from our App component. -->
{#if showModal}
    <div class="backdrop" on:click|self>
        <div class="modal">
            <slot></slot>
        </div>
    </div>
{/if}

<style>
    .backdrop{
        width: 100%;
        height: 100%;
        position: fixed;
        background: rgba(0, 0, 0, 0.8)
    }
    .modal{
        padding: 10px;
        border-radius: 10px;
        max-width: 600px;
        margin: 10% auto;
        text-align: center;
        background: white;
    }
</style>