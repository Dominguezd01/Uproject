<script>
    import UpLogo from "./UpLogo.svelte";
    import logo from '$lib/assets/UpLogoWhite.svg';
</script>
<header class="navbar navbar-expand-lg navbar-light bg-light w-120">
    <div class="logoContainer">
        <img class="headerLogo" src={logo} alt="">
    </div>
    <div>
        <h1>UPROJECT</h1>   
    </div>
    <div></div>
</header>

<style>
    .navbar{
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        place-items: center;
        background-color: #ff682c;
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