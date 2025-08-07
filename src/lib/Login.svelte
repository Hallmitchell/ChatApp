<script lang="ts">
    import {currentUser, pb} from './pocketbase';

    let userName: string;
    let password: string;
    let loginError: any;

    async function login(){
        try{
            await pb.collection('users').authWithPassword(userName, password);
            console.log(pb.authStore.isValid);
            console.log(pb.authStore.token);
            console.log(pb.authStore.record.id);
        } catch (err){
            console.error("Login Error", err.message)
            loginError = err; 
        }
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

</script>

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
    
    {#if loginError}
        <p style="color:red">Failed to authenticate</p>
    {/if}

</form>
