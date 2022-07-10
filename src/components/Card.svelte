<div class="card">
  <slot></slot>
</div>

<style lang="scss">
  .card{
    border-radius: 11px;
    border: 2px solid black;
    padding: 1em;
    box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.7);
  }

  @media screen and (max-width: 780px){
    .card{
      border: 1px solid black;
      padding: 15px;
    } 
  }


  @media screen and (max-width: 780px){
    .card{
      border: 1px solid black;
      padding: 13px;
    } 
  }
</style>