<script>
    import user from "$lib/assets/user.svg";
    import Swal from "sweetalert2";
    import { API_ROUTE } from "./routes";
    import { onMount } from "svelte";
    import { browser } from "$app/environment";
    export let userInfoData;
    
    let oldPass,
        userName,
        userEmail,
        newPass,
        userNameInput,
        userEmailInput,
        labelNewPass,
        newPassInput;

    onMount(() => {
        userName = userInfoData.name;
        userEmail = userInfoData.email;
    });

    const getUserId = () => {
        if (browser) {
            let userId = sessionStorage.getItem("userId");
            if (!userId || Object.keys(userId).length == 0) {
                location.href = "/auth/login";
            }
            return userId;
        }
    };

    const handleSubmit = async () => {
        let newPassForm;
        if (oldPass.trim() == "") {
            Swal.fire({
                title: "Error",
                text: "You must type your old password to update your profile",
                icon: "error",
            });
        }
        if (!newPass || newPass.trim() == "") {
            newPassForm = null;
        } else {
            newPassForm = newPass;
        }
        let options = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({
                userId: getUserId(),
                oldPass: oldPass,
                newPass: newPassForm,
                email: userEmail,
                userName: userName,
            }),
        };
        let updateProfileResponse = await fetch(
            `${API_ROUTE}/users/edit`,
            options
        );
        updateProfileResponse = await updateProfileResponse.json();

        if (updateProfileResponse.status == 200) {
            Swal.fire({
                title: "Success",
                text: `${updateProfileResponse.message}`,
                icon: "success",
                timer: 1200,
                showConfirmButton: false,
            });

            if (browser) {
                sessionStorage.setItem(
                    "userName",
                    updateProfileResponse.sendData.name
                );

                setTimeout(() => {
                    location.href = "/boards";
                }, 1200);
            }
        }else if(updateProfileResponse.status == 401){
            Swal.fire({
                title: "Error",
                title: `${updateProfileResponse.message}`,
                icon: "error"
            })
        }else if(updateProfileResponse.status == 500){
            Swal.fire({
                title: "Error",
                title: `${updateProfileResponse.message}`,
                icon: "error",
                timer: 2000
            })
            setTimeout(() =>{
                location.href = "/boards"
            }, 2000)
        }else{
            Swal.fire({
                title: "Error",
                title: `${updateProfileResponse.message}`,
                icon: "error",
                timer: 1500
            })
        }
    };
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
                <input
                    id="password"
                    required
                    type="password"
                    bind:value={oldPass}
                    class="input"
                />
            </div>
            <div class="inputContainer">
                <label for="newPass" bind:this={labelNewPass}
                    >New password</label
                >
                <input
                    id="newPass"
                    type="password"
                    bind:value={newPass}
                    bind:this={newPassInput}
                    class="input"
                />
            </div>
        </div>

        <div>
            <input class="submit" type="submit" />
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
    .editProfileForm {
        display: grid;
        place-items: center;
        gap: 2em;
    }
    .input {
        width: 280px;
        height: 45px;
        border-radius: 5px;
        border: none;
        text-align: center;
        background-color: #2b303a;
        border: solid 1px rgb(204, 204, 204);
    }

    .submit {
        background-color: #ff682c;
        padding: 1em;
        width: 150px;
        border: none;
        border-radius: 5px;
    }
</style>
