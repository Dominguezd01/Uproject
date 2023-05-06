<script>
    import Swal from "sweetalert2";
 
    import *  as API_ROUTE from "./routes.js" ;
    import { onMount } from "svelte";
    import { browser } from '$app/environment';
    import deleteIcon from '$lib/assets/delete.svg';
    
    export let board
    let idUser
    onMount(() =>{
 
    })

    const getUserId = () =>{
        if (browser) {
            idUser = sessionStorage.getItem("userId")
        }
    }
    //console.log(board)

    //let userId = sessionStorage.getItem("userId")
    let divClickable
    const handleClick = (e) =>{
        let id = divClickable.dataset.id
        if(e.target.tagName == "IMG"){
            deleteBoard()
            
        }else{
            localStorage.setItem("boardId", id)
            location.href= `/boards/view/${id}`
        }
    }

    const deleteBoard = async () => {
        Swal.fire({
            icon: "warning",
            title: "Are you sure you wanna delete this board?",
            text: "This would delete all the columns and tasks inside and disapear forever",
            inputAttributes: {
                autocapitalize: "off",
            },
            showCancelButton: true,
            confirmButtonText: "Yes, delete it",
            showLoaderOnConfirm: true,
            preConfirm: async () => {
                let options = {
                    method: "DELETE",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                        boardId: divClickable.dataset.id,
                        userId: getUserId()
                    }),
                };
                try {
                    let responseDeleteColumn = await fetch(
                        `${API_ROUTE}/boards/delete`,
                        options
                    );
                    if (!responseDeleteColumn.ok) {
                        throw new Error(responseDeleteColumn.statusText);
                    }
                    responseDeleteColumn = await responseDeleteColumn.json();
                    console.log(responseDeleteColumn);
                    if (responseDeleteColumn.status == 200) {
                        Swal.fire({
                            title: "Board deleted",
                            // @ts-ignore
                            text: responseDeleteColumn.message, 
                            icon: "success",
                            showConfirmButton: true,
                        });

                        divClickable.remove();
                    }else{
                        Swal.fire({
                            title: "Cannot delete the board",
                            // @ts-ignore
                            text: responseDeleteColumn.message, 
                            icon: "error",
                            showConfirmButton: true,
                        });
                    }
                } catch (error) {
                    Swal.fire({
                            title: "Cannot delete the board",
                            // @ts-ignore
                            text: error, 
                            icon: "error",
                            showConfirmButton: true,
                        });
                }
            },
            allowOutsideClick: () => !Swal.isLoading(),
        });
    }
</script>


<!-- svelte-ignore a11y-click-events-have-key-events -->

    <div on:click|stopPropagation={handleClick} bind:this={divClickable} class="board" data-id ={board.id}>
        <div id="boardName">
            <span class="dynamicText"><b>{board.name}</b></span>
        </div>
        <!--<div id="separator"></div>-->
        <div id="boardOwner">
            <div id="ownerName">
                <span class="permanentText">Owner: </span>
                {#if board.owner != "" }
                    <span class="dynamicText"> {board.owner}</span>
                {:else}
                    <span class="dynamicText">You</span>
                {/if}
            </div>
            <!-- svelte-ignore a11y-missing-attribute -->
            <a class="deleteBoard" on:click|preventDefault={deleteBoard}>
                <img class="deleteIcon" src={deleteIcon} alt="">
            </a>
        </div>
    </div>

  
<style>


    .board{
        border: 1px solid orange;
        border-radius: 15px;
        display: grid;
        place-items: center;
        width:250px;
        padding: 1em;
        height: 70px;
        gap: 1em;
        overflow: hidden;
        cursor: pointer;
    }

    .deleteBoard{
        border: none;

    }
    .deleteIcon{
        width: 20px;
        cursor: pointer;
    }
    #boardName, #boardOwner{
        display: flex;
        gap: 1em;
        align-items: center;
        border: 0px 0px 0px 1px solid orange;
    }
    #boardOwner{
        cursor: pointer;
    }
    .permanentText{
        font-weight: bold;
    }
    .dynamicText{
        margin-left: 1em;
       
    }
</style>