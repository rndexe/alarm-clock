<script>
    import { fade } from "svelte/transition";

    export let time;
</script>

<div class="card" transition:fade>
    <span>{time}</span>
</div>

<style>
    .card {
        display: flex;
        justify-content: space-between;
        padding: 4px;
        background-color: #373435;
        color: #fafafa;
        font-size: 2em;
        border-radius: 4px;
        padding: 4px 12px;
    }
</style>
