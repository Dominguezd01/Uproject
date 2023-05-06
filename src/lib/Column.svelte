<script>
    // @ts-nocheck

    
    import Task from "./Task.svelte"
    import Swal from "sweetalert2";
    import { API_ROUTE } from "./routes";
    export let columnInfo;
    export let states;
    console.log(columnInfo);
    let divContainer;
    let columnDiv;
    let columnTitle

    const getUserId = () =>{
        if (browser) {
            idUser = sessionStorage.getItem("userId")
            if(!idUser){
                location.href="/auth/login"
            }
        }
    }
    const getBoardId = () =>{
        if (browser) {

            boardId = sessionStorage.getItem("boarId")
            if(!boardId){
                location.href="/boards"
            }
            return boardId
        }
    }
    const getNewTask = async () => {
        Swal.fire({
            title: "Your new task",
            input: "text",
            inputAttributes: {
                autocapitalize: "off",
            },
            showCancelButton: true,
            confirmButtonText: "Add task!",
            showLoaderOnConfirm: true,
            preConfirm: async (content) => {
                if(!content){
                    content = " "
                }
                let options = {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                        content: content,
                        columnId: divContainer.dataset.id,
                        userId: getUserId(),
                        boardId: getBoardId()
                    }),
                };

                try {
                    let responseAddTask = await fetch(
                        `${API_ROUTE}/tasks/add`,
                        options
                    );
                    if (!responseAddTask.ok) {
                        throw new Error(responseAddTask.statusText);
                    }
                    responseAddTask = await responseAddTask.json();
                    console.log(responseAddTask);
                    if (responseAddTask.status == 200) {
                        new Task({
                            target: divContainer,
                            props: {
                                // @ts-ignore
                                taskContent: [
                                    {
                                        // @ts-ignore
                                        id: responseAddTask.task.id,
                                        // @ts-ignore
                                        content: responseAddTask.task.content,
                                        // @ts-ignore
                                        state_id: responseAddTask.task.state_id,
                                    },
                                ],

                                states: responseAddTask.states,
                            },
                        });
                    }
                } catch (error) {
                    Swal.showValidationMessage(`Request failed: ${error}`);
                }
            },
            allowOutsideClick: () => !Swal.isLoading(),
        });
    };

    const handleClickColumnDelete = async () => {
        Swal.fire({
            icon: "warning",
            title: "Are you sure you wanna delete this column?",
            text: "This would delete all the tasks inside and disapear forever",
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
                        columnId: divContainer.dataset.id,
                        userId: getUserId(),
                        boardId: getBoardId()
                    }),
                };
                try {
                    let responseDeleteColumn = await fetch(
                        `${API_ROUTE}/columns/delete`,
                        options
                    );
                    if (!responseDeleteColumn.ok) {
                        throw new Error(responseDeleteColumn.statusText);
                    }
                    responseDeleteColumn = await responseDeleteColumn.json();
                    console.log(responseDeleteColumn);
                    if (responseDeleteColumn.status == 200) {
                        Swal.fire({
                            title: "Column deleted",
                            text: "Its a bit sad ngl ;C",
                            icon: "success",
                            showConfirmButton: true,
                        });

                        columnDiv.remove();
                    }
                } catch (error) {
                    Swal.showValidationMessage(`Request failed: ${error}`);
                }
            },
            allowOutsideClick: () => !Swal.isLoading(),
        });
    };

    const editColumnTitle = async () => {
        console.log(columnTitle.value)
        const options = {
            method: "PUT",
            headers: {
                "Content-Type" : "application/json"
            },
            body: JSON.stringify({
                columnId: columnInfo.id,
                columnTitle: columnTitle.value,
                userId: getUserId(),
                boardId: getBoardId()
            })
        }
        let response = await fetch(`${API_ROUTE}/columns/editColumnTitle`, options)
        response = await response.json()
        console.log(response)
        if(response.state == 200){
            let previousStyle = columnTitle.style.border
            columnTitle.style.border = "1px solid green"
            setTimeout(() =>{
                columnTitle.style.border = previousStyle
            },1000)
        }else{
            let previousStyle = columnTitle.style.border
            columnTitle.style.border = "1px solid red"
            setTimeout(() =>{
                columnTitle.style.border = previousStyle
            },1000)
        }
    }
</script>

<main bind:this={columnDiv} class="main">
    <div class="columnTitleContainer">
        <input class="columnTitle" value={columnInfo.title} bind:this={columnTitle} on:blur={editColumnTitle} type="text">
    </div>
    <hr class="hr">
    <div class="buttonContainer">
        <button class="buttonAdd" on:click={getNewTask}> Add a task! </button>

        <button class="buttonDelete" on:click={handleClickColumnDelete}>
            Delete column
        </button>
    </div>

    <div
        bind:this={divContainer}
        id="taskContainer"
        data-id={columnInfo.id}
        class="taskContainer"
    >
        {#each columnInfo.tasks as task}
            <Task taskContent={task} {states} />
        {/each}
    </div>
</main>

<style>
    .main {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 0.2em;
        border: 1px solid rgb(235, 138, 100, 0.863);
        border-radius: 5px;
        padding: 0.3em;
        width: 900px;
    }

    .buttonContainer {
        width: 900px;
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        place-items: center;
        margin-bottom: 1em;
    }
    hr{
        width: 900px;
        opacity: 0.5;
        background-color: #ff682c;
        border: 1px solid #ff682c;
        margin-bottom: 1em;
    }
    .columnTitle{
        border: 0.4px #ff682c solid;
        font-size: 20px;
        margin-bottom: 1em;
        width: 450px;
        background-color: rgb(253, 140, 75);
        height: 35px;
        text-align: center;
        transition: 0.2s;
    }

    .buttonAdd {
        background-color: #ff682c;
        color: white;
        display: grid;
        place-items: center;
        text-align: center;
        width: 135px;
        padding: 0.2em;
        height: 25px;
        font-size: 15px;
        cursor: pointer;
        border: 0.4px rgba(250, 128, 114, 0.336) solid;
        border-radius: 5px;
        transition: 0.2s;
    }

    .buttonAdd:hover,
    .buttonDelete:hover {
        width: 95px;
        height: 40px;
        -webkit-box-shadow: 1px -25px 300px -40px #ff682c;
        -moz-box-shadow: 1px -25px 300px -40px #ff682c;
        box-shadow: 1px -25px 300px -40px #ff682c;
    }

    .buttonDelete {
        background-color: rgb(245, 27, 27);
        border-radius: 5px;
        border: 0.4px rgba(255, 32, 7, 0.336) solid;
        color: white;
        display: grid;
        place-items: center;
        text-align: center;
        width: 135px;
        padding: 0.2em;
        height: 25px;
        font-size: 15px;
        cursor: pointer;
        transition: 0.2s;
    }
    .columnTitle {
        margin-left: 0.4em;
        padding: 0.5em;
        text-align: center;
        margin-top: 0.2em;
        border-radius: 5px;
    }
    .taskContainer {
        display: flex;
        flex-direction: column;
        padding: 0.3em;
        width: min-content;
        justify-content: center;
    }
</style>
