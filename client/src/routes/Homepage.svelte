<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script>
//test    123
import {push} from "svelte-spa-router";
let roomExists = false;
let user = {
    name: "",
    password: ""
};
let quizParticipant = {

};

let activity = {

};

let roomKey = {
    roomkey : ""
};
async function login(){
    let haveErrors = false;
    if(user.name === ""){
        document.getElementById('error-user').innerHTML = "Sisesta kasutajatunnus!"
        haveErrors= true;

    }
    else{
        document.getElementById('error-user').innerHTML = ""
    }
    if (user.password === ""){
        document.getElementById('error-password').innerHTML = "Sisesta parool!"
        haveErrors= true;
    }
    else{
        document.getElementById('error-password').innerHTML = ""

    }
    if(haveErrors === false){
        const response = await fetch("http://localhost:3000/login", {
                method: 'POST', // *GET, POST, PUT, DELETE, etc.
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'same-origin', // include, *same-origin, omit
                headers: {
                'Content-Type': 'application/json'
                // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: 'follow', // manual, *follow, error
                referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(user) // body data type must match "Content-Type" header
            });
            if(response.ok){
                let token = await response.json();
                sessionStorage.setItem("token", `Bearer ${token.token}`);
                push("/userpanel");
            }
            if(response.status === 403){
                document.getElementById('error-login').innerHTML = "Vigane kasutajatunnus või parool!"
            }
            console.log(response);
    }
    haveErrors = false;            
}

async function enterQuiz(){
    let haveErrorsQuiz = false;
    console.log(quizParticipant);
    console.log(quizParticipant.part_email);

    if(typeof quizParticipant.part_name === 'undefined'){
        document.getElementById('error-part_name').innerHTML = "Sisesta nimi!"
        haveErrorsQuiz= true;

    }
    else{
        document.getElementById('error-part_name').innerHTML = ""
    }
    if(typeof quizParticipant.part_email === 'undefined'){
        document.getElementById('error-part_email').innerHTML = "Sisesta email!"
        haveErrorsQuiz= true;
    }
    else{
        const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
        if (re.test(quizParticipant.part_email) === false){
            haveErrorsQuiz = true;
            document.getElementById('error-part_email').innerHTML = "Pole korrektne meiliaadress!"
        }
        else{
            document.getElementById('error-part_email').innerHTML = "" 
        }  
    }
    
    if(haveErrorsQuiz === false){
        const response = await fetch("http://localhost:3000/enterquiz", {
                method: 'POST', // *GET, POST, PUT, DELETE, etc.
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'same-origin', // include, *same-origin, omit
                headers: {
                'Content-Type': 'application/json'
                // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: 'follow', // manual, *follow, error
                referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(quizParticipant) // body data type must match "Content-Type" header
            
            });
            
            if(response.ok){
                let token = await response.json();
                sessionStorage.setItem("token", `Bearer ${token.token}`);
                push("/questionpage");
            }
    }
    haveErrorsQuiz = false;
}
async function getRoomkey(){
    
    let haveErrorsRoom = false;
    if(roomKey.roomkey === ""|| roomKey.roomkey=== "undefined"){
        document.getElementById('error-room').innerHTML = "Sisesta ruumi võti!"
        haveErrorsRoom= true;
    }
    else{
        document.getElementById('error-room').innerHTML = ""
    }   
    if(haveErrorsRoom === false){
        const response = await fetch("http://localhost:3000/getroomkey", {
                method: 'POST', // *GET, POST, PUT, DELETE, etc.
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'same-origin', // include, *same-origin, omit
                headers: {
                'Content-Type': 'application/json'
                // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: 'follow', // manual, *follow, error
                referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(roomKey) // body data type must match "Content-Type" header
            });
            if(response.ok){
                roomExists = true;
                quizParticipant.activity_id = (await response.json()).activity_id;
            }
            if(response.status === 404){
                document.getElementById('error-room').innerHTML = "Vale ruumi võti!"
                roomExists = false;
            }
    }
    haveErrorsRoom = false;            
}







</script>
<table class="wrapper">
<div class="container">
    <div class="padding">
    <img src="../pictures\logo.png" alt="Harlog logo" onclick="window.location.href='http://localhost:5000'">   
{#if !roomExists}
    
    
    <div>
        <h2 class="title">Õpilane:</h2>
        <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Sisesta ruumivõti:</p>
        <form class="container">
        <input type="text" placeholder="Võti" bind:value="{roomKey.roomkey}"><span class="errormsg" id="error-room"></span>
        <button on:click|preventDefault={getRoomkey}>SISESTA</button>
        </form>
    </div>
    <div>
        <h2 class="title">Õpetaja:</h2>
        <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Logi sisse:</p>
        <form class="container">
        <input type="text" placeholder="Kasutajatunnus" bind:value="{user.name}"><span class="errormsg" id="error-user"></span>
        <input type="password" placeholder="Parool" bind:value="{user.password}"><span class="errormsg" id="error-password"></span><span class="errormsg" id="error-login"></span>
        <input type="submit" value="SISENE" on:click|preventDefault="{login}">   
        </form>
    </div>
    <input type="button" value="REGISTREERU" on:click|preventDefault="{() => {push('/registerpage')}}">
{:else}
<div>
    
    <h2 class="title">Sessioonis osalemine</h2>
    <form class="container">
    <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Sisesta nimi</p>
    <input type="text" placeholder="Nimi" bind:value="{quizParticipant.part_name}"><span class="errormsg" id="error-part_name"></span>
    <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Sisesta email</p>
    <input type="text" placeholder="Email" bind:value="{quizParticipant.part_email}"><span class="errormsg" id="error-part_email"></span>
    <input type="submit" value="SISENE" on:click|preventDefault="{enterQuiz}">
    <button on:click|preventDefault="{() => {roomExists=false}}">Pealehele</button>  
    </form>

</div>
{/if}
    </div>
</div>
</table>
<style>



</style>