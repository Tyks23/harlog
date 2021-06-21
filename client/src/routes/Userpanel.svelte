

<script>
    import {push} from "svelte-spa-router";
    
    if(sessionStorage.getItem("token") == null) {
           
           // Redirect to login page
           push("/");
       }
    function logout(){
        sessionStorage.removeItem("token");
        push("/");
    }
    </script>
    <div class="container">
        <img src="../pictures\logo.png" alt="Harlog logo">
        <h2 class="title">Sessiooni haldamine</h2>
        
        <button on:click|preventDefault="{() => {push('/startsession')}}">LOO UUS SESSIOON</button>
        <button on:click|preventDefault="{() => {push('/resultssession')}}">TULEMUSED</button>
        <button on:click|preventDefault="{() => {push('/comparisongroup')}}">VÕRDLUS</button>
        <button on:click|preventDefault="{logout}">Logi välja</button>
    </div>
    
    
    <style>

        
    </style>