<script>
    import {push} from "svelte-spa-router";

    let rangeArr = {
      answer1 : 1, 
      answer2 : 1,
      answer3 : 1,
      answer4 : 1,
      answer5 : 1,
      answer6 : 1
    };
    async function sendAnswer(){
        console.log(rangeArr);
        console.log(Array.from(rangeArr));
        const response = await fetch("http://localhost:3000/sendanswer", {
            method: 'POST', // *GET, POST, PUT, DELETE, etc.
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'same-origin', // include, *same-origin, omit
            headers: {
            'Content-Type': 'application/json',
            'Authorization' : sessionStorage.getItem('token')
            // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: 'follow', // manual, *follow, error
            referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(rangeArr) // body data type must match "Content-Type" header
        });
        if(response.ok){
            push("/");
        }

    }
    
    </script>
    <div class="container">
        <h1>Harlog</h1>

        <div class="slidecontainer">
            <p>I pöörasin sellele tegevusele enda täieliku tähelepanu</p>
            <input type="range" min="1" max="6" bind:value="{rangeArr.answer1}">
          </div>
          <div class="slidecontainer">
            <p>Ma ainult teesklesin, et osalesin selles tegevuses</p>
            <input type="range" min="1" max="6" bind:value="{rangeArr.answer2}">
          </div>
          <div class="slidecontainer">
            <p>Mulle meeldis õppida uusi asju seoses selle tegevusega</p>
            <input type="range" min="1" max="6" bind:value="{rangeArr.answer3}">
          </div>
          <div class="slidecontainer">
            <p>Tundsin ennast seoses tegevusega heitunult(???)</p>
            <input type="range" min="1" max="6" bind:value="{rangeArr.answer4}">
          </div>
          <div class="slidecontainer">
            <p>Tegevus aitas mul seda teemat õppida</p>
            <input type="range" min="1" max="6" bind:value="{rangeArr.answer5}">
          </div>
          <div class="slidecontainer">
            <p>See tegevus pani mind teemast uut moodi mõtlema</p>
            <input type="range" min="1" max="6" bind:value="{rangeArr.answer6}">
          </div>

        <button on:click|preventDefault="{sendAnswer}">LÕPETA</button>
    </div>
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
    </style>