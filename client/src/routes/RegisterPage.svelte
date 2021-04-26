<script>
    import {push} from "svelte-spa-router";
    
    </script>
    <div class="container">
        <h2 class="title">Registreeru</h2>

        <form class="container">
        <p>Sisesta kasutajatunnus:</p>
        <input type="text" placeholder="Kasutajatunnus">
        <p>Sisesta email:</p>
        <input type="text" placeholder="Email">
        <p>Sisesta parool:</p>
        <input type="password" placeholder="Parool">
        <p>Sisesta uuesti parool:</p>
        <input type="password" placeholder="Parool">
        <input type="submit" value="REGISTREERU" on:click|preventDefault="{() => {push('/')}}">     
        </form>
    </div>
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
        
    </style>