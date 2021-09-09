<script>

import { createEventDispatcher } from 'svelte'
import TodoCard from './TodoCard.svelte'
export let cards, listname

    const dispatch = createEventDispatcher()
    let todo = ""
	function handleAddCard(){
		// cards = [...cards, {todo: todo, list:'Task'}]

        dispatch('addCard', {todo, listname})
		todo = ''
	}

    function handleDeleteCard(event) {
        let data = event.detail
        dispatch('deleteCard', {index: data.index, listname})
    }

    function handleMoveRight(event){
        let data = event.detail
        dispatch('moveRight', {index: data.index, listname})
    }

    function handleMoveLeft(event) {
        let data = event.detail
        dispatch('moveLeft', {index: data.index, listname})
    }

</script>


<div class="column is-4">
    <div class="card has-background-light">
        <div class="card-header">
            <p class="card-header-title">{listname}</p>
        </div>
        <div class="card-content">
            {#each cards as card, index}
                <TodoCard 
                    content={card.todo} 
                    listname={listname} 
                    index={index} 
                    on:deleteCard={handleDeleteCard}
                    on:moveRight={handleMoveRight}
                    on:moveLeft={handleMoveLeft}/>
            {/each}
            <input type="text" class="input is-primary mb-1" bind:value={todo} />
            <button on:click={handleAddCard} class="button is-primary">Add Card</button>
        </div>
    </div>
</div>