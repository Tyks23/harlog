<script>  
    import {push} from "svelte-spa-router";

    let group = {
        group_name : undefined
    }
    async function createGroup(){
        const response = await fetch("http://localhost:3000/creategroup", {
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
            body: JSON.stringify(group) // body data type must match "Content-Type" header
        });
        if(response.ok){
            //push("/");
        }
    }
    </script>
    <div class="container">
        <h1>Harlog</h1>
        <form>
            <h1 class="title">Alusta küsitlust:</h1>
            <p>Tegevuse nimi:</p>
            <input type="text" placeholder="Tegevuse nimi">
        </form>
        <form>
            <p>Vali grupp:</p>
            <ul>
                <li><input type="text" bind:value="{group.group_name}"> </li>
                <li><button on:click|preventDefault="{createGroup}">LOO GRUPP</button></li>
            </ul>       
            <button on:click|preventDefault="{() => {push('/activesession')}}">ALUSTA KÜSITLUST</button>
        </form>
    </div>
    
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
        
    </style>