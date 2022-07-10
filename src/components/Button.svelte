<script>
  /*
    Our reusable button component. Contents are recieved as a slot.
  */

  // Accepts button ID as prop. 'danger' decides if a red button should be used.
  export let id;
  export let danger = false;

</script>

<button {id} class:danger={danger} on:click>
  <slot></slot>
</button>

<style lang="scss">
  button{
    padding: 2px 20px;
    line-height: 1.5em;
    font-size: 13pt;
    font-weight: bold;
    border: 1px solid black;
    border-radius: 5px;
    box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.7);
    background-color: hsl(216, 70%, 15%);
    color: hsl(184, 100%, 48%);
    cursor: pointer;
  }

  button.danger{
    background-color: hsl(0, 90%, 15%);
    color: white;
  }

  @media screen and (max-width: 1090px){
    button{
      font-size: 11pt;
      font-weight: normal;
      padding: 1px 10px;
    }
  }

  @media screen and (max-width: 780px){
    button{
      font-size: 6pt;
      padding: 1px 5px;
    } 
  }
</style>