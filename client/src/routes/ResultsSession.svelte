<script>
import { push } from "svelte-spa-router";
if(sessionStorage.getItem("token") == null) {
           
    // Redirect to login page
    push("/");
}
function logout(){
    sessionStorage.removeItem("token");
    push("/");
}


let selectedGroup = {
    group_id : sessionStorage.getItem('group_id')
};
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
async function getActivities(){
    
}; 
</script>
<div class="container">
    
    <img src="../pictures\logo.png" alt="Harlog logo">
    <form class="container">
    <h2 class="title">Tulemuste esitamine:</h2>        
    
    <p style="padding-left: 10px; padding-right: 10px; text-align: center;">Vali grupp mille tulemusi soovid näha:</p>
    </form>
        <form>
            
            <ul class = "groupList">
                {#await listGroup()}
                <p>Loading groups...</p>
                {:then groups}
                {#each groups as group}
                <button on:click="{() => {sessionStorage.setItem('group_id', group.group_id), sessionStorage.setItem('group_name', group.group_name); push('/resultsgroup')}}">{group.group_name}</button> 
                              
                {/each}
                {/await}                
            </ul>             
        </form>
        <hr />
        <button on:click|preventDefault="{() => {push('/userpanel')}}">Tagasi</button>
        <button on:click|preventDefault="{logout}">Logi välja</button>
    </div>


<style>
ul {
       display: flex;
       margin: auto;
       
        flex-direction: column;
        list-style-type: none;
        padding-left: 0%;
        /*width: 90%; 
        list-style-type:none;
        margin:auto;
        padding:0;
        position:relative;
        left:5%;
        width: 100%;
        list-style: none;*/
    }
    hr {
        width: 360px;
        margin-left: auto;
        margin-right: auto;
    }    
</style>