<script>
    import { group_outros } from "svelte/internal";
    import { writable } from "svelte/store";

    let participants = {};

    let activity1 = false;
    let participant1 = false;

    let activities = {};

    let data;

    let selected1 = {
        group_id: null,
        activity_id: null,
        part_id: null,
    };

    const graphs = [];

    async function listGroup() {
        const response = await fetch("http://localhost:3000/listgroup", {
            method: "GET", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                Authorization: sessionStorage.getItem("token"),
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
        });

        if (response.ok) {
            return await response.json();
            //push("/");
        }
    }

    async function listActivities(input) {
        const response = await fetch("http://localhost:3000/listactivities", {
            method: "POST", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(input),
        });
        if (response.ok) {
            return await response.json();
            //push("/");
        }
    }

    async function listParticipants(input) {
        const response = await fetch("http://localhost:3000/listparticipants", {
            method: "POST", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(input),
        });

        if (response.ok) {
            return await response.json();
            //push("/");
        }
    }

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
        const newArray = array.slice(start, stop + 1);
        if (isReversed != undefined)
            newArray.forEach((x, i) => {
                if (isReversed[i % isReversed.length]) {
                    newArray[i] = 6 - x;
                }
            });

        return newArray.reduce((acc, cur) => acc + cur, 0) / newArray.length;
    };

    async function getComparisonData(input) {
        const parsed = Object.entries(input).reduce((acc, [key, value]) => {
            if (value !== null) return { ...acc, [key]: value };
            return acc;
        }, {});

        const response = await fetch(
            "http://localhost:3000/getcomparisondata",
            {
                method: "POST", // *GET, POST, PUT, DELETE, etc.
                mode: "cors", // no-cors, *cors, same-origin
                cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
                credentials: "same-origin", // include, *same-origin, omit
                headers: {
                    "Content-Type": "application/json",
                    // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: "follow", // manual, *follow, error
                referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(parsed),
            }
        );
        if (response.ok) {
            data = (await response.json()).rows;
            console.log(data);
            for (let i of data) {
                activityEmotions.emotion1 = i.emotion1 = getAvgOfCols(
                    i.answer,
                    0,
                    1,
                    [false, true]
                );
                activityEmotions.emotion2 = i.emotion2 = getAvgOfCols(
                    i.answer,
                    2,
                    3,
                    [false, true]
                );
                activityEmotions.emotion3 = i.emotion3 = getAvgOfCols(
                    i.answer,
                    4,
                    5
                );
            }
                let chartList = document.getElementById('charts');
                let appendable = document.createElement('tr'); 
                
                for (let i = 0; i < 3; i++) { 
                    let appendable2 = document.createElement('td');
                    let numb = activityEmotions[`emotion${i+1}`];
                    appendable2.setAttribute('style', `--size:${numb}`);
                    appendable2.textContent = activityEmotions[`emotion${i+1}`];    
                    appendable.appendChild(appendable2);
                    console.log(appendable2);
                    
                }
                           
                chartList.appendChild(appendable);
                   
        }
    }
</script>

<div class="container">
    <form>
        <label for="groups">Vali esimene grupp</label>
        <!-- svelte-ignore a11y-no-onchange -->
        <select
            bind:value={selected1.group_id}
            on:change={() => {
                activity1 = true;
            }}
        >
            <option value={null}>Select group</option>
            {#await listGroup()}
                <p>Loading groups...</p>
            {:then groups}
                {#each groups as group}
                    <option value={group.group_id}>{group.group_name}</option>
                {/each}
            {/await}
        </select>
        {#if activity1}
            <!-- svelte-ignore a11y-no-onchange -->
            <select
                bind:value={selected1.activity_id}
                on:change={() => {
                    participant1 = true;
                }}
            >
                <option value={null}>Select activity</option>
                {#await listActivities(selected1)}
                    <p>Loading activities...</p>
                {:then activities}
                    {#each activities as activity}
                        <option value={activity.activity_id}
                            >{activity.activity_name}</option
                        >
                    {/each}
                {/await}
            </select>
        {/if}
        {#if participant1}
            <select bind:value={selected1.part_id}>
                <option value={null}>Select participant </option>
                {#await listParticipants(selected1)}
                    <p>Loading participants...</p>
                {:then participants}
                    {#each participants as participant}
                        <option value={participant.part_id}
                            >{participant.part_name}</option
                        >
                    {/each}
                {/await}
            </select>
        {/if}
    </form>
    <button
        on:click={() => {
            window.location.reload();
        }}
    />
    <button
        on:click={() => {
            getComparisonData(selected1);
        }}
    />

    <div>
        <table
            id="column-example-5"
            class="charts-css column multiple data-spacing-3"
        >
            <caption> Column Example #5 </caption>
            <tbody id="charts">
            
              
            </tbody>
        </table>
    </div>
</div>

<style>
    #column-example-5 {
        height: 40px;
        max-width: 300px;
        margin: 0 auto;
    }
</style>
