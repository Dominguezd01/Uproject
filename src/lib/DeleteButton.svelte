<script>
    export let taskId;
    export let taskDiv;
    import {API_ROUTE} from "./routes.js";
    import Swal from "sweetalert2";
    import { browser } from '$app/environment';
    const getUserId = () =>{
        if (browser) {
            let idUser = sessionStorage.getItem("userId")
            if(!idUser){
                location.href="/auth/login"
            }
            return idUser
        }
    }
    const getBoardId = () =>{
        if (browser) {
            let boardId = localStorage.getItem("boardId")
            if(!boardId){
                location.href="/boards"
            }
        
            return boardId
        }
    }
    const handleClick = async (e) => {
        Swal.fire({
            title: "You wanna delete this task?",
            inputAttributes: {
                autocapitalize: "off",
            },
            showCancelButton: true,
            confirmButtonText: "Yes, delete it!",
            showLoaderOnConfirm: true,
            preConfirm: async () => {
                const options = {
                    method: "DELETE",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                        taskId: taskId,
                        userId: getUserId(),
                        boardId: getBoardId()
                    }),
                };

                    let responseDeleteTask = await fetch(
                        `${API_ROUTE}/tasks/delete`,
                        options
                    );

                    responseDeleteTask = await responseDeleteTask.json();

                    if (responseDeleteTask.status == 200) {
                        Swal.fire(
                            // @ts-ignore
                            responseDeleteTask.message,
                            "Sometimes you have let it go :C",
                            "success"
                        )
                        taskDiv.remove();

                    }else if(responseDeleteTask.status == 401){
                        Swal.fire({
                            title: "Error",
                            text: `${responseDeleteTask.message}`,
                            icon: "error",
                            showConfirmButton: false,
                            timer: 1500
                        })

                        setTimeout(() =>{
                            location.href = "/boards"
                        }, 1500)
                    }else {
                        Swal.fire(
                            "ERROR!!",
                            // @ts-ignore
                            responseDeleteTask.message,
                            "error"
                        )
                        setTimeout(() =>{
                            location.reload()
                        }, 1500)
                    }
                },
        
            allowOutsideClick: () => !Swal.isLoading(),
        });
    };
</script>

<button on:click={handleClick} class="noselect"
    ><span class="text">Delete</span><span class="icon"
        ><svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
        >
            <path
                d="M24 20.188l-8.315-8.209 8.2-8.282-3.697-3.697-8.212 8.318-8.31-8.203-3.666 3.666 8.321 8.24-8.206 8.313 3.666 3.666 8.237-8.318 8.285 8.203z"
            />
        </svg></span
    >
</button>

<style>
    button {
        width: 150px;
        height: 20px;
        cursor: pointer;
        display: flex;
        align-items: center;
        background: red;
        border: none;
        border-radius: 5px;
        box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.15);
        background: #e62222;
    }

    button,
    button span {
        transition: 200ms;
    }

    button .text {
        transform: translateX(35px);
        color: white;
        font-weight: bold;
    }

    button .icon {
        position: absolute;
        transform: translateX(110px);
        height: 40px;
        width: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    button svg {
        width: 15px;
        fill: #eee;
    }

    button:hover {
        background: #ff3636;
    }

    button:hover .text {
        color: transparent;
    }

    button:hover .icon {
        width: 150px;
        border-left: none;
        transform: translateX(30px);
    }

    button:focus {
        outline: none;
    }

    button:active .icon svg {
        transform: scale(0.8);
    }
</style>
