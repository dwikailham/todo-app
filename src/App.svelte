<script>

	import CardList from './CardList.svelte'	

	let taskCardLocalStorage = JSON.parse(localStorage.getItem('taskCard'))
	let inProgressLocalStorage = JSON.parse(localStorage.getItem('inProgress'))
	let doneStorage = JSON.parse(localStorage.getItem('doneCards'))

	let taskCards = taskCardLocalStorage ? taskCardLocalStorage: []
	let inProgress = inProgressLocalStorage ? inProgressLocalStorage:  []
	let done = doneStorage ? doneStorage: []

	function handleEventAddCard(event){
		let data = event.detail
		if(data.listname == 'Task'){
			taskCards = [...taskCards, {todo: data.todo}]
			localStorage.setItem('taskCard', JSON.stringify(taskCards))
		}else if(data.listname == 'In Progress'){
			inProgress = [...inProgress, {todo: data.todo}]
			localStorage.setItem('inProgress', JSON.stringify(inProgress))
		}else{
			done = [...done, {todo: data.todo}]
			localStorage.setItem('doneCards', JSON.stringify(done))
		}
	}

	function handleEventDeleteCard(event) {
		let data = event.detail

		if(data.listname == 'Task'){
			taskCards.splice(data.index, 1)
			taskCards = taskCards
			localStorage.setItem('taskCard', JSON.stringify(taskCards))
		}else if(data.listname == 'In Progress'){
			inProgress.splice(data.index, 1)
			inProgress = inProgress
			localStorage.setItem('inProgress', JSON.stringify(inProgress))
		}else{
			done.splice(data.index, 1)
			done = done
			localStorage.setItem('doneCards', JSON.stringify(done))
		}

	}

	function handleEventMoveRight(event){
		let data = event.detail

		if(data.listname == 'Task'){
			let cardToMove = taskCards.splice(data.index, 1)
			inProgress = [...inProgress, cardToMove[0]]
			taskCards = taskCards
			localStorage.setItem('taskCard', JSON.stringify(taskCards))
			localStorage.setItem('inProgress', JSON.stringify(inProgress))
		}else if(data.listname == 'In Progress'){
			let cardToMove = inProgress.splice(data.index, 1)
			done = [...done, cardToMove[0]]
			inProgress = inProgress
			localStorage.setItem('inProgress', JSON.stringify(inProgress))
			localStorage.setItem('doneCards', JSON.stringify(done))
		}
	}

	function handleEventMoveLeft(event){
		let data = event.detail

		if(data.listname == 'In Progress'){
			let cardToMove = inProgress.splice(data.index, 1)
			taskCards = [...taskCards, cardToMove[0]]
			inProgress = inProgress
			localStorage.setItem('taskCard', JSON.stringify(taskCards))
			localStorage.setItem('inProgress', JSON.stringify(inProgress))
		}else{
			let cardToMove = done.splice(data.index, 1)
			inProgress = [...inProgress, cardToMove[0]]
			done = done
			localStorage.setItem('inProgress', JSON.stringify(inProgress))
			localStorage.setItem('doneCards', JSON.stringify(done))
		}
	}

</script>

<div class="container is-fluid">
	<h1 class="is-size-3">Todo App</h1>
	<div class="columns">
		<CardList 
			cards={taskCards} 
			listname={'Task'} 
			on:addCard={handleEventAddCard} 
			on:deleteCard={handleEventDeleteCard}
			on:moveRight={handleEventMoveRight}>
		</CardList>
		<CardList 
			cards={inProgress} 
			listname={'In Progress'} 
			on:addCard={handleEventAddCard}
			on:deleteCard={handleEventDeleteCard}
			on:moveRight={handleEventMoveRight}
			on:moveLeft={handleEventMoveLeft}>
		</CardList>
		<CardList 
			cards={done} 
			listname={'Done'} 
			on:addCard={handleEventAddCard}
			on:deleteCard={handleEventDeleteCard}
			on:moveLeft={handleEventMoveLeft}>
		</CardList>
	</div>
</div>


<style>
</style>

<svelte:head>
	<link rel="stylesheet" href="/bulma.min.css">
	<link href="http://maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css" rel="stylesheet">
	<!-- <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@5.15.3/css/fontawesome.min.css" integrity="sha384-wESLQ85D6gbsF459vf1CiZ2+rr+CsxRY0RpiF1tLlQpDnAgg6rwdsUF1+Ics2bni" crossorigin="anonymous"> -->
	<!-- <link rel="stylesheet" href="/fontawesome.min.css"> -->
</svelte:head>