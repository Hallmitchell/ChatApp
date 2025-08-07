
<script lang="ts">
    
    import LogoutButton from "./LogoutButton.svelte";
    import {currentUser} from './pocketbase';
</script>
<div  style="display: flex; justify-content:space-between;">
    <h1>ChatAPP</h1>
    {#if $currentUser}
        <p>signed in as @{$currentUser.userName}</p>
    {/if}
    <LogoutButton/>
</div>