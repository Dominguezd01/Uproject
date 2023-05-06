<script>
    import UpLogo from "./UpLogo.svelte";
    import logo from '$lib/assets/UpLogoWhite.svg';
</script>
<header class="navbar navbar-expand-lg navbar-light bg-light w-120">
    
    <img class="headerLogo" src={logo} alt="">
</header>

<style>
    .navbar{

        background-color: #ff682c;
    }
    .headerLogo{
        width: 100px;
    }
</style>