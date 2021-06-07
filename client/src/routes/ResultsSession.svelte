<script>

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
            
        </form>
        <form>
            <p>Vali grupp mille tulemusi soovid näha:</p>
            <ul class = "groupList">
                {#await listGroup()}
                <p>Loading groups...</p>
                {:then groups}
                {#each groups as group}
                <button>{group.group_name}</button>
                
                {/each}
                {/await}                
            </ul>             
        </form>
    </div>


<style>
.container {  
    display: flex;
    flex-direction: column;
}
    
</style>