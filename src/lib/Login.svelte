<script lang="ts">
    import { currentUser, pocketbase } from "./pocketbase";
    import { Google } from 'svelte-bootstrap-icons';
    import educationSvg from '../assets/undraw_education_f8ru.svg';

    let username: string;
    let password: string;

    async function login() {
        await pocketbase.collection('users').authWithPassword(username, password);
        // clear the form
        username = '';
        password = '';
    }

    function signOut() {
        pocketbase.authStore.clear();
    }
</script>

<style>
    .container {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        height: 100vh;
    }
    .login{
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
        width: 16rem;
    }
    .input {
        width: 100%;
        padding: 0.75rem;
        border: 1px solid #ccc;
        border-radius: 0.5rem;
    }
</style>

<div class="container">
    {#if $currentUser}
        <h1>Welcome {$currentUser.username}</h1>
        <button on:click={signOut}>Sign Out</button>
    {:else}
        
        <div class="login">
            <h1>Login</h1>
            <button><Google style="padding-right: 8px;"/> Login in with Google</button>
            <input class="input" type="text" bind:value={username} placeholder="Username" />
            <input class="input" type="password" bind:value={password} placeholder="Password" />
            <button on:click={login}>Login</button>
        </div>  
    {/if}
</div>