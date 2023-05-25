<script>
    import { onMount } from "svelte";
    import Swal from "sweetalert2";
    import { API_ROUTE } from "../../../lib/routes";
    import Loader from "../../../lib/Loader.svelte"
    import { browser } from '$app/environment';
    import UserInfo from "../../../lib/UserInfo.svelte"
    const getUserId = () =>{
        if(browser){
            let userId = sessionStorage.getItem("userId")
            if(!userId){
                location.href = "/auth/login"
            }
            return userId
        }
    }
    onMount(() =>{
        getUserId()
    })
    const getUserInfo = async () =>{
        let options = {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                userId: getUserId()
            })
        }
        let getUserInfoResponse = await fetch(`${API_ROUTE}/users/getUserInfo`, options)
        getUserInfoResponse = await getUserInfoResponse.json()
        return getUserInfoResponse
    }
    let userInfo = getUserInfo()
</script>


<main>
    {#await userInfo}
        <Loader></Loader>
    {:then userInfoData } 
        <UserInfo userInfoData={userInfoData.sendData}></UserInfo>
    {/await}
</main>
<style>
    main{
        display: grid;
        place-items:center;
    }

 
</style>