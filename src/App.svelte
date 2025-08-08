<script lang="ts">
  import Login from "./lib/Login.svelte";
  import Messages from "./lib/Messages.svelte";
  import {currentUser} from "./lib/pocketbase";
  import Logout from "./lib/LogoutButton.svelte";
  import Header from "./lib/Header.svelte";
  import Direct from "./lib/Direct.svelte";

  let homeDirectToggleState = "ChatRoom";
  let toggleButtonText = "Direct Messages"
  function homeDirectToggle(){
    if (homeDirectToggleState=="ChatRoom"){
      homeDirectToggleState = "DirectMessages"
      toggleButtonText = "ChatRoom"
    }
    else{
      homeDirectToggleState = "ChatRoom"
      toggleButtonText = "Direct Messages"
    }
  }
</script> 

<Header/>

<h1 style="color:orange;">{homeDirectToggleState}</h1>
<button on:click={homeDirectToggle}>{toggleButtonText}</button>

<!-- I want to create a DM button that allows you to pick and choose what user to DM -->
  
{#if $currentUser}
  {#if homeDirectToggleState == "ChatRoom"}
    <Messages/>
  {:else}
    <Direct/>
    <h1>In Progress</h1>
  {/if}

{:else}
  <Login/>
{/if}

