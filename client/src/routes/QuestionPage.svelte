<script>
    import {push} from "svelte-spa-router";
    
    </script>
    <div class="container">
        <h1>Harlog</h1>

        <div class="slidecontainer">
            <p>I pöörasin sellele tegevusele enda täieliku tähelepanu</p>
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange">
          </div>
          <div class="slidecontainer">
            <p>Ma ainult teesklesin, et osalesin selles tegevuses</p>
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange">
          </div>
          <div class="slidecontainer">
            <p>Mulle meeldis õppida uusi asju seoses selle tegevusega</p>
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange">
          </div>
          <div class="slidecontainer">
            <p>Tundsin ennast seoses tegevusega heitunult(???)</p>
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange">
          </div>
          <div class="slidecontainer">
            <p>Tegevus aitas mul seda teemat õppida</p>
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange">
          </div>
          <div class="slidecontainer">
            <p>See tegevus pani mind teemast uut moodi mõtlema</p>
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange">
          </div>

        <button on:click|preventDefault="{() => {push('/')}}">LÕPETA</button>
    </div>
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
    </style>