<script>

  /*
    A component for making page tabs.
  */

  import {createEventDispatcher} from 'svelte';
  export let tabs = [];
  export let activeTab = "";
  const dispatch = createEventDispatcher();

  // Dispatch a custom event when the tab is switched.
  const changeTab = (tab) => {
    if (tab !== activeTab)
      dispatch("changeTab", tab);
  };
</script>

<ul>
  {#each tabs as tab}
    <li class:active={tab === activeTab} on:click={() => changeTab(tab)}>{tab}</li>
  {/each}
</ul>

<style lang="scss">
  ul{
    list-style-type: none;
    padding: 0;
    text-align: center;
    li{
      font-size: 12pt;
      display: inline-block;
      margin: 0px 20px;
      padding: 5px 10px;
      cursor: pointer;
      color: hsl(184, 100%, 48%);
      font-weight: bold;
    }
    li.active{
      font-size: 12pt;
      background-color: hsl(184, 30%, 40%);
      color: black;
      border-radius: 6px 6px 0px 0px;
    }
  }

  // Responsive styles

  @media screen and (max-width: 1090px){
    ul li, ul li.active{
      margin: 0px 10px;
      font-size: 11pt;
    }
  }

  @media screen and (max-width: 780px){
    ul li, ul li.active{
      font-size: 7pt;
    }
  }
</style>