<script>
    export let taskState

    const changeState = () =>{
        taskState = "text" ? taskState = "read" : taskState = "text"
    }
</script>

<button class="noselect" id="editButton">
    <span class="text">Edit</span>
    <span class="icon" >
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="#000000"
    ><g id="SVGRepo_bgCarrier" stroke-width="0" /><g
        id="SVGRepo_tracerCarrier"
        stroke-linecap="round"
        stroke-linejoin="round"
    /><g id="SVGRepo_iconCarrier">
        <title />
        <g id="Complete">
            <g id="edit">
                <g>
                    <path
                        d="M20,16v4a2,2,0,0,1-2,2H4a2,2,0,0,1-2-2V6A2,2,0,0,1,4,4H8"
                        fill="none"
                        stroke="#ffffff"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                    />
                    <polygon
                        fill="none"
                        points="12.5 15.8 22 6.2 17.8 2 8.3 11.5 8 16 12.5 15.8"
                        stroke="#ffffff"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                    />
                </g>
            </g>
        </g>
    </g></svg
>

    </span>
</button>


<style>
    button {
        width: 150px;
        height: 50px;
        cursor: pointer;
        display: flex;
        align-items: center;
        background: rgb(136, 255, 0);
        border: none;
        border-radius: 5px;
        box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.15);
        background: rgb(136, 255, 0);;
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
        border-left: 1px solid rgb(136, 255, 0);;
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
        background: rgb(136, 255, 0);
    }

    button:hover .text {
        color: transparent;
    }

    button:hover .icon {
        width: 150px;
        border-left: none;
        transform: translateX(0);
    }

    button:focus {
        outline: none;
    }

    button:active .icon svg {
        transform: scale(0.8);
    }
</style>
