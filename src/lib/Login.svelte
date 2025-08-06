<script lang="ts">
    import {currentUser, pb} from './pocketbase';

    let userName: string;
    let password: string;
    

    async function login(){
        await pb.collection('users').authWithPassword(userName, password);
    }

    async function signUp(){
        try{
            const data = {
                userName,
                password,
                passwordConfirm: password,
                
            };
            const createdUser = await pb.collection('users').create(data);
            await login();
        } catch (err){
            console.error(err)
        }
    }

    function signOut(){
        pb.authStore.clear();
    }

</script>

{#if $currentUser}
    <p>Signed in as {$currentUser.userName}</p>
    <button on:click={signOut}>Sign Out</button>
{:else}
    <form on:submit|preventDefault>
        <input
            placeholder="Username"
            type="text"
            bind:value={userName}
        />

        <input
            placeholder="Password"
            type="password"
            bind:value={password}
        />
       
        <button on:click={signUp}>Sign Up</button>
        <button on:click={login}>Login</button>
        

    </form>
{/if}