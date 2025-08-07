<script lang="ts">
  import Login from "./lib/Login.svelte";
  import Messages from "./lib/Messages.svelte";
  import {currentUser} from "./lib/pocketbase";
  
</script> 

<h1 style="color:orange;">Chat Room</h1>

<p style="color:orange; style=font-size:150%;"> I appreciate your visit to my chat room. Kindly leave a message, and I will get back to you at my earliest convenience. </p>

    
<Login />
{#if $currentUser}
  <Messages/>
{/if}
