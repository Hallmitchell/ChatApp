<script lang="ts">
  import Login from "./lib/Login.svelte";
  import Messages from "./lib/Messages.svelte";
  import {currentUser} from "./lib/pocketbase";
  import Logout from "./lib/LogoutButton.svelte";
  import Header from "./lib/Header.svelte";
</script> 

<Header/>

<h1 style="color:orange;">Chat Room</h1>

<p style="color:orange; font-size:150%;"> I appreciate your visit to my chat room. Kindly leave a message, and I will get back to you at my earliest convenience. </p>

  
{#if $currentUser}
  <Messages/>
{:else}
  <Login/>
{/if}

