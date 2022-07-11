<script>

  /*
    This component displays flash cards for the active category.
  */

  // Imports.
  import {createEventDispatcher} from 'svelte';
  import {flip} from 'svelte/animate';
  import Button from './Button.svelte';
  import Card from './Card.svelte';
  import FlashCard from './FlashCard.svelte';
  
  // The flash cards and categories are received as props.
  export let flashCards = [];
  export let category = "";

  const dispatch = createEventDispatcher();

  // Emits the event used for deleting flash cards.
  const deleteCard = (id) => {
    dispatch("deleteCard", {category, id});
  };

  // Emits the event used for deleting a category.
  const deleteCategory = () => {
    if (confirm("Delete all cards in this category?")){
      dispatch("deleteCategory", category);
    }
  };

</script>

<main>
  {#each flashCards as flashCard (flashCard.id)}
    <div class="flash-card" animate:flip={{duration: 300}}>
      <FlashCard {category} {flashCard} on:deleteCard={() => deleteCard(flashCard.id)}/>
    </div>
  {:else}
    <p class="error">No flash cards.<br> They will show up here once you create them!</p>
  {/each}
  <div class="center">
    <Button id="delete-cat-btn" danger={true} on:click={deleteCategory}>Delete Category</Button>
  </div>
</main>

<style lang="scss">
  main{
    display: grid;
    grid-template-columns: 1fr 1fr;
    align-items: start;
    grid-gap: 10px;
  }

  .error{
    grid-column: span 2;
    text-align: center;
    font-size: 13pt;
  }

  .center{
    margin-top: 1.5em;
    text-align: center;
    grid-column: span 2;
  }

  @media screen and (max-width: 1090px){
    p.error{
      font-size: 11pt;
    }
  }

  @media screen and (max-width: 780px){

    p.error{
      font-size: 9pt;
    }

    .flash-card{
      grid-column: span 2;
      width: 95%;
      margin-right: auto;
      margin-left: auto;
    }
  }

</style>