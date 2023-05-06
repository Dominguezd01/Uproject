<div class="spinner-3" />

<style>
    .spinner-3 {
        position: absolute;
        top: 50%;
        width: 50px;
        padding: 8px;
        aspect-ratio: 1;
        border-radius: 50%;
        background: #eb8a6479;
        --_m: conic-gradient(#0000 10%, #000),
            linear-gradient(#000 0 0) content-box;
        -webkit-mask: var(--_m);
        mask: var(--_m);
        -webkit-mask-composite: source-out;
        mask-composite: subtract;
        animation: s3 1s infinite linear;
    }
    @keyframes s3 {
        to {
            transform: rotate(1turn);
        }
    }
</style>
