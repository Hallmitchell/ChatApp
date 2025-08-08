
<script lang="ts">
    
    import LogoutButton from "./LogoutButton.svelte";
    import {currentUser} from './pocketbase';
</script>
<div  style="display: flex; justify-content:space-between;">
    {#if $currentUser}
        <p>signed in as @{$currentUser.userName}</p>
    {/if}
    <LogoutButton/>
</div>