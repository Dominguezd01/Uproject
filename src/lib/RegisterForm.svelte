<script>
    import { API_ROUTE } from "./routes.js";

    let emailForm
    let passwordForm
    let userNameForm
    const handleSubmit = async () =>{
        let response = await fetch(`${API_ROUTE}/users/register`, {
            method: "POST", 
            headers: {
                "Content-Type": "application/json"
            },
            body:JSON.stringify({
                name: userNameForm,
                email: emailForm,
                password: passwordForm
            })
        })

        response =  await response.json()
    }   


</script>

<div id="formContainer">
    <form id="formLogin" on:submit|preventDefault={handleSubmit}>
        <span class="inputContainer" id="userNameContainer">
            <label for="userName">Username</label>
            <input class="inputForm" type="text" bind:value={userNameForm}>
        </span>
        <span class="inputContainer" id="emailContainer">
            <label for="email">Email</label>
            <input class="inputForm" type="text" bind:value={emailForm}>
        </span>
        <span class="inputContainer" id="passwordContainer">
            <label for="password">Password</label>
            <input  class="inputForm" type="password" bind:value={passwordForm}>
        </span>
        <input type="submit">
    
    </form>
</div>


<style>
    #formContainer{
        margin-top: 2em;
        display: grid;
        place-items: center;
    }
    .inputContainer{
        display: grid;
        place-items: center;
        gap: 0.2em;
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
    .inputForm:focus{
        -webkit-box-shadow: 10px 10px 5px 0px rgba(235, 138, 100, 0.473);
        -moz-box-shadow: 10px 10px 5px 0px rgba(235, 138, 100, 0.473);
        box-shadow: 10px 10px 5px 0px rgba(235, 138, 100, 0.473);
    }


    #formLogin{
        display: grid;
        place-items: center;
        gap: 3em
    }

</style>