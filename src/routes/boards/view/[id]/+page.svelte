<script>
  // @ts-nocheck

  import { onMount } from "svelte"
  import Column from "../../../../lib/Column.svelte"
  import deleteUserIcon from "$lib/assets/delete-user.svg"
  import addUserIcon from "$lib/assets/addUser.svg"
  import createColumnIcon from "$lib/assets/createColumn.svg"
  import Loader from "../../../../lib/Loader.svelte"
  //import UpLogo from "../../../../UpLogo.svelte"
  import Swal from "sweetalert2"
  import { API_ROUTE } from "../../../../lib/routes.js"
  import { browser } from "$app/environment"

  let states
  let columnContainer
  let boardId
  let userId
  let boardNameValue
  onMount(() => {
    if (browser) {
      localStorage.setItem(
        "boardId",
        location.href.split("/")[location.href.split("/").length - 1]
      )
    }
  })
  const setBoardId = () => {
    if (browser) {
      localStorage.setItem(
        "boardId",
        location.href.split("/")[location.href.split("/").length - 1]
      )
    }
  }
  const getUserId = () => {
    if (browser) {
      let userId = sessionStorage.getItem("userId")
      if (!userId || Object.keys(userId).length == 0) {
        location.href = "/auth/login"
      }
      return userId
    }
  }
  const getBoardId = () => {
    if (browser) {
      boardId = localStorage.getItem("boardId")
      if (!boardId) {
        location.href = "/boards"
      }
      return parseInt(boardId)
    }
  }

  const getBoard = async () => {
    setBoardId()
    let options = {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        boardId: getBoardId(),
        userId: getUserId(),
      }),
    }
    let response = await fetch(`${API_ROUTE}/boards/view`, options)

    response = await response.json()
    if (response.status == 404) {
      location.href = "/boards"
    }
    boardId = response.board[0].id
    states = response.board[1]

    return response
  }

  const getBoardInfo = async () => {
    const board = await getBoard()
    return board
  }
  let response = getBoardInfo()

  async function createColumn() {
    Swal.fire({
      title: "Column name",
      input: "text",
      inputAttributes: {
        autocapitalize: "off",
      },
      showCancelButton: true,
      confirmButtonText: "Create",
      showLoaderOnConfirm: true,
      preConfirm: async (name) => {
        if (!name) {
          name = " "
        }
        let options = {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            boardId: getBoardId(),
            columnTitle: name,
            userId: getUserId(),
          }),
        }

        let responseCreateColumn = await fetch(
          `${API_ROUTE}/columns/add`,
          options
        )

        responseCreateColumn = await responseCreateColumn.json()
        if (responseCreateColumn.status == 200) {
          new Column({
            target: columnContainer,
            props: {
              // @ts-ignore
              columnInfo: {
                // @ts-ignore
                id: responseCreateColumn.column.id,
                title: responseCreateColumn.column.title,
                // @ts-ignore
                tasks: [],
              },

              states: JSON.parse(localStorage.getItem("states")),
            },
          })
        } else if (response.status == 401) {
          Swal.fire({
            title: "Error",
            text: "You are not part of this board",
            icon: "error",
            showConfirmButton: false,
            timer: 1500,
          })
          setTimeout(() => {
            location.href = "/boards"
          }, 1500)
        } else {
          Swal.fire({
            title: "Error",
            text: `${responseCreateColumn.message}`,
            icon: "error",
            showConfirmButton: false,
            timer: 1500,
          })
          setTimeout(() => {
            location.reload()
          }, 1500)
        }
      },
      allowOutsideClick: () => !Swal.isLoading(),
    })
  }

  const addUser = async () => {
    Swal.fire({
      title: "Type the user email",
      input: "email",
      text: "This user will see this board",
      inputAttributes: {
        autocapitalize: "off",
      },
      showCancelButton: true,
      confirmButtonText: "Send",
      showLoaderOnConfirm: true,
      preConfirm: async (email) => {
        let options = {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            board_id: boardId,
            user_id: getUserId(),
            add_user_email: email,
          }),
        }

        let responseAddUser = await fetch(
          `${API_ROUTE}/boards/addUser`,
          options
        )

        responseAddUser = await responseAddUser.json()
        if (responseAddUser.status == 200) {
          Swal.fire({
            title: "User added",
            // @ts-ignore
            text: responseAddUser.message,
            icon: "success",
            showConfirmButton: true,
          })
        } else {
          Swal.fire({
            title: `Cannot add user`,
            // @ts-ignore
            text: responseAddUser.message,
            icon: "error",
            showConfirmButton: true,
          })
        }
      },
      allowOutsideClick: () => !Swal.isLoading(),
    })
  }

  const deleteUser = async () => {
    let userId = await getUserId()
    let boardId = await getBoardId()
    const options = {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        id: boardId,
        userId: userId,
      }),
    }
    let response = await fetch(`${API_ROUTE}/boards/getUsersBoard`, options)

    response = await response.json()
    if (response.status == 200) {
      let optionSwal = []
      response.users.forEach((user) => {
        optionSwal.push({ user })
      })
      Swal.fire({
        title: "User in this board",
        input: "select",
        inputOptions: response.users,

        inputAttributes: {
          autocapitalize: "off",
        },
        showCancelButton: true,
        confirmButtonText: "Delete this user",
        showLoaderOnConfirm: true,
        preConfirm: async (user_delete) => {
          const options = {
            method: "DELETE",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              board_id: await getBoardId(),
              user_id: await getUserId(),
              user_id_delete: user_delete,
            }),
          }
          let response = await fetch(`${API_ROUTE}/boards/deleteUser`, options)

          response = await response.json()

          if (response.status == 200) {
            Swal.fire({
              text: `${response.message}`,
              icon: "success",
              timer: 2000,
              timerProgressBar: true,
            })
          } else {
            Swal.fire({
              text: `${response.message}`,
              icon: "error",
              timer: 2000,
              timerProgressBar: true,
            })
          }
        },
        allowOutsideClick: () => !Swal.isLoading(),
      })
      if (browser) {
        let optgroups = document.querySelectorAll("optgroup")
        let select = document.querySelector(".swal2-select")
        optgroups.forEach((opt) => {
          let options = opt.querySelectorAll("option")
          options.forEach((option) => {
            select.appendChild(option)
          })
          opt.remove()
        })
      }
    }
  }

  const handleEditBoardName = async () => {
    let options = {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        userId: getUserId(),
        boardId: getBoardId(),
        name: boardNameValue.value,
      }),
    }
    let updateBoardName = await fetch(`${API_ROUTE}/boards/update`, options)

    updateBoardName = await updateBoardName.json()

    if (updateBoardName.status == 401) {
      Swal.fire({
        title: "Error",
        text: `${updateBoardName.message}`,
        icon: "error",
        timer: 2000,
        showConfirmButton: false,
      })

      setTimeout(() => {
        location.href = "/boards"
      }, 2000)
    } else if (updateBoardName.status == 400) {
      Swal.fire({
        title: "Error",
        text: `${updateBoardName.message}`,
        icon: "error",
      })
    } else if (updateBoardName.status == 500) {
      Swal.fire({
        title: "Error",
        text: `${updateBoardName.message}`,
        icon: "error",
      })
    }
  }
</script>

<div id="main">
  {#await response}
    <Loader />
  {:then boardInfo}
    <input
      class="boardName"
      value={boardInfo["board"][0].name}
      on:blur={handleEditBoardName}
      bind:this={boardNameValue}
    />
    <div id="flexRight">
      <button class="createColumnButton" on:click={createColumn}>
        <span class="text">Create a new column!</span>
        <img
          src={createColumnIcon}
          class="icon"
          alt="Create column icon"
          width="20px"
        />
      </button>
      <button class="addUserButton" on:click={addUser}>
        <span class="text">Add a user!</span>
        <img src={addUserIcon} class="icon" alt="Add user Icon" />
      </button>
      <button class="deleteUserButton" on:click={deleteUser}>
        <span class="text">Delete a user</span>
        <img src={deleteUserIcon} class="icon" alt="Delete user icon" />
      </button>
    </div>
    <div class="columnContainer" bind:this={columnContainer}>
      {#each boardInfo["board"][1].columns as column}
        <Column states={boardInfo["board"][2]} columnInfo={column} />
      {/each}
    </div>
  {/await}
</div>

<style>
  #main {
    display: grid;
    place-items: center;
    text-align: center;
  }
  #flexRight {
    display: flex;
    width: 900px;
    margin-bottom: 1.5em;
    justify-content: space-around;
  }
  .boardName {
    text-transform: capitalize;
    font-size: 35px;
  }
  .columnContainer {
    display: grid;
    place-items: center;
    gap: 1em;
  }
  .deleteUserButton {
    border: none;
    border-radius: 15px;
    padding: 1em;
    background-color: #ff2c2c;
    color: white;
    transition: 0.3s;
    font-size: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.3em;
    cursor: pointer;
  }

  .createColumnButton,
  .addUserButton {
    border: none;
    border-radius: 15px;
    padding: 1em;
    background-color: #ff682c;
    color: white;
    border: 1px rgba(250, 128, 114, 0.336) solid;
    transition: 0.3s;
    font-size: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.3em;
    cursor: pointer;
  }

  .addUserButton {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.2em;
  }

  .createColumnButton:hover,
  .addUserButton:hover,
  .deleteUserButton:hover {
    border: 1px solid rgb(247, 237, 236);
  }
  .icon {
    width: 20px;
  }
  .boardName {
    font-size: 35px;
    margin-top: 1em;
    margin-bottom: 1em;
    width: 450px;
    background-color: #2b303a;
    transition: 0.4s;
    border: none;
    height: 55px;
    text-align: center;
    border-radius: 15px;
  }

  .boardName:hover,
  .boardName:focus {
    border: 0.4px #ff682c solid;
    background-color: rgb(253, 140, 75);
  }

  @media screen and (max-width: 500px) {
    #main {
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      flex-direction: column;
    }

    #flexRight {
      display: flex;
      width: 400px;
      margin-bottom: 1.5em;
      justify-content: space-around;
    }
    .text {
      display: none;
    }
    .boardName {
      font-size: 35px;
      margin-top: 1em;
      margin-bottom: 1em;
      width: 300px;
      background-color: #2b303a;
      transition: 0.4s;
      border: none;
      height: 55px;
      text-align: center;
      border-radius: 15px;
      overflow: hidden;
    }
  }
</style>
