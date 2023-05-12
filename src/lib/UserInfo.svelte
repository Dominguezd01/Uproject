<script>
    import user from "$lib/assets/user.svg";
    import Swal from "sweetalert2";
    export let userInfoData;
    console.log(userInfoData);
    let oldPass, userName, userEmail, newPass, userNameInput, userEmailInput;
    userName = userInfoData.name
    userEmail = userInfoData.email

    const handleSubmit = async() =>{
        if(oldPass.trim() == "" || newPass.trim() == ""  ){
            Swal.fire({
                title: "Error",
                text: "You must type your old password to update your profile",
                icon: "error"
            })
        }
    }
</script>

<div class="mainContainer">

    <div class="imageContainer">
        <img class="profilePicture" src={user} alt="userProfilePhoto" />
    </div>
    <form class="editProfileForm" on:submit|preventDefault={handleSubmit}>
        <div class="inputsContainer">
            <div class="inputContainer">
                <label for="userName">User name</label>
                <input
                    id="userName"
                    type="text"
                    bind:value={userName}
                    bind:this={userNameInput}
                    required
                    class="input"
                />
            </div>
            <div class="inputContainer">
                <label for="email">Email</label>
                <input
                    type="email"
                    bind:value={userEmail}
                    bind:this={userEmailInput}
                    required
                    class="input"
                />
            </div>
    
            <div class="inputContainer">
                <label for="password">Old password</label>
                <input id="password" required type="password" bind:value={oldPass} class="input" />
            </div>
            <div class="inputContainer">
                <label for="newPass">New password</label>
                <input id="newPass" type="password" bind:value={newPass} class="input" />
            </div>
        </div>

        <div>
            <input class="submit" type="submit">
        </div>
    </form>
</div>

<style>
    .profilePicture {
        width: 200px;
    }
    .mainContainer {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        gap: 2em;
        margin-top: 100px;
    }
    .inputContainer {
        display: grid;
        place-items: center;
    }
    .inputsContainer {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        grid-template-rows: repeat(2, 1fr);
        gap: 1em;
        margin-top: 2em;
    }
    .editProfileForm{
        display: grid;
        place-items: center;
        gap: 2em;
    }
    .input{
        width: 280px;
        height: 45px;
        border-radius: 5px;
        border: none;
        text-align: center;
    }

    .submit{
        background-color: #ff682c;
        padding: 1em;
        width: 150px;
        border: none;
        border-radius:  5px;
    }

</style>
