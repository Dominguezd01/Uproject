<script>
    import Swal from "sweetalert2";
 
    import  {API_ROUTE} from "./routes.js" ;
    import { onMount } from "svelte";
    import { browser } from '$app/environment';
    import deleteIcon from '$lib/assets/delete.svg';
    
    export let board
    let idUser
    onMount(() =>{
 
    })

    const getUserId = () =>{
        if (browser) {
            let idUser = sessionStorage.getItem("userId")
            if(!idUser){
                location.href = "/auth/login"
            }
            return idUser
        }

    }
    let divClickable
    const handleClick = (e) =>{
        let id = divClickable.dataset.id
        if(e.target.tagName == "IMG"){
            deleteBoard()
            
        }else{
            if(browser){
                localStorage.setItem("boardId", id)
            }
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
       
                    let responseDeleteColumn = await fetch(
                        `${API_ROUTE}/boards/delete`,
                        options
                    );

                    responseDeleteColumn = await responseDeleteColumn.json();
                    if (responseDeleteColumn.status == 200) {
                        Swal.fire({
                            title: "Board deleted",
                            // @ts-ignore
                            text: responseDeleteColumn.message, 
                            icon: "success",
                            showConfirmButton: true,
                        });

                        divClickable.remove();
                    }else if(responseDeleteColumn.status == 401){
                        Swal.fire({
                            title: "Cannot delete the board",
                            // @ts-ignore
                            text: responseDeleteColumn.message, 
                            icon: "error",
                            showConfirmButton: true,
                        });
                    }else{
                        Swal.fire({
                            title: "Cannot delete the board",
                            // @ts-ignore
                            text: responseDeleteColumn.message, 
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
        transition: 0.2s;
    }
    .board:hover{
        -webkit-box-shadow: 10px 10px 5px 0px rgba(235, 138, 100, 0.986);
        -moz-box-shadow: 10px 10px 5px 0px rgb(235, 138, 100);
        box-shadow: 10px 10px 5px 0px rgb(235, 138, 100);
    }
    .deleteBoard{
        border: none;
        width: 20px;
        cursor: pointer;
    }
    .deleteIcon{
        width: 20px;
    }
    #boardName, #boardOwner{
        display: flex;
        gap: 1em;
        align-items: center;
        justify-content: center;
        border: 0px 0px 0px 1px solid orange;
    }
    #boardOwner{
        cursor: pointer;
        color: white;
        grid-template-columns: repeat(2, 1fr);
    }
    .permanentText{
        font-weight: bold;
    }
    
</style>