<script>
import { group_outros } from "svelte/internal";


    let participants = {};    

    let activity1 = false;
    let participant1 = false;

    let data;

    let selected1 = {
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
            data = (await response.json()).rows;
            console.log(data);

            if(participant1 && activity1){
                participants = Array.from(
                new Set(data.map((item) => item.part_id))
            ).reduce(
                (acc, curr) => ({ ...acc, [curr]: { name: "", answers: [] } }),
                {}
            );
            data.forEach(({ part_id, answer, part_name }) => {
                //console.log(row);
                participants[part_id].answers.push(answer);
                participants[part_id].name = part_name;
            });

            let counter = 0;

            Object.entries(participants).forEach(([, value]) => {
                const { answers } = value;

                value.emotion1 = getAvgOfCols(answers, 0, 1, [false, true]);
                value.emotion2 = getAvgOfCols(answers, 2, 3, [false, true]);
                value.emotion3 = getAvgOfCols(answers, 4, 5);

                activityEmotions.emotion1 = value.emotion1;
            });
            }
            if(activity1 && !participant1){
                new Set(data.map((item) => item.activity_id))
            ).reduce(
                (acc, curr) => ({ ...acc, [curr]: { name: "", answers: [] } }),
                {}
            );
            data.forEach(({ activity_id, answer, activity_name }) => {
                activities[activity_id].answers.push(answer);
                activities[activity_id].name = activity_name;
            });

            let counter = 0;

            Object.entries(activities).forEach(([, value]) => {
                const { answers } = value;

                value.emotion1 = getAvgOfCols(answers, 0, 1, [false, true]);
                value.emotion2 = getAvgOfCols(answers, 2, 3, [false, true]);
                value.emotion3 = getAvgOfCols(answers, 4, 5);
            });
            }
            if(!activity1 && !participant1){
                
            }
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
    <button on:click="{() => {getComparisonData(selected1)}}" />
</div>
<div>
    <table id="column-example-5" class="charts-css column multiple data-spacing-3">
        <caption> Vordlused </caption>
        <tbody>
          <tr>
            <td style="--size:{activityEmotions.emotion1}"></td>
            <td style="--size:{activityEmotions.emotion2}"></td>
            <td style="--size:{activityEmotions.emotion3}"></td>
          </tr>
          <tr>
            
          </tr>
        </tbody>
      </table>
</div>
<style>
    #column-example-5 {
  height: 200px;
  max-width: 300px;
  margin: 0 auto;
}

</style>