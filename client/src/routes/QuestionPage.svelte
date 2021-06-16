<meta name="viewport" content="width=device-width, initial-scale=1.0">
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
    
    <div class="questioncontainer">
      <img src="../pictures\logo.png" alt="Harlog logo">
      <p style="padding-left: 10px; padding-right: 10px; text-align: left; ">Pühendusin ainult õppimisele</p>
      <div  class="slidecontainer">
        <input type="range" min="1" max="6" bind:value="{rangeArr.answer1}">
        
      </div>
      <div class="slidecontainer">
        
        <p class="leftmarking">Ei nõustu</p>
        <p class="rightmarking">Nõustun</p>
      </div>  
      <p style="padding-left: 10px; padding-right: 10px; text-align: left; ">Teesklesin osalemist</p>
      <div class="slidecontainer">
        <input type="range" min="1" max="6"  bind:value="{rangeArr.answer2}">
      </div>
      <div class="slidecontainer">
        
        <p class="leftmarking">Ei nõustu</p>
        <p class="rightmarking">Nõustun</p>
      </div>  
      <p style="padding-left: 10px; padding-right: 10px; text-align: left; ">Mulle meeldis õppida</p>
      <div class="slidecontainer">
        <input type="range" min="1" max="6"  bind:value="{rangeArr.answer3}">
      </div>
      <div class="slidecontainer">
        
        <p class="leftmarking">Ei nõustu</p>
        <p class="rightmarking">Nõustun</p>
      </div>  
      <p style="padding-left: 10px; padding-right: 10px; text-align: left; ">Selle tegevuse ajal kartsin pisut / olin segaduses</p>
      <div class="slidecontainer">
        
        <input type="range" min="1" max="6"  bind:value="{rangeArr.answer4}">
      </div>
      <div class="slidecontainer">
        
        <p class="leftmarking">Ei nõustu</p>
        <p class="rightmarking">Nõustun</p>
      </div>  
      <p style="padding-left: 10px; padding-right: 10px; text-align: left; ">Sain õpitust aru</p>
      <div class="slidecontainer">
        
        <input type="range" min="1" max="6"  bind:value="{rangeArr.answer5}">
      </div>
      <div class="slidecontainer">
        
        <p class="leftmarking">Ei nõustu</p>
        <p class="rightmarking">Nõustun</p>
      </div>  
      <p style="padding-left: 10px; padding-right: 10px; text-align: left; ">Sain midagi uut teada / harjutada</p>
      <div class="slidecontainer">
        
        <input type="range" min="1" max="6"  bind:value="{rangeArr.answer6}">
      </div>
      <div class="slidecontainer">
        
        <p class="leftmarking">Ei nõustu</p>
        <p class="rightmarking">Nõustun</p>
      </div>  
      
      <button style="margin-top: 10px;" on:click|preventDefault="{sendAnswer}">LÕPETA</button>
      
      
    </div>
    
    <style>
    /*span{
      font-family: 'Montserrat', sans-serif;
      color:#60495A;
      font-size: 75%;
      vertical-align: text-bottom;
    }*/
    .leftmarking{
      font-family: 'Montserrat', sans-serif;
      color:#60495A;
      font-size: 75%;
      
      text-align: left; 
      width:49%; 
      display: inline-block;
      margin-bottom: 10px;
	    margin-top: 5px;
    }
    .rightmarking{
      font-family: 'Montserrat', sans-serif;
      color:#60495A;
      font-size: 75%;
      text-align: right; 
      width:50%;  
      display: inline-block;
      margin-bottom: 10px;
	    margin-top: 5px;
      
      
    }
    </style>