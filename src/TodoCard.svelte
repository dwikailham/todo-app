<script> 
    import {createEventDispatcher} from 'svelte'
    import {fade, fly} from 'svelte/transition'

    export let content, listname, index

    const dispatch = createEventDispatcher()
    function handleDeleteCard() {
        dispatch('deleteCard', {index})
    }

    function handleMoveRight() {
        dispatch('moveRight', {index})
    }

    function handleMoveLeft(){
        dispatch('moveLeft', {index})
    }

</script>

<div class="card mb-3 has-background-primary-light" transition:fly>
    <div class="card-content">
        <div class="columns">
            <div class="column is-1" on:click={handleMoveLeft}>
                {#if listname != 'Task'}                    
                    <span class="icon has-text-primary">
                        <i class="fa fa-chevron-left" ></i>
                    </span>
                {/if}
            </div>
            <div class="column is-9">
                {content}
            </div>
            <div class="column is-1" on:click={handleDeleteCard}>
                <span class="icon has-text-danger">
                    <i class="fa fa-trash-o"></i>
                </span>
            </div>
            <div class="column is-1" on:click={handleMoveRight}>
                {#if listname != 'Done'}                    
                    <span class="icon has-text-primary">
                        <i class="fa fa-chevron-right" ></i>
                    </span>
                {/if}
            </div>
        </div>
    </div>
</div>