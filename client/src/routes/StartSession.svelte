<script>  
    import {push, replace} from "svelte-spa-router";
    

    let group = {
        group_name : undefined
    }
    let activity= {
        incognito : false
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
            let data = await response.json();
        let groupList = document.getElementsByClassName('groupList')[0];
           let appendable = document.createElement('li'); 
           let button = document.createElement('button');
           button.value = data.group_id;
           button.textContent = data.group_name;
           button.onclick = () => {createActivity(data.group_id)};    
           appendable.appendChild(button);
           groupList.appendChild(appendable);
            //push("/");
        }
    }
    
    async function createActivity(group_id){
        console.log(activity);
        activity.group_id = group_id;
        const response = await fetch("http://localhost:3000/createactivity", {
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
            body: JSON.stringify(activity) // body data type must match "Content-Type" header
        });
        if(response.ok){
           
            //push("/");
        }
    }

    async function listGroup(){
        const response = await fetch("http://localhost:3000/listgroup", {
            method: 'GET', // *GET, POST, PUT, DELETE, etc.
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
        });
        
       
        if(response.ok){
            return await response.json();
            //push("/");
        }
    };
    </script>
    <div class="container">
        <h1>Harlog</h1>
        <form>
            <h1 class="title">Alusta küsitlust:</h1>
            <p>Tegevuse nimi:</p>
            <input type="text" placeholder="Tegevuse nimi" bind:value="{activity.activity_name}">
        </form>
        <form>
            <p>Vali grupp millega tegevust alustada:</p>
            <ul class = "groupList">
                {#await listGroup()}
                <p>Loading groups...</p>
                {:then groups}
                {#each groups as group}
            
                <button on:click|preventDefault="{() => {createActivity(group.group_id)}}">{group.group_name}</button>
                {/each}
                {/await}                
            </ul>    
            <input type="text" bind:value="{group.group_name}"> 
                <button on:click|preventDefault="{createGroup}">LOO GRUPP</button>   
        
        <input type="checkbox" on:click="{() => {activity.incognito = !activity.incognito}}">
            
        </form>
    </div>
    
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
        
    }
    ul {
        list-style-type: none;
    }
        
    </style>