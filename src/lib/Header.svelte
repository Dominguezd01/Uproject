<script>
    import UpLogo from "./UpLogo.svelte";
    import logo from '$lib/assets/UpLogoWhite.svg';
    import Dropdown from "./Dropdown.svelte";
    import { browser } from "$app/environment";
    import { onMount } from "svelte";
    const getUserId = () => {
        if (browser) {
            let userId = sessionStorage.getItem("userId");
            if (!userId || Object.keys(userId).length == 0) {
               return false
            }
            return true;
        }
    };
</script>
<header class="navbar">
    <div class="logoContainer">
        {#if getUserId() === true}
            <a href="/boards"><img class="headerLogo" width="100px" src={logo} alt="Logo of the app"></a> 
        {:else}
            <a href="/auth/login"><img class="headerLogo" width="100px" src={logo} alt="Logo of the app"></a> 
        {/if}
    </div>
    <div>
        <h1 class="title">UPROJECT</h1>   
    </div>
    {#if getUserId() === true}
        <Dropdown userId={getUserId()}></Dropdown>
    {:else}
        <a class="loginButton" href="/auth/login">Login</a>

    {/if}

</header>

<style>
    .navbar{
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        place-items: center;
        background-color: #ff682c;
        width: 100%;
    }
    .logoContainer{
        display: flex;
        justify-content: flex-start;
    }
    .headerLogo{
        width: 100px;
    }
    .title {
        font-size: 3em;
    }
    .loginButton{
        background-color: white;
        color: #ff682c;
        border: none;
        border-radius: 15px;
        padding: 1em;
        text-decoration: none;
    }
    @media screen and (max-width: 500px){
        .title {
            display: none;
        }
        .navbar{
            width: 100%;
        }
    }
</style>