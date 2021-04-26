<script>
    import {push} from "svelte-spa-router";
    
    </script>
    <div class="container">
        <h1 class="title">Küsitluste haldamine</h1>
        <button>MINU KÜSITLUSED</button>
        <button on:click|preventDefault="{() => {push('/startsession')}}">LOO UUS KÜSITLUS</button>
        <button on:click|preventDefault="{() => {push('/resultssession')}}">TULEMUSED</button>
        <button>KÜSITLUSE LÄBIVIIMINE</button>
    </div>
    
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
        
    </style>