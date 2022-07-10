<script>
  /*
    This components displays our 'about' page.
  */
  import {scale} from 'svelte/transition';
</script>

<div class="about" in:scale>
  <p><strong>BrainStacks</strong> is a simple Svelte application for creating flash cards to help you study.
  Each flash card has a question and an answer, and must also be assigned a category, which helps
  group the cards into various topics.
  <br><br>
  The application does not have a backend, so all flash cards are stored persistently using the
  browser's local storage, which means all cards will be lost if a user clears this data.
  </p>
</div>  

<style lang="scss">
  .about{
    width: 60%;
    margin: 0px auto;
  }

  @media screen and (max-width: 1090px){
    .about{
      width: 70%;
      font-size: 13pt;
    }
  }

  @media screen and (max-width: 780px){
    .about{
      width: 85%;
      font-size: 8pt;
    } 
  }
</style>