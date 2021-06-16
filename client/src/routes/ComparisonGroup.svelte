<script>
    
    let activity1 = false;
    let activity2 = false;

    let participant1 = false;
    let participant2 = false;

    let selectedGroup1 = {};
    let selectedActivity1 = {};
    let selectedParticipant1 = {};

    let selectedGroup2 = {};
    let selectedActivity2 = {};
    let selectedParticipant2 = {};

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
        }};

        async function listActivities(){
        const response = await fetch("http://localhost:3000/listactivities", {
            method: 'POST', // *GET, POST, PUT, DELETE, etc.
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'same-origin', // include, *same-origin, omit
            headers: {
            'Content-Type': 'application/json',
            // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: 'follow', // manual, *follow, error
            referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(selectedGroup1)
        })
        if(response.ok){
            return await response.json();
            //push("/");
        }};

        async function listParticipants(){
        const response = await fetch("http://localhost:3000/listparticipants", {
            method: 'POST', // *GET, POST, PUT, DELETE, etc.
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'same-origin', // include, *same-origin, omit
            headers: {
            'Content-Type': 'application/json',
            // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: 'follow', // manual, *follow, error
            referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(selectedActivity1)
        });
               
        if(response.ok){
            return await response.json();
            //push("/");
        }};
    
</script>
<div class="container">
    <form>
        <label for="groups">Vali esimene grupp</label>
        <select bind:value="{selectedGroup1.group_id}" on:change="{() => {activity1 = true;}}">
            <option>Select group</option>
            {#await listGroup()}
            <p>Loading groups...</p>
            {:then groups}
            {#each groups as group}      
            <option value="{group.group_id}">{group.group_name}</option>
            {/each}
            {/await}                
        </select>
        {#if activity1}
        <select bind:value="{selectedActivity1.activity_id}" on:change="{() => {participant1 = true;}}">
            <option>Select activity</option>
            {#await listActivities()}
            <p>Loading activities...</p>
            {:then activities}
            {#each activities as activity}      
            <option value="{activity.activity_id}">{activity.activity_name}</option>
            {/each}
            {/await}                
        </select>    
        {/if}    
        {#if participant1}
        <select bind:value="{selectedParticipant1.activity_id}">
            <option>Select participant </option>
            {#await listParticipants()}
            <p>Loading participants...</p>
            {:then participants}
            {#each participants as participant}      
            <option>{participant.part_name}</option>
            {/each}
            {/await}                
        </select>
        {/if}
    </form>

    <form>
        <label for="groups">Vali teine grupp</label>
        <select bind:value="{selectedGroup2.group_id}" on:change="{() => {activity2 = true;}}">
            <option>Select group</option>
            {#await listGroup()}
            <p>Loading groups...</p>
            {:then groups}
            {#each groups as group}      
            <option value="{group.group_id}">{group.group_name}</option>
            {/each}
            {/await}                
        </select>
        {#if activity2}
        <select bind:value="{selectedActivity2.activity_id}" on:change="{() => {participant2 = true;}}">
            <option>Select activity</option>
            {#await listActivities()}
            <p>Loading activities...</p>
            {:then activities}
            {#each activities as activity}      
            <option value="{activity.activity_id}">{activity.activity_name}</option>
            {/each}
            {/await}                
        </select>    
        {/if}    
        {#if participant2}
        <select bind:value="{selectedParticipant2.activity_id}">
            <option>Select participant </option>
            {#await listParticipants()}
            <p>Loading participants...</p>
            {:then participants}
            {#each participants as participant}      
            <option>{participant.part_name}</option>
            {/each}
            {/await}                
        </select>
        {/if}
    </form>
</div>
<style>

</style>