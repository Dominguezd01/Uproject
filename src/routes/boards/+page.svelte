<script>
  // @ts-nocheck

  import { onMount } from "svelte"
  import BoardDisplay from "../../lib/BoardDisplay.svelte"
  import Swal from "sweetalert2"
  import { API_ROUTE } from "../../lib/routes"
  import Loader from "../../lib/Loader.svelte"
  import UpLogo from "../../lib/UpLogo.svelte"
  import { browser } from "$app/environment"
  let searchParams
  let inputValue
  let idUser

  const getUserId = () => {
    if (browser) {
      let userId = sessionStorage.getItem("userId")
      if (!userId || Object.keys(userId).length == 0) {
        location.href = "/auth/login"
      }
      return userId
    }
  }

  onMount(async () => {
    if (browser) {
      let idUser = sessionStorage.getItem("userId")

      if (!idUser) {
        location.href = "/auth/login"
      }
    }
  })

  async function createBoard() {
    Swal.fire({
      title: "Type your board name",
      input: "text",
      inputAttributes: {
        autocapitalize: "off",
      },
      showCancelButton: true,
      confirmButtonText: "Add board!",
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
            userId: getUserId(),
            board_name: name,
          }),
        }
        try {
          let responseCreateBoard = await fetch(
            `${API_ROUTE}/boards/create`,
            options
          )
          if (!responseCreateBoard.ok) {
            throw new Error(responseCreateBoard.statusText)
          }
          responseCreateBoard = await responseCreateBoard.json()
          if (responseCreateBoard.status == 200) {
            Swal.fire({
              title: "Board created successfully",
              text: "Enjoy leveling up your project :D",
              icon: "success",
              showConfirmButton: false,
            })
            if (browser) {
              localStorage.setItem("boardId", responseCreateBoard.board.id)
            }
            setTimeout(() => {
              location.href = `/boards/view/${responseCreateBoard.board.id}`
            }, 1000)
          }
        } catch (error) {
          Swal.showValidationMessage(`Request failed: ${error}`)
        }
      },
      allowOutsideClick: () => !Swal.isLoading(),
    })
  }

  const getUserBoards = async () => {
    let res
    let searchParams = getUserId()

    res = await fetch(`${API_ROUTE}/boards/${String(searchParams)}`)

    res = await res.json()

    return res.boards
  }
  let getBoards = getUserBoards()
</script>

<div id="main">
  {#await getBoards}
    <Loader />
  {:then boards}
    <h1 id="boardTitle">BOARDS</h1>
    <button class="addBoardButton" on:click={createBoard}>Add a board!</button>
    <div id="boardsContainer">
      {#each boards as board}
        <BoardDisplay {board} />
      {/each}
    </div>
  {/await}
</div>

<style>
  #boardTitle {
    font-size: 5em;
  }
  .addBoardButton {
    border: none;
    border-radius: 15px;
    padding: 1em;
    background-color: #ff682c;
    color: white;
    border: 1px rgba(250, 128, 114, 0.336) solid;
    cursor: pointer;
    transition: 0.3s;
  }

  #main {
    display: grid;
    place-items: center;
  }

  #boardsContainer {
    display: grid;
    place-items: center;
    grid-template-columns: repeat(2, 1fr);
    gap: 1em;
    margin-top: 1em;
  }

  @media screen and (max-width: 700px) {
    #boardTitle {
      font-size: 3em;
    }
    #boardsContainer {
      display: grid;
      place-items: center;
      grid-template-columns: 1fr;
      gap: 1em;
      margin-top: 1em;
    }
  }
</style>
