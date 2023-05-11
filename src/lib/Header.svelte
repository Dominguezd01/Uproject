<script>
    import UpLogo from "./UpLogo.svelte";
    import logo from '$lib/assets/UpLogoWhite.svg';
    import Dropdown from "./Dropdown.svelte";
    import { browser } from "$app/environment";
    const getUserId = () => {
        if (browser) {
            let userId = sessionStorage.getItem("userId");
            if (!userId || Object.keys(userId).length == 0) {
               return false
            }
            return userId;
        }
    };
</script>
<header class="navbar navbar-expand-lg navbar-light bg-light w-120">
    <div class="logoContainer">
        <img class="headerLogo" src={logo} alt="">
    </div>
    <div>
        <h1>UPROJECT</h1>   
    </div>
    {#if getUserId}
    <Dropdown userId={getUserId()}></Dropdown>
    {:else}
        <a href="/auth/login">Login</a>

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
        margin-left: -10em;
        width: 100px;
    }
</style>