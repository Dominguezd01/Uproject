<script>
    import { onMount } from 'svelte';
    import { API_ROUTE } from '../../../lib/routes.js';
    import { browser } from '$app/environment';
    import Swal from 'sweetalert2';
    //import userId from "$app/stores"

    onMount(() => {
        if(browser){
            if(sessionStorage.getItem("userId")){
                location.href = "/boards"
            }
        }
    })

 
    let emailForm;
    let passwordForm;
    const handleSubmit = async () => {
        let response = await fetch(`${API_ROUTE}/users/login`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({
                email: emailForm,
                password: passwordForm,
            }),
        });

        response = await response.json();

        if (response.status == 200) {
            if(browser){
                sessionStorage.setItem("userId", response.sendData.id)
                sessionStorage.setItem("userName",response.sendData.name )
            }
            Swal.fire({
                title: "Welcome again",
                text: `${response.message}`,
                icon: "success",
                timer: 1500,
            })
            setTimeout(() =>{
                location.href = `/boards`
            },1500)
           
        }else if(response.status == 401){
            Swal.fire({
                title: "Error",
                text: `${response.message}`,
                icon: "error",
                timer: 2000
            })
        }
    };
</script>

<div id="formContainer">
    <form class="formLogin" on:submit|preventDefault={handleSubmit}>
        <div class="inputContainer">
            <span class="inputContainerSpan" id="userNameContainer">
                <label for="email">Email</label>
                <input class="inputForm" type="text" bind:value={emailForm} />
            </span>
            <span class="inputContainerSpan" id="passwordContainer">
                <label for="password">Password</label>
                <input
                    class="inputForm"
                    type="password"
                    bind:value={passwordForm}
                />
            </span>
        </div>
        <input class="submit" type="submit" />
    </form>

    <a class="loginRedirect" href="/auth/register">Are you a new? Sing Up!</a>
</div>

<style>
    #formContainer {
        margin-top: 2em;
        display: grid;
        place-items: center;
    }
    .inputContainer {
        display: grid;
        place-items: center;

        gap: 1em;
    }
    .inputContainerSpan{
        display: grid;
        place-items: center;
    }
    .inputForm{
        width: 300px;
        height: 50px;
        border: solid 1px #00000048;
        border-radius: 15px;
        outline: none;
        text-align: center;
        transition: 0.2S;
    }
    .submit{
        width: 150px;
        background-color: #ff682c ;
        text-align: center;
        padding: 1em;
        border: none;
        border-radius: 15px;
        transition: 0.3s;
    }
    .inputForm:focus, .submit:hover{
        -webkit-box-shadow: 10px 10px 5px 0px rgba(235, 138, 100, 0.986);
        -moz-box-shadow: 10px 10px 5px 0px rgb(235, 138, 100);
        box-shadow: 10px 10px 5px 0px rgb(235, 138, 100);
    }
    .formLogin {
        display: grid;
        place-items: center;
        gap: 3em;
    }
    .loginRedirect{
        margin-top: 1em;
        color: rgb(255, 120, 67);
    }
</style>
