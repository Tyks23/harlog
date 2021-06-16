<script>
import { group_outros } from "svelte/internal";


    
    let activity1 = false;
    let activity2 = false;

    let participant1 = false;
    let participant2 = false;

    let data;

    let selected1 = {
        group_id : null, 
        activity_id : null,
        part_id : null
    };


    let selected2 = {
        group_id : null, 
        activity_id : null,
        part_id : null
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
        }};

        async function listActivities(input){
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
            body: JSON.stringify(input)
        })
        if(response.ok){
            return await response.json();
            //push("/");
        }};

        async function listParticipants(input){
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
            body: JSON.stringify(input)
        });
               
        if(response.ok){
            return await response.json();
            //push("/");
        }};


    let activityEmotions = {
        emotion1: 0,
        emotion2: 0,
        emotion3: 0,
    };
    const getAvgOfCols = (
        array,
        start = 0,
        stop = 0,
        isReversed = undefined
    ) => {
        let counter = 0;
        const newArray = array.reduce((acc, cur) => {
            counter += stop + 1 - start;
            return [...acc, ...cur.slice(start, stop + 1)];
        }, []);
        if (isReversed != undefined)
            newArray.forEach((x, i) => {
                if (isReversed[i % isReversed.length]) {
                    newArray[i] = 6 - x;
                }
            });

        return newArray.reduce((acc, cur) => acc + cur, 0) / counter;
    };

    async function getComparisonData(input){

        const parsed = Object.entries(input).reduce((acc, [key, value]) => {
        if (value !== null) return { ...acc, [key]: value };
        return acc;
        }, {});

        const response = await fetch("http://localhost:3000/getcomparisondata", {
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
            body: JSON.stringify(parsed)
        });
        if (response.ok) {
            
           
    }
}
</script>
<div class="container">
    <form>
        <label for="groups">Vali esimene grupp</label>
        <!-- svelte-ignore a11y-no-onchange -->
        <select bind:value="{selected1.group_id}" on:change="{() => {activity1 = true;}}">
            <option value="{null}">Select group</option>
            {#await listGroup()}
            <p>Loading groups...</p>
            {:then groups}
            {#each groups as group}      
            <option value="{group.group_id}">{group.group_name}</option>
            {/each}
            {/await}                
        </select>
        {#if activity1}
        <!-- svelte-ignore a11y-no-onchange -->
        <select bind:value="{selected1.activity_id}" on:change="{() => {participant1 = true;}}">
            <option value="{null}">Select activity</option>
            {#await listActivities(selected1)}
            <p>Loading activities...</p>
            {:then activities}
            {#each activities as activity}      
            <option value="{activity.activity_id}">{activity.activity_name}</option>
            {/each}
            {/await}                
        </select>    
        {/if}    
        {#if participant1}
        <select bind:value="{selected1.part_id}">
            <option value="{null}">Select participant </option>
            {#await listParticipants(selected1)}
            <p>Loading participants...</p>
            {:then participants}
            {#each participants as participant}      
            <option value="{participant.part_id}">{participant.part_name}</option>
            {/each}
            {/await}                
        </select>
        {/if}
    </form>
    <button on:click="{() => {/*window.location.reload()*/console.log(selected1)}}" />
    <form>
        <label for="groups">Vali teine grupp</label>
        <!-- svelte-ignore a11y-no-onchange -->
        <select bind:value="{selected2.group_id}" on:change="{() => {activity2 = true;}}">
            <option value="{null}">Select group</option>
            {#await listGroup()}
            <p>Loading groups...</p>
            {:then groups}
            {#each groups as group}      
            <option value="{group.group_id}">{group.group_name}</option>
            {/each}
            {/await}                
        </select>
        {#if activity2}
        <!-- svelte-ignore a11y-no-onchange -->
        <select bind:value="{selected2.activity_id}"  on:change="{() => {participant2 = true;}}">
            <option value="{null}" disabled>Select activity</option>
            {#await listActivities(selected2)}
            <p>Loading activities...</p>
            {:then activities}
            {#each activities as activity}      
            <option value="{activity.activity_id}">{activity.activity_name}</option>
            {/each}
            {/await}                
        </select>    
        {/if}    
        {#if participant2}
        <select bind:value="{selected2.part_id}">
            <option value="{null}">Select participant </option>
            {#await listParticipants(selected2)}
            <p>Loading participants...</p>
            {:then participants}
            {#each participants as participant}      
            <option value="{participant.part_id}">{participant.part_name}</option>
            {/each}
            {/await}                
        </select>
        {/if}
    </form>
    <button on:click="{() => {getComparisonData(selected1)}}" />
</div>
<style>

</style>