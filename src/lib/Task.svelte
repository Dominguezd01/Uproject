<script>
    import { onMount } from "svelte";
    import SlimSelect from "slim-select";
    import DeleteButton from "./DeleteButton.svelte";
    import EditButton from "./EditButton.svelte";
    import { API_ROUTE } from "./routes";

    export let taskContent
    export let states

    console.log(states)
    let content = taskContent[0].content
    let state = taskContent[0].state
    let inputTask
    let select
    let taskDiv
    const getUserId = async () => {
        if (browser) {
            userId = sessionStorage.getItem("userId");
            if (!userId) {
                location.href = "/auth";
            }
            return userId;
        }
    }
    const getBoardId = () => {
        if (browser) {
            boardId = localStorage.getItem("boardId");
            if (!boardId) {
                location.href = "/auth";
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
        if(e.target.tagName == "INPUT"){
            document.getElementById(`taskContent${taskContent.id}`)
        }
        const options = {
            method: "PUT",
            headers: {
                "Content-Type": "Application/json"
            },
            body: JSON.stringify({
                taskId: taskContent[0].id,
                // @ts-ignore
                content: content,
                // @ts-ignore
                state: state, 
                userId: getUserId(),
                boardId: getBoardId()
            })   
        }
        try{
            let response = await fetch(`${API_ROUTE}/tasks/edit`, options)
            response = await response.json()
            console.log(response)
        }catch(err){
       
        }

    }

</script>

<div bind:this={taskDiv} class="task" id={taskContent.id} data-id={taskContent.id}>
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
            name="state" id={"selectState"+taskContent[0].id}>
            {#each states as state}
             
                {#if state.id === taskContent[0].state_id}
                    <option selected value={state.id}>
                        {state.state}
                    </option>
                {:else}
                    <option class={state.state.split(" ").join("")} value={state.id}>
                        {state.state}
                    </option>
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
        gap: 2em;
        border: 2px solid rgb(235, 138, 100, 0.863);
        border-radius: 15px;
        width: min-content;
        padding: 1em;
        margin-bottom: 0.3em;
    }

    .taskContent, select{
        height: 35px;
        text-align: center;
    }

    option {
        text-align: center;

    }


    #buttonContainer{
        display: flex;
        gap: 1em;
    }
</style>
