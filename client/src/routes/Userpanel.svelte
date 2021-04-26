<script>
    import {push} from "svelte-spa-router";
    
    </script>
    <div class="container">
        <h1 class="title">Küsitluses osalemine</h1>
        <button>MINU KÜSITLUSED</button>
        <button on:click|preventDefault="{() => {push('/startsession')}}">LOO UUS KÜSITLUS</button>
        <button>TULEMUSED</button>
        <button>KÜSITLUSE LÄBIVIIMINE</button>
    </div>
    
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
        
    </style>