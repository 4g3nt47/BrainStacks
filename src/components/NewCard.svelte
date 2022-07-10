<script>

  /*
    This component displays the form used to create new flash cards.
  */

  import {scale} from 'svelte/transition';
  import {createEventDispatcher} from 'svelte';
  import Card from './Card.svelte';
  import Button from './Button.svelte';

  const dispatch = createEventDispatcher();

  // Accept category as prop. Default to "general".
  // The following variables are all bound to the form.
  export let category = "general";
  let question = "";
  let answer = "";

  // Validate input and emit the event used to add a new flash card.
  const addCard = () => {

    category = category.trim();
    question = question.trim();
    answer = answer.trim();
    const error = document.getElementById("error");
    if (question.length < 3){
      error.innerText = "question too short!";
      return;
    }
    if (answer.length === 0){
      error.innerText = "Answer too short!";
      return;
    }
    dispatch("addCard", {category, question, answer});
  };
  
</script>

<main>
  <Card>
    <form on:submit|preventDefault={addCard} in:scale>
      <label for="category">Category:</label>
      <input type="text" id="category" bind:value={category} spellcheck="false">
      <label for="question">Question:</label>
      <textarea id="question" cols="30" rows="3" bind:value={question} spellcheck="false"></textarea>
      <label for="answer">Answer:</label>
      <input type="text" id="answer" bind:value={answer} spellcheck="false">
      <p class="error" id="error"></p>
      <div class="center">
        <Button id="new-card-btn">Add Card</Button>
      </div>
    </form>
  </Card>
</main>

<style lang="scss">
  main{
    padding: 1em;
    margin: 1em auto;
    width: 50%;
    form label{
      display: block;
      width: 100%;
      margin-bottom: 8px;
    }

    form input[type="text"], textarea{
      border: 1px solid black;
      border-radius: 6px;
      line-height: 1.3em;
      width: 100%;
      display: block;
      font-size: 13pt;
      padding: 5px;
      margin-bottom: 10px;
      background-color: hsl(184, 30%, 65%);
    }

    p.error{
      text-align: center;
      font-style: italic;
      color: red;
    }

    .center{
      text-align: center;
    }
  }

  // Responsive styles

  @media screen and (max-width: 1090px){
    main{
      width: 70%;
      form label{
        font-size: 13pt;
      }

      form input[type="text"], textarea{
        font-size: 12pt;
      }
    }
  }

  @media screen and (max-width: 780px){
    main{
      width: 90%;
      form label{
        font-size: 10pt;
      }

      form input[type="text"], textarea{
        font-size: 8pt;
      }
    }
  }
</style>