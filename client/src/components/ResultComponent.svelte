<script>
    export let componentObject;


    let activityEmotions = {
        firstEmot : 0,
        secondEmot : 0,
        thirdEmot : 0
    };
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
            let counter = 0;
        data.array.forEach(row => {
            activityEmotions.firstEmot = row.answer[0] + (6 - row.answer[1]);
            activityEmotions.secondEmot = row.answer[2] + (6 - row.answer[3]);
            activityEmotions.thirdEmot = row.anser[4] + row.answer[5];
            counter += 2;
        });    
            activityEmotions.firstEmot /= counter;
            activityEmotions.secondEmot /= counter;
            activityEmotions.thirdEmot /= counter;
            console.log(activityEmotions);
        }
    }

</script>
<p>{componentObject.group_name}</p>
<button on:click="{getActivities}"></button>

<style>

</style>