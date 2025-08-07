<script lang="ts">
  import {currentUser, pb} from './pocketbase';
 

    function signOut(){
            pb.authStore.clear();
        }   
</script>
{#if $currentUser}
    <div style="text-align:left;">
        <button on:click={signOut}>Sign Out</button>
    </div>
{/if}
