<script>

  /*
    This component represents a single flash card.
  */

  import {createEventDispatcher} from 'svelte';
  import Button from './Button.svelte';
  import {scale, fade} from 'svelte/transition';

  // Receive the flash card an it's category as props.
  export let flashCard;
  export let category;

  const dispatch = createEventDispatcher();

  // Show/hide the answer of the flash card.
  const toggleAnswer = (id) => {

    let elem = document.getElementById("answer-" + id);
    let btn = document.getElementById("btn-" + id);
    if (elem.innerText.length == 0){
      elem.innerText = flashCard.answer;
      btn.innerText = "Hide Answer";
    }
    else{
      elem.innerText = "";
      btn.innerText = "Show Answer";
    }
  };

  // Emit the event used for deleting the flash card.
  const deleteCard = () => {
    if (confirm("Delete this card?"))
      dispatch("deleteCard", {category, id: flashCard.id});
  };
  
</script>

<div class="flash-card" in:scale out:fade|local>
  <button class="cancel" on:click={deleteCard}>X</button>
  <p class="question">
    {flashCard.question}
  </p>
  <p class="answer" id={"answer-" + flashCard.id}></p>
  <Button id={"btn-" + flashCard.id} on:click={() => toggleAnswer(flashCard.id)}>Show Answer</Button>
</div>

<style lang="scss">
  .flash-card{
    position: relative;
    text-align: center;
    border: 2px solid black;
    border-radius: 10px;
    box-shadow: 3px 3px 3px black;
    color: black;
    padding: 1em;

    p.question{
      text-align: left;
      font-size: 17pt;
    }

    p.answer{
      padding-top: 10px;
      border-top: 1px dashed black;
      font-style: italic;
    }

    button.cancel{
      border: none;
      background: none;
      color: black;
    }
  }

  button.cancel{
    position: absolute;
    top: 3px;
    right: 3px;
    font-weight: bold;
    border: none;
    cursor: pointer;
    font-size: 13pt;
    color: red;
  }

  // Responsive styles.

  @media screen and (max-width: 1090px){
    
    .flash-card{
      border: 1px solid black;
      p.question{
        font-size: 14pt;
      }

      p.answer{
        font-size: 12pt;
      }
    }

    button.cancel{
      font-size: 11pt;
    }
  }

  @media screen and (max-width: 780px){
    .flash-card{
      p.question{
        font-size: 9pt;
      }

      p.answer{
        font-size: 7pt;
      }
    }

    button.cancel{
      font-size: 8pt;
    }
  }

</style>
