<script>
    import { onMount } from "svelte";
    import SlimSelect from "slim-select";
    import DeleteButton from "./DeleteButton.svelte";
    import EditButton from "./EditButton.svelte";
    import { API_ROUTE } from "./routes";
    import { browser } from '$app/environment'; 
    import Swal from "sweetalert2";
    export let taskContent
    export let states

    let content = taskContent[0].content
    let state = taskContent.states
    let inputTask
    let select
    let taskDiv
    const getUserId = () => {
        if (browser) {
            let userId = sessionStorage.getItem("userId");
            if (!userId || Object.keys(userId).length == 0 ) {
                location.href = "/auth/login";
            }
            return userId;
        }
    }
    const getBoardId = () => {
        if (browser) {
            let boardId = localStorage.getItem("boardId");
            if (!boardId) {
                location.href = "/boards";
            }
            return parseInt(boardId);
        }
    }
    const handleClick = () =>{
        localStorage.setItem("taskContentIfFailed", inputTask.value)
        inputTask.removeAttribute("readonly")  
    }
    const handleClickSelect =  () =>{
        localStorage.setItem("selectStateIfFailed", select.value)
    }
    const handleBlur = async (e) =>{
        if(inputTask.value == ""){
            inputTask.value=" "
        }
        const options = {
            method: "PUT",
            headers: {
                "Content-Type": "Application/json"
            },
            body: JSON.stringify({
                taskId: taskContent[0].id,
                // @ts-ignore
                content: inputTask.value,
                // @ts-ignore
                state: state, 
                userId: getUserId(),
                boardId: getBoardId()
            })   
        }
        try{
            let response = await fetch(`${API_ROUTE}/tasks/edit`, options)
            response = await response.json()
            if(response.status == 401){
                Swal.fire({
                    title: "Error",
                    text:`${response.message}`,
                    icon: "error",
                    timer: 1500,
                    showConfirmButton: false,
                    timer: 1500
                })

                setTimeout(() => {
                    location.href = "/boards"
                }, 1500)
            }
            if(response.status != 200){
                Swal.fire(
                    "Error",
                    `${response.message}`,
                    "error"
                )
            }
        }catch(err){
       
        }

    }

</script>

<div bind:this={taskDiv} class="task" id={taskContent.id} data-id={taskContent[0].id}>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div  >
        <input data-id={taskContent.id}  
        bind:this={inputTask}
        bind:value={content} 
        on:blur={handleBlur} 
        on:click={handleClick}
        readonly type="text"
        id={"taskContent"+taskContent.id}
        class="taskContent">
    </div>
   

    <div id="selectContainer">
        <select 
            on:click={handleClickSelect}
            on:blur={handleBlur} 
            bind:this={select} 
            bind:value={state} 
            name="state" id={"selectState"+taskContent.id}>
            {#each states as state}
                {#if taskContent[0]}
                    {#if state.id === taskContent[0].state_id}
                        <option selected value={state.id}>
                            {state.state}
                        </option>
                    {:else}
                        <option class={state.state.split(" ").join("")} value={state.id}>
                            {state.state}
                        </option>
                    {/if}
                {/if}
            {/each}
        </select>
    </div>

    <div id="buttonContainer">
        <DeleteButton taskDiv={taskDiv} taskId={taskContent[0].id}></DeleteButton>
    </div>
</div>

<style>
    option {
        text-align: center;
        padding: 1em;
    }
    .task {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 0.3em;
        border: 2px solid rgb(235, 138, 100, 0.863);
        border-radius: 15px;
        width: min-content;
        padding: 0.4em;
        margin-bottom: 0.3em;
    }
    .taskContent{
        width: 500px;
        border-radius: 10px;
        border: solid 1px rgb(204, 204, 204);
        background-color: #2b303a;
        transition: 0.4s;
        font-size: 20px;
    }
    .taskContent:hover, .taskContent:focus{
        border: 2px solid rgb(235, 138, 100, 0.863);
        background-color: #f58f58;
    }
    select:hover{
        border: 2px solid rgb(235, 138, 100, 0.863);
    }

    .taskContent, select{
        height: 35px;
        text-align: center;
    }
    select{
        border-radius: 5px;
        border: solid 1px rgb(204, 204, 204);
        background-color: #2b303a;
        transition: 0.4s;
    }
    option {
        text-align: center;
    }

    #buttonContainer{
        display: flex;
        gap: 1em;
    }

    @media screen and (max-width: 500px){
        .taskContent{
            width: 200px;
            border-radius: 10px;
            border: solid 1px rgb(204, 204, 204);
            background-color: #2b303a;
            transition: 0.4s;
            font-size: 20px;
            overflow: hidden;
        }
        
    }
</style>
