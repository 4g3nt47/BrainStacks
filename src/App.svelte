<script>

  /*
    This is the main component for the application.
  */
  
  import Tabs from './components/Tabs.svelte';
  import Button from './components/Button.svelte';
  import FlashCardList from './components/FlashCardList.svelte';
  import NewCard from './components/NewCard.svelte';
  import About from './components/About.svelte';
  import Footer from './components/Footer.svelte';
  import Logo from './assets/logo.png';

  // The tabs used by the app.
  const tabs = ["Flash Cards", "Add Card", "About"];
  let activeTab = tabs[0];

  // Handles tab switch.
  const changeTab = (e) => {
    activeTab = e.detail;
  };

  // Load flash cards from localStorage.
  const loadCards = () => {

    if (localStorage){
      let data = localStorage.getItem("flashCards");
      if (data)
        return JSON.parse(data);
    }
    return {};
  };

  // Save flash cards to local storage. Return true on success.
  const saveCards = () => {

    if (localStorage){
      localStorage.setItem("flashCards", JSON.stringify(flashCards));
      return true;
    }
    return false;
  };

  // Create a numeric ID for a new flash card in the given category.
  const makeCardID = (category) => {

    let id = 1;
    if (flashCards[category] !== undefined){
      id = flashCards[category].length + 1;
      while (true){
        // Check if the ID exists.
        let found = false;
        for (let i = 0; i < flashCards[category].length; i++){
          let card = flashCards[category][i];
          if (card.id === id){
            found = true;
            break;
          }
        }
        if (!found)
          break;
        id++;
      }
      return id;
    }
  };

  // Add a new flash card to the collection.
  const addCard = (e) => {

    let card = e.detail;
    let id = makeCardID(card.category);
    if (!flashCards[card.category])
      flashCards[card.category] = [{id, question: card.question, answer: card.answer}];
    else
      flashCards[card.category] = [{id, question: card.question, answer: card.answer}, ...flashCards[card.category]];
    saveCards();
    activeTab = tabs[0];
    currentCategory = card.category;
  };

  // Delete a flash card.
  const deleteCard = (e) => {

    const {category, id} = e.detail;
    flashCards[category] = flashCards[category].filter(card => card.id !== id);
    saveCards();
  };

  // Delete a category.
  const deleteCategory = (e) => {

    delete flashCards[e.detail];
    flashCards = flashCards; // Svelte needs this re-assignment.
    let categories = Object.keys(flashCards);
    if (categories.length == 0){
      currentCategory = "general"
      flashCards = {};
      flashCards[currentCategory] = [];
    }else{
      currentCategory = categories[0];
    }
    saveCards();
  };

  // Setup the session.

  let flashCards = loadCards();
  let categories = Object.keys(flashCards);
  let currentCategory = "";
  if (categories.length == 0){
    // No categories available. Create the default one.
    currentCategory = "general";
    flashCards[currentCategory] = [];
  }else{
    // Use the first category.
    currentCategory = categories[0];
  }

</script>

<main>

  <header>
    <img src={Logo} alt="">
    <h2>BrainStacks</h2>
    <Tabs {tabs} {activeTab} on:changeTab={changeTab} />
  </header>

  {#if (activeTab === "Flash Cards")}
    <div class="categories">
      <label for="category">Category:</label>
      <select name="category" id="category" bind:value={currentCategory}>
        {#each Object.keys(flashCards) as category}
          <option value={category}>{category}</option>
        {/each}
      </select>      
    </div>
    <FlashCardList category={currentCategory} flashCards={flashCards[currentCategory]} on:deleteCard={deleteCard} on:deleteCategory={deleteCategory} />
  {:else if (activeTab === "Add Card")}
    <NewCard category={currentCategory} on:addCard={addCard} />
  {:else if (activeTab === "About")}
    <About />
  {/if}

  <Footer />

</main>

<style lang="scss">

  main{
    width: 80%;
    margin: 0px auto;
    border: 2px solid black;
    border-radius: 5px;
    padding: 5px;
  }

  header{
    padding: 0px;
    margin-bottom: 2.5em;
    border-radius: 5px;
    background-color: hsl(216, 77%, 15%);
    text-align: center;
    h2{
      color: hsl(184, 100%, 48%);
      text-align: center;
      font-size: 16pt;
    }
  }

  .categories{
    text-align: center;
    margin-bottom: 2em;
    color: hsl(216, 77%, 10%);
    font-size: 13pt;
    select{
      border: none;
      padding: 5px 5px;
      border-radius: 5px;
      background-color: hsl(216, 70%, 15%);
      color: hsl(184, 100%, 48%);
    }
  }

  // Responsive styles.

  @media screen and (max-width: 1090px){
    main{
      width: 100%;
    }

    header h2{
      font-size: 14pt;
    }

    .categories, .categories select{
      font-size: 11pt;
    }

    .categories select{
      padding: 1px 10px;
    }
  }

  @media screen and (max-width: 780px){

    main{
      border: 1px solid black;
    }
    
    header h2{
      font-size: 10pt;
    }

    .categories, .categories select{
      font-size: 10pt;
    }

    .categories select{
      padding: 1px 8px;
    }
  }
</style>