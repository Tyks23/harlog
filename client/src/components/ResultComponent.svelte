<script>
    export let componentObject;

    async function getActivities(){
        const response = await fetch("http://localhost:3000/getactivities", {
            method: 'POST', // *GET, POST, PUT, DELETE, etc.
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'same-origin', // include, *same-origin, omit
            headers: {
            'Content-Type': 'application/json',
            //'Authorization' : sessionStorage.getItem('token')
            // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: 'follow', // manual, *follow, error
            referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(componentObject) // body data type must match "Content-Type" header
        });
        if(response.ok){
            let data = await response.json();
            console.log(data);
        }
    }

</script>
<p>{componentObject.group_name}</p>
<button on:click="{getActivities}"></button>

<style>

</style>