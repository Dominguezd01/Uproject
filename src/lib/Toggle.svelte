

<label class="button" for="toggle">
    <input id="toggle" type="checkbox" />
    <span class="slider" id="slider" />
</label>


<style>
    :root{
        color-scheme: light;
        --orange: #ff4700;
    }

    .button {
        display: inline-block;
        width: 100px;
        height: 40px;
        background-color: #363636;
        border-radius: 30px;
        cursor: pointer;
        padding: 0;
        -webkit-box-shadow: 1px -6px 121px -3px rgba(255,71,0,0.57);
        -moz-box-shadow: 1px -6px 121px -3px rgba(255,71,0,0.57);
        box-shadow: 1px -6px 121px -3px rgba(255,71,0,0.57);
    }
    
    #toggle {
        display: none;
    }
    
    .slider {
        display: block;
        font-size: 20px;
        position: relative;

    }
    
    .slider::after {
        content: 'REG';
        color: white;
        width: 50px;
        height: 50px;
        background-color: var(--orange);
        border: 2px solid #fff;
        border-radius: 50%;
        box-shadow: 0 0 5px rgba(0, 0, 0, .25);
        position: absolute;
        top: -5px;
        left: 0;
        display: grid;
        place-content: center;
        line-height: 0;
        transition: background-color .25s, transform .25s ease-in;
    }
    
    #toggle:checked + .slider::after {
     content: 'LOG';
     background-color: var(--orange);
     transform: translateX(50px) rotate(360deg);
    }
    
    </style>