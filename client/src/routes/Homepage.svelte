
<script>
//test    
import {push} from "svelte-spa-router";
let roomExists = false;
let user = {
    name: "",
    password: ""
};
let quizParticipant = {

};
async function login(){
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
}

async function enterQuiz(){
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
</script>
<div class="container">
{#if !roomExists}
    <h1>Harlog</h1>
    <div>
        <h2 class="title">Küsitluses osalemine</h2>
        <p>Sisesta ruumivõti:</p>
        <form class="container">
        <input type="text" placeholder="Võti">
        <button on:click|preventDefault="{() =>{roomExists=true}}">SISESTA</button>
        </form>
    </div>
    <div>
        <h2 class="title">Küsitluste haldamine</h2>
        <p>Logi sisse</p>
        <form class="container">
        <input type="text" placeholder="Kasutajatunnus" bind:value="{user.name}">
        <input type="password" placeholder="Parool" bind:value="{user.password}">
        <input type="submit" value="SISENE" on:click|preventDefault="{login}">     
        </form>
    </div>
    <input type="button" value="Registreeru" on:click|preventDefault="{() => {push('/registerpage')}}">
{:else}
<div>
    <h1>Harlog</h1>
    <h1 class="title">Küsitluses osalemine</h1>
    <form class="container">
    <p>Sisesta nimi</p>
    <input type="text" placeholder="Nimi" bind:value="{quizParticipant.part_name}">
    <p>Sisesta email</p>
    <input type="text" placeholder="Email" bind:value="{quizParticipant.part_email}">
    <input type="submit" value="SISENE" on:click|preventDefault="{enterQuiz}">     
    </form>
</div>
{/if}
</div>


<style>
.container {  
    display: flex;
    flex-direction: column;
}
	
</style>