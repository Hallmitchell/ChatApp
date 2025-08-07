<script lang="ts">
  import {currentUser, pb} from './pocketbase';
 

    function signOut(){
            pb.authStore.clear();
        }   
</script>
{#if $currentUser}
    
    <button on:click={signOut}>Sign Out!</button>
    
{/if}
