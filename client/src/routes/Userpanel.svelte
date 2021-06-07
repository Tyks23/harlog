<script>
    import {push} from "svelte-spa-router";
import { bubble } from "svelte/internal";
    
    </script>
    <div class="container">
        <h1>Harlog</h1>
        <h1 class="title">Küsitluste haldamine</h1>
        <button on:click|preventDefault="{() => {push('/startsession')}}">LOO UUS KÜSITLUS</button>
        <button on:click|preventDefault="{() => {push('/resultssession')}}">TULEMUSED</button>
        <button>LOGI VÄLJA</button>
    </div>
    
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
        
    </style>