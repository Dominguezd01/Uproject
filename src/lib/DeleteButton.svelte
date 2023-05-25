<script>
    export let taskId;
    export let taskDiv;
    import {API_ROUTE} from "./routes.js";
    import deleteIcon from '$lib/assets/delete.svg';
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


<img on:click={handleClick} src={deleteIcon} alt="deletIcon">

   


<style>
    img {
        width: 50px;
        cursor: pointer;
        display: flex;
        align-items: center;
        border: none;
        border-radius: 5px;
    }
    @media screen and (max-width: 500px){
        img{
            width: 50px;
            height: 20px;
        }
    }
</style>
