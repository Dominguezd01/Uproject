<script>
  import { onMount } from "svelte"
  import Swal from "sweetalert2"
  import { API_ROUTE } from "../../../lib/routes.js"
  import { browser } from "$app/environment"
  let emailForm
  let passwordForm
  let userNameForm
  onMount(() => {
    if (browser) {
      if (sessionStorage.getItem("userId")) {
        location.href = "/boards"
      }
    }
  })
  const handleSubmit = async () => {
    let response = await fetch(`${API_ROUTE}/users/register`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        name: userNameForm,
        email: emailForm,
        password: passwordForm,
      }),
    })

    response = await response.json()

    if (response.status == 200) {
      Swal.fire({
        title: "Success!",
        text: `${response.message}`,
        icon: "success",
      })
      if (browser) {
        sessionStorage.setItem("userId", response.sendData.id)
        sessionStorage.setItem("userName", response.sendData.name)
      }
      setTimeout(() => {
        location.href = "/boards"
      }, 1000)
    } else {
      Swal.fire(`${response.message}`, "error")
    }
  }
</script>

<div id="formContainer">
  <form id="formLogin" on:submit|preventDefault={handleSubmit}>
    <span class="inputContainer" id="userNameContainer">
      <label for="userName">Username</label>
      <input
        class="inputForm"
        type="text"
        bind:value={userNameForm}
        id="userName"
      />
    </span>
    <span class="inputContainer" id="emailContainer">
      <label for="email">Email</label>
      <input class="inputForm" type="email" bind:value={emailForm} id="email" />
    </span>
    <span class="inputContainer" id="passwordContainer">
      <label for="password">Password</label>
      <input
        class="inputForm"
        type="password"
        bind:value={passwordForm}
        id="password"
      />
    </span>
    <input class="submit" type="submit" />
  </form>
  <a class="loginRedirect" href="/auth/login">Are you a memeber?Log in!</a>
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
    gap: 0.2em;
  }
  .inputForm {
    width: 300px;
    height: 50px;
    border: solid 1px rgb(204, 204, 204);
    border-radius: 15px;
    outline: none;
    text-align: center;
    transition: 0.2s;
    background-color: #2b303a;
  }
  .inputForm:focus,
  .inputForm:hover {
    -webkit-box-shadow: 10px 10px 5px 0px #f07f53;
    -moz-box-shadow: 10px 10px 5px 0px #f07f53;
    box-shadow: 10px 10px 5px 0px #f07f53;
  }
  .submit {
    width: 150px;
    background-color: #ff682c;
    text-align: center;
    padding: 1em;
    border: none;
    border-radius: 15px;
    transition: 0.3s;
    cursor: pointer;
  }
  .submit:hover {
    -webkit-box-shadow: 10px 10px 60px 0px rgba(231, 215, 215, 1);
    -moz-box-shadow: 10px 10px 60px 0px rgba(231, 215, 215, 1);
    box-shadow: 10px 10px 60px 0px rgba(231, 215, 215, 1);
  }
  .loginRedirect {
    margin-top: 1em;
    color: rgb(255, 120, 67);
  }

  #formLogin {
    display: grid;
    place-items: center;
    gap: 3em;
  }
</style>
