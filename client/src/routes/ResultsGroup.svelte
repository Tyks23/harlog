<script>

let selectedGroup = {
    group_id : sessionStorage.getItem('group_id')
};

let activities = {

};

let data;

let activityEmotions = {
    emotion1 : 0,
    emotion2 : 0,
    emotion3 : 0
};

    (async function getActivities(){
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
            body: JSON.stringify(selectedGroup) // body data type must match "Content-Type" header
        });
        if(response.ok){
            data = (await response.json()).rows;
            
            activities = Array.from(new Set(data.map((item) => item.activity_id))).reduce((acc, curr) => ({... acc, [curr]: {'name' : '', 'answers' : []}}), {});
            console.log(activities);
        data.forEach(row => {
            console.log(row);
            activities[row.activity_id].answers.push(row.answer);
            activities[row.activity_id].name = row.activity_name;
        
        });
            let counter = 0;
        data.forEach(row => {
            activityEmotions.emotion1 += row.answer[0] + (6 - row.answer[1]);
            activityEmotions.emotion2 += row.answer[2] + (6 - row.answer[3]);
            activityEmotions.emotion3 += row.answer[4] + row.answer[5];
            counter += 2;
        });    
            console.log(activityEmotions);
            activityEmotions.emotion1 /= counter;
            activityEmotions.emotion2 /= counter;
            activityEmotions.emotion3 /= counter;
            console.log(activityEmotions);
        }
    })();

</script>
<div class='container'>
    <h1>Harlog</h1>
    
    
    
</div>


<style>
.container {  
    display: flex;
    flex-direction: column;
}
</style>