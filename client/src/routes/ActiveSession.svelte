<script>
    import {push} from "svelte-spa-router";
    
    </script>
    <div class="container">
        <h1>Harlog</h1>
        <h2 class="title">Aktiivne küsitlus</h2>
        <p>Toa kood:</p>
        <h2 class="boxtext">EXAMPLE</h2>

        <button on:click|preventDefault="{() => {push('/userpanel')}}">LÕPETA KÜSITLUS</button>
    </div>
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
    .boxtext{       
        width: 300px;
        border: 15px solid rgb(92, 0, 128);
        padding: 50px;
        margin: 20px;
    }
    </style>