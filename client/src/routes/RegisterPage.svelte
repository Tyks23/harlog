<script>
    import {push} from "svelte-spa-router";
    let user = {
     
    }
    let passwordVal;

    async function register(){
        console.log(user);
        console.log(JSON.stringify(user));
        const response = await fetch("http://localhost:3000/register", {
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
            push("/");
        }
    }
    </script>
    <div class="container">
        <h1>Harlog</h1>
        <h2 class="title">Registreeru</h2>

        <form class="container">
        <p>Sisesta kasutajatunnus:</p>
        <input type="text" placeholder="Kasutajatunnus" bind:value="{user.name}">
        <p>Sisesta email:</p>
        <input type="text" placeholder="Email" bind:value="{user.email}">
        <p>Sisesta parool:</p>
        <input type="password" placeholder="Parool" bind:value="{user.password}">
        <p>Sisesta uuesti parool:</p>
        <input type="password" placeholder="Parool" bind:value="{passwordVal}">
        <input type="submit" value="REGISTREERU" on:click|preventDefault="{register}">     
        </form>
    </div>
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
        
    </style>